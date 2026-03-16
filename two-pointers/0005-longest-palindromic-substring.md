# Longest Palindromic Substring

**LeetCode 5 · Longest Palindromic Substring**  
**难度：Medium | 方法：中心扩展法**

---

## 思路

回文串是对称的，对称就有中心，从中心往外扩展最自然：
- 枚举每个字符作为中心
- 左右指针往两边扩展
- 两边字符相等就继续扩，不等就停
- 记录最长的那个

**口诀：以每个字符为中心，左右往外扩，两边相等就继续，不等就停。奇偶各扩一次。**

---

## 代码

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        res = ""      # 存最长回文串，初始为空
        resLen = 0    # 存最长回文串的长度，初始为0

        for i in range(len(s)):  # 枚举每个字符作为回文串的中心

            # 奇数长度：中心是一个字符，左右指针都从 i 出发
            l, r = i, i
            while l >= 0 and r < len(s) and s[r] == s[l]:  # 没越界 且 两边相等
                if r - l + 1 > resLen:   # 当前回文比历史最长还长 → 更新
                    res = s[l:r+1]       # 更新最长回文串（切片左闭右开，所以 r+1）
                    resLen = r - l + 1   # 更新最长长度
                l -= 1  # 左指针往左移
                r += 1  # 右指针往右移

            # 偶数长度：中心是两个字符，左指针从 i，右指针从 i+1 出发
            l, r = i, i + 1
            while l >= 0 and r < len(s) and s[r] == s[l]:  # 没越界 且 两边相等
                if r - l + 1 > resLen:   # 当前回文比历史最长还长 → 更新
                    res = s[l:r+1]       # 更新最长回文串
                    resLen = r - l + 1   # 更新最长长度
                l -= 1  # 左指针往左移
                r += 1  # 右指针往右移

        return res
```

---

## 步骤

1. 初始化 `res` 和 `resLen`
2. `for` 循环枚举每个字符作为中心
3. 奇数回文：检查边界和两侧是否相等，如果长度大于最长长度，更新 `res` 和 `resLen`，然后左指针往左，右指针往右
4. 偶数回文：重复同样逻辑
5. 返回 `res`

---

## 例子演示

`s = "babad"`

```
以 i=1 的'a'为中心（奇数）：
  l=1, r=1   s[l]='a', s[r]='a'  相等 → "a"
  l=0, r=2   s[l]='b', s[r]='b'  相等 → "bab"  ✅ 更新 resLen=3
  l=-1 越界 → 停止

以 i=2 的'b'为中心（奇数）：
  l=2, r=2   s[l]='b', s[r]='b'  相等 → "b"
  l=1, r=3   s[l]='a', s[r]='a'  相等 → "aba"  长度=3，不大于3，不更新
  l=0, r=4   s[l]='b', s[r]='d'  不相等 → 停止

答案："bab"
```

---

## 关键点

- `while` 的三个条件：`l >= 0`（左没越界）、`r < len(s)`（右没越界）、`s[l] == s[r]`（两边相等）
- `s[l:r+1]`：Python 切片右边取不到，所以要 `r+1` 才能包含 index `r` 的字符
- `r - l + 1`：当前窗口长度，`+1` 是因为下标从 0 开始
- 奇偶都要扩，不然会漏掉偶数长度的回文（如 `"bb"`）
