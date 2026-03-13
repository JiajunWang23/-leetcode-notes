# 994. Rotting Oranges

**Link:** https://leetcode.com/problems/rotting-oranges/
**Tags:** `BFS` `Matrix`
**Difficulty:** Medium

---

## Key Idea
> Multi-source BFS from all initially rotten oranges at once. Each minute, rot spreads to adjacent fresh oranges. Count minutes until no fresh oranges remain.

## Notes
- Start BFS from **all** rotten oranges simultaneously (multi-source)
- Track `fresh` count — decrement as oranges rot
- Answer = number of BFS levels (minutes), only if `fresh == 0` at the end

## Common Mistakes
- Starting BFS from only **one** rotten orange → wrong spread order
- Returning `time` even when some fresh oranges are unreachable → should return `-1`
- Incrementing `time` per cell instead of per BFS level → overcounts minutes

## Solution
```python
from collections import deque

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        fresh = 0
        queue = deque()

        # 初始化：找到所有烂橘子和新鲜橘子
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    fresh += 1
                elif grid[r][c] == 2:
                    queue.append((r, c))  # 多源 BFS 起点

        time = 0
        directions = [(1,0), (-1,0), (0,1), (0,-1)]

        while queue and fresh > 0:
            for _ in range(len(queue)):   # 一层 = 一分钟
                r, c = queue.popleft()
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                        grid[nr][nc] = 2  # 变烂
                        fresh -= 1
                        queue.append((nr, nc))
            time += 1

        return time if fresh == 0 else -1  # 还有新鲜橘子说明有孤岛
```

## Complexity
- Time: `O(m × n)`
- Space: `O(m × n)`
