# Ransom Note

**LeetCode 383 · Ransom Note**  
**难度：Easy | 方法：HashMap / Counter**

---

## 思路

用 Counter 统计 magazine 里每个字母的数量，然后遍历 ransomNote，每用一个字母就减一，减到 0 就不够用了。

---

## 代码

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 如果 ransomNote 比 magazine 长，字母肯定不够用，直接返回 False
        if len(ransomNote) > len(magazine):
            return False

        # 用 Counter 统计 magazine 里每个字母出现了几次
        letters = collections.Counter(magazine)

        # 遍历 ransomNote 里的每个字母
        for char in ransomNote:
            # 如果这个字母不在 magazine 里，或者已经用完了，返回 False
            if char not in letters or letters[char] <= 0:
                return False
            # 用掉一个这个字母，数量减一
            letters[char] -= 1

        # 所有字母都能找到，返回 True
        return True
```

---

## 步骤

1. 如果 ransomNote 比 magazine 长，字母肯定不够用，直接返回 False
2. 用 Counter 统计 magazine 里每个字母出现了几次
3. 遍历 ransomNote 里的每个字母
4. 如果这个字母不在 magazine 里，或者已经用完了，返回 False
5. 用掉一个这个字母，数量减一
6. 所有字母都能找到，返回 True

---

## 关键点

- `letters = Counter(magazine)` → 统计每个字母的数量，比如 `{'a': 2, 'b': 1}`
- `letters[char]` → 查这个字母还剩几个可以用
- `char not in letters` → 这个字母在 magazine 里根本没有
- `letters[char] <= 0` → 这个字母有，但已经用完了
- `letters[char] -= 1` → 用掉一个，库存减一
