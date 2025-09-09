# Tree

## Notes:

- A valid tree is:
  - Connected graph
  - No cycles
  - Exactly `n` nodes `n-1` edges

### Problems:

#### 261. Graph Valid Tree

**Code: (Iterative DFS)**

```
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        '''
        Valid Tree:
            - Connected graph
            - No cycles
            - Exactly n-1 edges

        DFS and keep a visited array to ensure no cycles
        '''
        if len(edges) != n - 1:
            return False

        adj_list = defaultdict(list)

        for edge in edges:
            u, v = edge[0], edge[1]

            adj_list[u].append(v)
            adj_list[v].append(u)


        visited = set()

        def dfs(i: int, parent: int) -> bool:
            if i in visited:
                return False

            visited.add(i)

            for neighbor in adj_list[i]:
                if neighbor != parent:
                    if dfs(neighbor, i) == False:
                        return False

            return True


        return dfs(0, -1) and len(visited) == n
```

**Code: (Union Find):**

```
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        '''
        Valid Tree:
            - Connected graph
            - No cycles
            - Exactly n-1 edges

        Union find for cycles
            - For every edge, union u and v. If they have the same parent, return False
        '''
        if len(edges) != n - 1:
            return False

        parent = [i for i in range(n)]
        rank = [1] * n

        def find(x: int) -> int:
            if x != parent[x]:
                parent[x] = find(parent[x])

            return parent[x]

        def union(x: int, y: int) -> bool:
            root_x = find(x)
            root_y = find(y)

            if root_x != root_y:
                if rank[root_x] > rank[root_y]:
                    parent[root_y] = root_x

                elif rank[root_x] < rank[root_y]:
                    parent[root_x] = root_y

                else:
                    parent[root_y] = root_x
                    rank[root_x] += 1

                return True

            else:
                return False

        for edge in edges:
            u, v = edge[0], edge[1]

            if union(u, v) == False:
                return False

        return True
```

#### 684. Redundant Connection

**Code: (Union Find)**

```
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        '''
        Union find -> merge u, v for each edge. If both nodes have the same parent, then there's a cycle and we should delete.

        Time Complexity: O(a(V)) + O(a(V)*e) = O(a(V)*E)
        Space Complexity: O(2*v) = O(v)
        '''
        n = len(edges)

        parent = [i for i in range(n + 1)]
        rank = [1] * (n + 1)

        ans = None

        def find(x: int) -> int: # O(a(V))
            if x != parent[x]:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x: int, y: int) -> bool: # O(a(V) * e)
            root_x = find(x)
            root_y = find(y)

            if root_x == root_y:
                return False

            else:
                if rank[root_x] > rank[root_y]:
                    parent[root_y] = root_x

                elif rank[root_x] < rank[root_y]:
                    parent[root_x] = root_y

                else:
                    parent[root_y] = root_x
                    rank[root_x] += 1

                return True

        for edge in edges:
            u, v = edge[0], edge[1]

            if union(u, v) == False:
                ans = edge

        return ans
```

# Binary Tree

## Notes:

### LCA of Binary Tree

- 2 Cases:
  1. LCA is not p or q
  2. LCA is p or q.
- Recursive DFS down. If we find p or q, return it. Otherwise, keep going
- If left and right both return a node, then return the current node, as it's option 1.
- If only one side returns a node, keep returning it, as it's option 2.

**Code:**

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 2 Possibilities:
        # 1. LCA is not p or q
        # 2. LCA is p or q.
        # Recursive DFS down. If we find p or q, return it. Otherwise, keep going
        # If left and right both return a node, then return the current node, as it's option 1.
        # If only one side returns a node, keep returning it, as it's option 2.

        def dfs(node: 'TreeNode') -> 'TreeNode':
            if not node:
                return None

            if node == p or node == q:
                return node

            left = dfs(node.left)
            right = dfs(node.right)

            # Option 1
            if left and right:
                return node

            if left and not right:
                return left

            if right and not left:
                return right

            return None

        return dfs(root)
```

# Binary Search Tree

## Notes:

- **No duplicates**
- Height of a BST tends to be log(n)
- **Everything** right has to be greater than parent
- **Everything** left has to be less than parent
- **use in-order-traversal (left, curr, right) for sorted**
- Validating BST:
  - lower and upper boundary as parameters
  ```
  #Keep track of:
  #root, min, max
  #min --> updated if we go right
  #max --> updated if we go left
  ```

# Minimum Spanning Tree

## Notes:

- A minimum spanning tree connects all vertices in a weighted, undirected graph with the minimum possible total edge weight.
- Edges must equal `V-1`, and must not contain cycles.

### Kruskal's Algorithm (Greedy)

1. Sort all possible edges by weight
2. Union find to add the lowest

# AVL Tree
