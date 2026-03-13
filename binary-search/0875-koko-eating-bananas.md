# 875. Koko Eating Bananas

**Link:** https://leetcode.com/problems/koko-eating-bananas/
**Tags:** `Binary Search`
**Difficulty:** Medium

---

## Key Idea
> Binary search over eating speed. For speed `mid`, check if Koko can finish within `h` hours. Find the **minimum** valid speed.

## Notes
- Speed range: `[1, max(piles)]`
- For each pile, hours needed = `ceil(pile / speed)`
- Use `right = mid` (not `mid - 1`) — `mid` itself may be the answer

## Common Mistakes
- Using `right = mid - 1` → skips the correct answer
- Forgetting ceiling division — integer division rounds down, need `math.ceil`

## Solution
```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        left, right = 1, max(piles)  # 速度范围

        while left < right:
            mid = (left + right) // 2  # 猜测当前速度
            hours = sum(math.ceil(p / mid) for p in piles)  # 按此速度需要多少小时
            if hours <= h:
                right = mid       # 可行，但尝试更小的速度
            else:
                left = mid + 1    # 太慢，需要更快

        return left
```

## Binary Search Pattern Reference

| Goal | `right` update | `left` update |
|------|---------------|---------------|
| Find **minimum** valid value | `right = mid` | `left = mid + 1` |
| Find **maximum** valid value | `right = mid - 1` | `left = mid` |

## Complexity
- Time: `O(n log m)` where m = `max(piles)`
- Space: `O(1)`
