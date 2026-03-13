# 12. Integer to Roman

**Link:** https://leetcode.com/problems/integer-to-roman/
**Tags:** `Math` `Greedy`
**Difficulty:** Medium

---

## Key Idea
> Greedily subtract the largest possible Roman numeral value at each step.

## Notes
- Store all value-symbol pairs in **descending** order
- Each iteration: while `num >= val`, append `sym` and subtract `val`
- Must include subtractive forms: `IV (4)`, `IX (9)`, `XL (40)`, `XC (90)`, `CD (400)`, `CM (900)`

## Common Mistakes
- Forgetting subtractive forms → wrong output for 4, 9, 40, 90, 400, 900
- Not listing pairs in descending order → greedy won't work correctly

## Solution
```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # 降序排列，包含所有减法形式
        vals = [
            (1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
            (100, "C"),  (90, "XC"),  (50, "L"),  (40, "XL"),
            (10, "X"),   (9, "IX"),   (5, "V"),   (4, "IV"),
            (1, "I")
        ]

        result = ""
        for val, sym in vals:
            while num >= val:   # 贪心：能减就减
                result += sym
                num -= val
        return result
```

## Complexity
- Time: `O(1)` (max value is 3999, bounded)
- Space: `O(1)`
