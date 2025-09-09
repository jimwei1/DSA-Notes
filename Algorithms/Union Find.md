# Union Find / Disjoint Set Union (DSU):

## Notes:

- Keep track of parent and joining of disjoint subsets / graphs
- Rank represents the upper bound of the height of the tree representing the set
  - Basically, all the children are connected to the parent underneath.
  - Only increase rank when attaching two trees of the same height. If one is larger than the other, it won’t change the height of the resulting tree.
- Find function compresses the tree to minimum height (optimizes it). It doesn’t actually change the ultimate root.
- Overall Time Complexity: `O(m * α(n)) -> # of operations (m) * inverse ackermann function`
  - Find Time Complexity: `O(α(n)) -> Inverse Ackermann function, basically constant`
  - Union Time Complexity: `O(α(n)) -> Inverse Ackermann function, basically constant`
- Overall Space Complextiy: `O(size of arrays)`
- **To find a cycle, simply check, before adding v to u, if both have the same root**
- **For Union, make sure you modify the roots and not the initial nodes**

**Find:**

- When we call this the first time, we run path compression to flatten the structure of the union find tree, to make future lookups easier.
- Example:

  ```
  1 → 3 → 5 → 7 (root)

  Now, calling find(1) without path compression would do:

  find(1) → find(3)
  find(3) → find(5)
  find(5) → find(7), which is the root.

  After Path Compression:

  1 → 7
  3 → 7
  5 → 7
  7 (root)
  ```

**Union:**

- We merge the smaller tree to the larger tree to keep it as balanced as possible
  - We keep track of this using the rank array
  - When we merge a smaller tree to the larger tree, the size of the larger tree doesn't change.
- If both are the same size, append one to the other and `+= 1` to the rank, as the height has increased by 1.

```
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size

    def find(self, p):
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])  # Compress the tree to minimum height
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)

        if rootP != rootQ:
            # Union by rank
            if self.rank[rootP] > self.rank[rootQ]:
                self.parent[rootQ] = rootP
            elif self.rank[rootP] < self.rank[rootQ]:
                self.parent[rootP] = rootQ
            else:
                self.parent[rootQ] = rootP
                self.rank[rootP] += 1

    def connected(self, p, q):
        return self.find(p) == self.find(q)

```

## Problems:

### 1102. Path With Maximum Minimum Value (Matrix as Implicit Graph)

**Code:**

```
class Solution:
    def maximumMinimumPath(self, grid: List[List[int]]) -> int:
        '''
        We can also solve this with Union Find:
            - We basically just going through each cell from highest value downwards.
            - We keep a visited set and union each new cell with its adjacent cells in visited.

        For rank and root lists, we can flatten the 2D array into a 1D array.
        '''
        rows = len(grid)
        cols = len(grid[0])

        rank = [1] * (rows * cols)

        root = []
        for num in range(rows * cols):
            root.append(num)

        visited = set()

        def find(x: int) -> int:
            if x != root[x]:
                root[x] = find(root[x])
            return root[x]

        def union(x:int, y:int) -> None:
            root_x = find(x)
            root_y = find(y)

            # Attach smaller tree to larger tree
            if root_x != root_y:
                if rank[root_x] > rank[root_y]:
                    root[root_y] = root_x
                elif rank[root_x] < rank[root_y]:
                    root[root_x] = root_y
                else:
                    root[root_y] = root_x
                    rank[root_x] += 1

        directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]

        values = [(row, col) for row in range(rows) for col in range(cols)]

        values.sort(key=lambda x: grid[x[0]][x[1]], reverse = True)

        for curr_row, curr_col in values:
            curr_pos = curr_row * cols + curr_col

            visited.add(curr_pos)

            for direction in directions:
                new_row = curr_row + direction[0]
                new_col = curr_col + direction[1]

                new_pos = new_row * cols + new_col

                if 0 <= new_row < rows and 0 <= new_col < cols and new_pos in visited:
                    union(curr_pos, new_pos)

            if find(0) == find((rows * cols) - 1):
                return grid[curr_row][curr_col]
```

### 305. Number of Islands II

**Code:**

