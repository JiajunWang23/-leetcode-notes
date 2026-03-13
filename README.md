# 📒 LeetCode Mistake Notebook

> Tracking mistakes, patterns, and solution templates.

---

## Table of Contents

| # | Problem | Category | Difficulty | Status |
|---|---------|----------|------------|--------|
| 1 | [Two Sum](#1-two-sum) | HashMap | Easy | ✅ |
| 3 | [Longest Substring Without Repeating Characters](#3-longest-substring-without-repeating-characters) | Sliding Window | Medium | ✅ |
| 12 | [Integer to Roman](#12-integer-to-roman) | Math / Greedy | Medium | ✅ |
| 875 | [Koko Eating Bananas](#875-koko-eating-bananas) | Binary Search | Medium | ✅ |
| 994 | [Rotting Oranges](#994-rotting-oranges) | BFS | Medium | ✅ |
| 1482 | [Minimum Number of Days to Make m Bouquets](#1482-minimum-number-of-days-to-make-m-bouquets) | Binary Search | Medium | ✅ |

---

## Category Index

- [HashMap](#-hashmap)
- [Sliding Window](#-sliding-window)
- [Math / Greedy](#-math--greedy)
- [Binary Search](#-binary-search)
- [BFS](#-bfs)

---

## 🔑 HashMap

### 1. Two Sum

**Link:** https://leetcode.com/problems/two-sum/

**Key Idea:**
> Walk through the array once. At each step, ask: "Have I already seen the number I need?"

**Notes:**
- Use HashMap to store `value -> index`
- `enumerate` is required because the answer is **indices**, not values
- Compute `diff = target - n`, check if `diff` is already in the map

**Common Mistakes:**
- Forgetting `enumerate` → can't return indices
- Putting `prevMap[n] = i` **before** the `if` check → might pair element with itself

**Solution:**
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

**Line-by-line Breakdown:**

| Line | Purpose |
|------|---------|
| `prevMap = {}` | Empty HashMap: `{value: index}` |
| `for i, n in enumerate(nums)` | `i` = index, `n` = current value |
| `diff = target - n` | The number we need to complete the pair |
| `if diff in prevMap` | Have we seen this number before? |
| `return [prevMap[diff], i]` | Found a pair — return both indices |
| `prevMap[n] = i` | Not found yet — store for future lookups |

**Complexity:** Time `O(n)` · Space `O(n)`

---

## 🪟 Sliding Window

### 3. Longest Substring Without Repeating Characters

**Link:** https://leetcode.com/problems/lengthofLongestSubstring/

**Key Idea:**
> Use a sliding window with a set. Expand right, shrink left whenever a duplicate appears.

**Notes:**
- `left` pointer shrinks the window when a duplicate is found
- `set` tracks characters in the current window
- Update `res = max(res, right - left + 1)` at every step

**Common Mistakes:**
- Forgetting to remove `s[left]` from the set before moving `left` forward
- Off-by-one: window size is `right - left + 1`, not `right - left`

**Solution:**
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

**Complexity:** Time `O(n)` · Space `O(min(n, m))` where m = charset size

---

## 🔢 Math / Greedy

### 12. Integer to Roman

**Link:** https://leetcode.com/problems/integer-to-roman/

**Key Idea:**
> Greedily subtract the largest possible Roman numeral value at each step.

**Notes:**
- Store all value-symbol pairs in descending order
- Each iteration: while `num >= val`, append `sym` and subtract `val`
- Works because Roman numerals follow a greedy structure

**Common Mistakes:**
- Forgetting subtractive forms like `IV (4)`, `IX (9)`, `XL (40)`, etc.
- Not listing pairs in **descending** order

**Solution:**
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

**Complexity:** Time `O(1)` (bounded by max value 3999) · Space `O(1)`

---

## 🔍 Binary Search

### 875. Koko Eating Bananas

**Link:** https://leetcode.com/problems/koko-eating-bananas/

**Key Idea:**
> Binary search over eating speed. For speed `mid`, check if Koko can finish within `h` hours. Find the **minimum** valid speed.

**Notes:**
- Speed range: `[1, max(piles)]`
- For each pile, hours needed = `ceil(pile / speed)`
- Use `right = mid` (not `mid - 1`) — `mid` itself may be the answer

**Common Mistakes:**
- Using `right = mid - 1` → might skip the correct answer
- Forgetting to use ceiling division (integer divide rounds down)

**Solution:**
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

**Complexity:** Time `O(n log m)` where m = `max(piles)` · Space `O(1)`

---

### 1482. Minimum Number of Days to Make m Bouquets

**Link:** https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/

**Key Idea:**
> Binary search over the number of days. For a given day `mid`, count how many bouquets can be made using consecutive bloomed flowers. Find the **minimum** day that gives at least `m` bouquets.

**Notes:**
- A flower blooms on day `bloomDay[i]`; can only pick if `bloomDay[i] <= mid`
- Need `k` **consecutive** bloomed flowers per bouquet
- Count bouquets by scanning for consecutive bloomed flowers

**Common Mistakes:**
- Resetting `consecutive` to `0` when a flower hasn't bloomed yet (this is correct — don't forget it)
- Not checking `if m * k > len(bloomDay)` early (impossible case)

**Solution:**
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

**Verify `mid` logic (the "dumbest" check):**
> For day `mid`, scan left to right. Count consecutive bloomed flowers. Every time you hit `k` in a row, that's one bouquet. If total bouquets `>= m`, day `mid` works.

**Comparison with Koko:**

| | Koko (875) | Bouquets (1482) |
|---|---|---|
| Binary search on | eating speed | days |
| Condition check | `sum(ceil(p/mid)) <= h` | count consecutive blooms `>= m` |
| Answer | minimum speed | minimum days |

**Complexity:** Time `O(n log d)` where d = `max(bloomDay)` · Space `O(1)`

---

## 🌊 BFS

### 994. Rotting Oranges

**Link:** https://leetcode.com/problems/rotting-oranges/

**Key Idea:**
> Multi-source BFS from all initially rotten oranges simultaneously. Each minute, rot spreads to adjacent fresh oranges. Count minutes until no fresh oranges remain.

**Notes:**
- Start BFS from **all** rotten oranges at once (multi-source)
- Track count of fresh oranges; decrement as they rot
- Answer is the number of BFS levels (minutes), only if `fresh == 0` at the end

**Common Mistakes:**
- Starting BFS from only one rotten orange instead of all at once
- Returning `time` even when some fresh oranges are unreachable → should return `-1`
- Off-by-one on `time`: increment after each full BFS level, not each cell

**Solution:**
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

**Complexity:** Time `O(m × n)` · Space `O(m × n)`

---

## 📌 Template Cheatsheet

### HashMap
```python
# 频率统计 — frequency count
from collections import Counter
freq = Counter(arr)

# 分组 — group by key
from collections import defaultdict
groups = defaultdict(list)
for item in items:
    groups[item[0]].append(item)
```

### Sliding Window
```python
# 固定/可变窗口通用框架
left = 0
for right in range(len(s)):
    # 扩展窗口，加入 s[right]
    while <window invalid>:
        # 收缩左边
        left += 1
    # 更新答案
```

### Binary Search — Find Minimum Valid Value
```python
left, right = lo, hi

while left < right:
    mid = (left + right) // 2
    if condition(mid):
        right = mid       # mid 可行，继续往左缩
    else:
        left = mid + 1    # mid 不行，往右走

return left
```

### BFS
```python
from collections import deque

queue = deque([start])
visited = {start}

while queue:
    node = queue.popleft()
    for neighbor in get_neighbors(node):
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)
```

---

## 💡 Key Reminders

> **O(1) lookup / counting → HashMap**
> **Contiguous subarray/substring → Sliding Window**
> **Answer in a range + monotonic condition → Binary Search**
> **Shortest path / level-by-level spread → BFS**
> **Need the index → `enumerate`; only need the value → `for n in nums`**
