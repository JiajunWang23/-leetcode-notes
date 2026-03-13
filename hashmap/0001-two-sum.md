# 1. Two Sum

**Link:** https://leetcode.com/problems/two-sum/
**Tags:** `HashMap`
**Difficulty:** Easy

---

## Key Idea
> Walk through the array once. At each step, ask: "Have I already seen the number I need?"

## Notes
- Use HashMap to store `value -> index`
- `enumerate` is required because the answer is **indices**, not values
- Compute `diff = target - n`, check if `diff` is already in the map

## Common Mistakes
- Forgetting `enumerate` → can't return indices
- Putting `prevMap[n] = i` **before** the `if` check → might pair element with itself

## Solution
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index，存已见过的数字和下标

        for i, n in enumerate(nums):
            diff = target - n       # 需要找的配对数
            if diff in prevMap:     # 之前见过吗？
                return [prevMap[diff], i]
            prevMap[n] = i          # 没找到，存入 map
```

## Line-by-line Breakdown

| Line | Purpose |
|------|---------|
| `prevMap = {}` | Empty HashMap: `{value: index}` |
| `for i, n in enumerate(nums)` | `i` = index, `n` = current value |
| `diff = target - n` | The number we need to complete the pair |
| `if diff in prevMap` | Have we seen this number before? |
| `return [prevMap[diff], i]` | Found a pair — return both indices |
| `prevMap[n] = i` | Not found yet — store for future lookups |

## Complexity
- Time: `O(n)`
- Space: `O(n)`