```
class Solution:
    def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
        '''
        We could check for # of islands every time in positions -> O(m * n * p)

        Instead of checking each time, we could keep track of # of islands...
            - Union find each time we add land, append it to the larger island (in all 4 directions)
                - Keep track of # of islands
                    - A new island is when we can't union
                    - If we can't union on all 4, islands += 1
                    - BUT, if we can union MULTIPLE directions, islands -= 1 each time!
                    - When we're at a cell, islands += 1, and -= 1 per union!
                - O(p)
            - For each island, track its position with row * m + col -> Flatten into 1D

        - Keep a visited set for current land

        Edge cases:
        - What if two directions are part of the same island?
            - Just have union return a bool for whether it unioned
        - What if a position appears twice in positions?
            - Don't increment islands

        Time Complexity: O(m * n) for creating union find, O(k) for actual algo -> O(m*n+k)
        Space Complexity: O(m * n) for union find arrays

        '''

        parent = list(range(m * n)) # Top of tree
        rank = [1] * (m * n) # Height of tree
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        visited = set()
        islands = 0
        ans = []


        def find(x: int) -> int: # O(1)
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x: int, y:int) -> bool: # O(1)
            parent_x = find(x)
            parent_y = find(y)

            if parent_x != parent_y:
                if rank[parent_y] > rank[parent_x]:
                    parent[parent_x] = parent_y
                elif rank[parent_y] < rank[parent_x]:
                    parent[parent_y] = parent_x
                else:
                    parent[parent_x] = parent_y
                    rank[parent_y] += 1
                return True
            return False

        for position in positions: # O(k * 4 O (1))
            row, col = position[0], position[1]
            position_value = row * n + col
            if position_value not in visited:
                islands += 1
            visited.add(position_value)

            for direction in directions:
                new_row, new_col = row + direction[0], col + direction[1]
                new_position_value = new_row * n + new_col

                if 0 <= new_row < m and 0 <= new_col < n and new_position_value in visited:
                    if union(position_value, new_position_value):
                        islands -= 1

            ans.append(islands)

        return ans
```

### 261. Graph Valid Tree

**Code:**

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

### 684. Redundant Connection

**Code:**

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

### Power Grid Maintenance

```
class Solution:
    def processQueries(self, c: int, connections: List[List[int]], queries: List[List[int]]) -> List[int]:
        '''
        inputs:
            c -> int, # of power stations
                - id is 1 to c (1-based)
            n -> # of bidirectional cabbles, represented by connections
            connections -> array of [u_i, v_i], bidirectional
            queries ->
                [1, x] -> maintenance check
                    - if online, resolved
                    - if offline, check is resolved by the operational station with the smallest id in the same power grid. if no operational station exists, return -1
                [2, x] -> station x goes offline

        return:
            - array of integers representing the results of each query of type [1, x] in the order they appear
        _____________________________________________________________
        - Keep track of all power grids (union find)
        - Use min heap to keep track of the power grid of the union
            - Do this by making c heaps. Push each station to its parent's heap.
        - Create online array. While the bottom of a heap station is offline, pop.
        '''
        parent = list(range(c)) # 0-index the id
        rank = [1] * c
        
        def find(p: int) -> int:
            if parent[p] != p:
                parent[p] = find(parent[p])
            return parent[p]

        def union(p: int, q: int) -> None:
            root_p = find(p)
            root_q = find(q)

            if root_p != root_q: #union
                if rank[p] > rank[q]:
                    parent[root_q] = root_p

                elif rank[p] < rank[q]:
                    parent[root_p] = root_q

                else:
                    parent[root_q] = root_p
                    rank[root_p] += 1

        for u, v in connections:
            u -= 1
            v -= 1
            union(u, v)
        
        heap = [[] for _ in range(c)]

        for i in range(c):
            heapq.heappush(heap[find(i)], i)

        online = [True] * c
        ans = []

        for type, station in queries:
            x = station - 1
            if type == 2:
                online[x] = False
            elif type == 1:
                if online[x]:
                    ans.append(x + 1)
                else:
                    while len(heap[find(x)]) > 0 and online[heap[find(x)][0]] == False:
                        heapq.heappop(heap[find(x)])
                    if len(heap[find(x)]) == 0:
                        ans.append(-1)
                    else:
                        ans.append(heap[find(x)][0] + 1)

        return ans
```

### Minimum Time for K Connected Components

```
class Solution:
    def minTime(self, n: int, edges: List[List[int]], k: int) -> int:
        '''
        n -> int
        edges -> undirected graph
            edges[i] = [u_i, v_i, time_i]
        k -> int

        return:
            t -> min time s.t. after removing all edges with time <= t, the graph contains at least k connected components
        _______________________________________________________________

        DSU to find # of connected components. Can't reverse a union, so go by reverse time. Add components until we have < k components
        '''
        parent = list(range(n))
        rank = [1] * n

        def find(p: int) -> int:
            if parent[p] != p:
                parent[p] = find(parent[p])
            return parent[p]

        def union(p: int, q: int) -> bool:
            rootP = find(p)
            rootQ = find(q)

            if rootP != rootQ:
                if rank[rootP] > rank[rootQ]:
                    parent[rootQ] = rootP
                elif rank[rootQ] > rank[rootP]:
                    parent[rootP] = rootQ
                else:
                    parent[rootQ] = rootP
                    rank[rootP] += 1

                return True

            return False

        components = n
        
        edges.sort(key = lambda x: x[2], reverse = True)

        for u, v, t in edges:
            if union(u, v):
                components -= 1

                if components < k:
                    return t

        return 0 # If we never drop below k, then the original graph already had >= k components.
```
