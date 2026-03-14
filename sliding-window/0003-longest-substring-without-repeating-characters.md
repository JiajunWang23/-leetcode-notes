# Longest Substring Without Repeating Characters

**LeetCode 3 · Longest Substring Without Repeating Characters**  
**难度：Medium | 方法：滑动窗口 Sliding Window**

Link: https://leetcode.com/problems/longest-substring-without-repeating-characters/?envType=problem-list-v2&envId=rab78cw1

---

## 思路

用左右两个指针维护一个**无重复字符的滑动窗口**：
- 右指针不断扩展窗口
- 遇到重复字符就用左指针收缩，直到没有重复为止
- 全程记录窗口的最大长度

---

## 代码

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()  # 记录当前窗口有哪些字符，不允许重复，查找速度快
        left = 0         # 左指针，当前窗口的左边界
        res = 0          # 历史最大窗口长度

        for r in range(len(s)):           # 右指针从0开始扫描，每次扩展窗口右边界
            while s[r] in charSet:        # 右指针字符已在集合里 → 窗口内有重复
                charSet.remove(s[left])   # 把左边界字符从集合里删掉
                left += 1                 # 左指针右移一格，缩小窗口
            charSet.add(s[r])             # 窗口内无重复后，把右指针字符加入集合
            res = max(res, r - left + 1)  # 更新最大长度（r-left+1 是当前窗口长度）

        return res
```

---

## 逐行解释

| 代码 | 说明 |
|------|------|
| `charSet = set()` | 空集合，记录当前窗口里有哪些字符 |
| `left = 0` | 左指针，初始指向第 0 位 |
| `res = 0` | 存历史最大值，初始为 0 |
| `for r in range(len(s))` | 右指针从左到右扫描整个字符串 |
| `while s[r] in charSet` | 右指针字符已在集合里 → 有重复 → 进入收缩 |
| `charSet.remove(s[left])` | `s` 是字符串，`s[left]` 是左指针位置的字符，把它从集合删掉 |
| `left += 1` | 左指针右移，窗口左边界缩小一格 |
| `charSet.add(s[r])` | 无重复后，把右指针字符登记进集合 |
| `res = max(res, r - left + 1)` | 当前窗口长度 vs 历史最大，取大的保留 |
| `return res` | 返回最终结果 |

---

## 例子演示

`s = "abcab"`

```
r=0  s[r]='a'  charSet={}        → 加入 → {'a'}          res=1
r=1  s[r]='b'  charSet={'a'}     → 加入 → {'a','b'}       res=2
r=2  s[r]='c'  charSet={'a','b'} → 加入 → {'a','b','c'}   res=3
r=3  s[r]='a'  'a' 已在集合里！
               删掉 s[0]='a'，left=1，charSet={'b','c'}
               → 加入 'a' → {'b','c','a'}                  res=3
r=4  s[r]='b'  'b' 已在集合里！
               删掉 s[1]='b'，left=2，charSet={'c','a'}
               → 加入 'b' → {'c','a','b'}                  res=3

答案：3（"abc" 或 "bca" 或 "cab"）
```

---

## 关键点

- 集合里存的是**字符**，不是下标，所以用 `s[left]` 而不是 `left`
- 用 `while` 而不是 `if`，是因为可能需要**连续删多次**才能消除重复
- `r - left + 1` 要 `+1` 是因为下标从 0 开始
- 删掉的是**左边旧的字符**，加入的是**右边新的字符**，两者虽然值相同但位置不同
