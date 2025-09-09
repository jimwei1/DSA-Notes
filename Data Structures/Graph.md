# Graph

- Adjacency List / Matrix:
  - Basically just ways to keep track of all edges for each node

## Notes:

### Directional Graphs:

- Strongly connected Components:
  - Each node in this group can travel to any other node in this list
  - By definition, a node cannot be a part of 2 strongly connected components.
  - Use a DSU for this

### Implicit Graphs:

- Not explicitly stored as a data structure.
- Nodes and edges aren't stored explicitly, but can be derived from a structure
- e.g. Using a 2D matrix as a graph.
- **When doing this, you can flatten the 2D list into a long 1D list, as such:**
  ```
  cols = len(grid[0])
  (row, col) -> index = row * cols + col
  ```

### Sorting (row, col) based on its `grid[row][col]` value

```
vals = [(row, col) for row in range(rows) for col in range(cols)]
vals.sort(key= lambda x: grid[x[0]][x[1]], reverse=True)
```

### Converting Tree to Graph (Adjacency List):

```
def convertToGraph(current, parent, graph):
  if current is None:
      return
  if parent:
      graph[current.val].append(parent)
  if current.left:
      graph[current.val].append(current.left.val)
      convertToGraph(current.left, current.val, graph)
  if current.right:
      graph[current.val].append(current.right.val)
      convertToGraph(current.right, current.val, graph)
```

## Algorithms:

### Djikstra's

- Find the shortest path from a source node to all other nodes in a weighted graph with non-negative edge weights.
- **Note:** Djikstra's algorithm guarantees that the first time we pop a node from the min heap, it is the shortest distance from the start. However, there can still be multiple paths that all have that same shortest distance to the node.

#### Notes:

- Use a min heap (priority queue) to always explore the node with the smallest known distance so far, with `(curr_distance, curr_node)`
	- **Make sure to put distance at the start so it's sorted by this!**
- Maintain a shortest_paths dictionary / distance array to store the earliest time we reach each node.
- Initialize with (0, source) in the heap.

#### Problems:

##### 743. Shortest Network time

**Code:**

```
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        '''
        Djikstra's
            - Keep a min heap, dictionary of times
            - Start at source, append (path, node) to min heap and always move to the next step that gives us the smallest path
            - Once we visit a node, set the dictionary time
                - Since we will always visit nodes on the least path value, don't need to update it after we visit it.
        '''
        visited = set()
        ans = 0

        # (curr_time, curr_node)
        priority_q = [(0, k)]
        heapq.heapify(priority_q)

        adj_list = defaultdict(list)

        for time in times:
            u, v, w = time[0], time[1], time[2]
            adj_list[u].append((v, w))

        while priority_q and len(visited) != n:
            curr_time, curr_node = heapq.heappop(priority_q)

            if curr_node in visited:
                continue

            ans = max(ans, curr_time)
            visited.add(curr_node)

            neighbors = adj_list[curr_node]

            for neighbor in neighbors:
                v, w = neighbor[0], neighbor[1]
                if v not in visited:
                    heapq.heappush(priority_q, (curr_time + w, v))

        if len(visited) != n:
            return -1

        return ans
```

##### 1976. Number of Ways to Arrive at Destination

**Code:**

```
class Solution:

    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        '''
        djikstras, count # of times we reach intersection n-1 in shortest time

        Keep track of these:
            - min time to get to each node
            - # of times for each of those

        For each new node we visit, add the # of previous paths to the new node's path.
        Then, each time we reach the new node with the same curr_distance, we will add the previous node's count, as they will each be a new way of reaching the new node.
        '''

        MOD = 10**9 + 7

        min_distance = [float('inf')] * n
        counts = [0] * n
        counts[0] = 1

        # curr_distance, curr_node
        min_heap = [(0,0)]

        heapq.heapify(min_heap)

        adj_list = defaultdict(list)

        for road in roads:
            u, v, t = road[0], road[1], road[2]

            adj_list[u].append((v, t))
            adj_list[v].append((u, t))

        while min_heap:
            curr_distance, curr_node = heapq.heappop(min_heap)

            if curr_distance > min_distance[curr_node]:
                continue

            neighbors = adj_list[curr_node]

            for neighbor in neighbors:
                new_node, add_distance = neighbor[0], neighbor[1]

                new_distance = curr_distance + add_distance

                if new_distance < min_distance[new_node]:
                    min_distance[new_node] = new_distance
                    counts[new_node] = counts[curr_node]
                    heapq.heappush(min_heap, (new_distance, new_node))
                elif new_distance == min_distance[new_node]:
                    # Increase count of new node as there's a new way of reaching it, but no need to push it back into the min_heap as it would be counting the same (distance, node) pair.
                    counts[new_node] += counts[curr_node]

        return counts[n-1] % MOD

```

### A\*

### Topological Sort

-



## Problems:
