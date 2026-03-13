# 3. Longest Substring Without Repeating Characters

**Link:** https://leetcode.com/problems/longest-substring-without-repeating-characters/
**Tags:** `Sliding Window`
**Difficulty:** Medium

---

## Key Idea
> Use a sliding window with a set. Expand right, shrink left whenever a duplicate appears.

## Notes
- `left` pointer shrinks the window when a duplicate is found
- `set` tracks characters in the current window
- Update `res = max(res, right - left + 1)` at every step

## Common Mistakes
- Forgetting to remove `s[left]` from the set before moving `left` forward
- Off-by-one: window size is `right - left + 1`, not `right - left`

## Solution
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()     # 当前窗口内的字符
        left = 0
        res = 0

        for right in range(len(s)):
            while s[right] in charSet:  # 有重复，收缩左边
                charSet.remove(s[left])
                left += 1
            charSet.add(s[right])       # 加入当前字符
            res = max(res, right - left + 1)

        return res
```

## Complexity
- Time: `O(n)`
- Space: `O(min(n, m))` where m = charset size
