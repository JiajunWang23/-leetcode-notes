# Clone Graph

**LeetCode 133 · Clone Graph**  
**难度：Medium | 方法：DFS + HashMap**

---

## 思路

给你一个图，复制一份一模一样的图（深拷贝）。

**难点：** 图里有环，如果不记录已经克隆过的节点，DFS 会无限循环。

**解法：** 用一个 hashmap `oldToNew` 记录**原节点 → 克隆节点**的对应关系，遇到已经克隆过的节点直接返回，不重复克隆。

---

## 代码

```python
from typing import Optional

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        # 1. 创建空 hashmap，记录原节点和克隆节点的对应关系
        oldToNew = {}

        def dfs(node):
            # 3. 如果这个节点已经克隆过，直接返回，不重复克隆
            if node in oldToNew:
                return oldToNew[node]

            # 4. 创建新节点，值和原节点一样
            copy = Node(node.val)

            # 5. 把原节点和克隆节点的对应关系存进 hashmap
            oldToNew[node] = copy

            # 6-7. 遍历每个邻居，递归克隆，加进 copy 的邻居列表
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))

            # 8. 返回克隆好的节点
            return copy

        # 9. 节点不为空就开始 DFS，否则返回 None
        return dfs(node) if node else None
```

---

## 步骤

1. 创建空 hashmap `oldToNew`，记录原节点和克隆节点的对应关系
2. 定义 DFS 函数，传入一个原节点
3. 如果这个节点已经在 hashmap 里，直接返回克隆节点，不重复克隆
4. 创建新节点 `copy`，值和原节点一样
5. 把原节点和克隆节点的对应关系存进 hashmap
6. 遍历原节点的每一个邻居
7. 递归克隆每个邻居，把克隆出来的邻居加进 `copy` 的邻居列表
8. 返回克隆好的节点
9. 如果传进来的节点不是空的就开始 DFS，否则返回 `None`

---

## 例子演示

```
图：
1 --- 2
|     |
4 --- 3

节点1 的 neighbors = [2, 4]
节点2 的 neighbors = [1, 3]

dfs(1) → copy1 = Node(1)，oldToNew = {1: copy1}
  dfs(2) → copy2 = Node(2)，oldToNew = {1: copy1, 2: copy2}
    dfs(1) → 已在 hashmap 里 → 直接返回 copy1  ✅ 不会无限循环
  copy2.neighbors = [copy1]
copy1.neighbors = [copy2, copy4]
```

---

## 关键点

- `oldToNew` 的作用：防止图里有环导致无限循环
- 先存 hashmap 再递归邻居，顺序不能反，否则还是会无限循环
- `copy = Node(node.val)` 创建的是全新的节点，改克隆图不影响原图
- `return dfs(node) if node else None` 防止传进来的是空节点
