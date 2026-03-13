# 1482. Minimum Number of Days to Make m Bouquets

**Link:** https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/
**Tags:** `Binary Search`
**Difficulty:** Medium

---

## Key Idea
> Binary search over days. For a given day `mid`, count how many bouquets can be made from consecutive bloomed flowers. Find the **minimum** day that produces at least `m` bouquets.

## Notes
- A flower at index `i` blooms on day `bloomDay[i]`; usable if `bloomDay[i] <= mid`
- Need `k` **consecutive** bloomed flowers per bouquet
- Scan left to right counting consecutive bloomed flowers; every `k` in a row = 1 bouquet
- Early exit: if `m * k > len(bloomDay)`, return `-1` immediately

## Common Mistakes
- Forgetting to reset `consecutive = 0` when a flower hasn't bloomed yet
- Not handling the impossible case `m * k > n` → would loop forever

## Solution
```python
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        if m * k > len(bloomDay):   # 不够花，直接返回 -1
            return -1

        left, right = 1, max(bloomDay)

        while left < right:
            mid = (left + right) // 2
            # 验证：第 mid 天能凑够 m 束吗？
            bouquets, consecutive = 0, 0
            for bloom in bloomDay:
                if bloom <= mid:        # 这朵花已经开了
                    consecutive += 1
                else:
                    consecutive = 0     # 不连续，重置
                if consecutive == k:    # 凑够一束
                    bouquets += 1
                    consecutive = 0
            if bouquets >= m:
                right = mid             # 可行，尝试更早
            else:
                left = mid + 1          # 不够，需要更多天

        return left
```

## Comparison with Koko (875)

| | Koko (875) | Bouquets (1482) |
|---|---|---|
| Binary search on | eating speed | days |
| Condition check | `sum(ceil(p/mid)) <= h` | consecutive bloom count `>= m` |
| Answer | minimum speed | minimum days |

## Complexity
- Time: `O(n log d)` where d = `max(bloomDay)`
- Space: `O(1)`
