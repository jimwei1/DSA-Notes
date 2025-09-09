# BFS

## Notes:

- BFS is a Queue. Think of it as BBQ -> BQ :)
- To optimize space, you can just modify the graph in place instead of creating a visited set.
- BFS is usually better for "shortest path" than DFS

### Graph BFS:

- This is the requirement:

  - `if 0 <= new_row < rows and 0 <= new_col < cols and grid[new_row][new_col] == 1 and (new_row, new_col) not in visited:`

- BFS is queue

  - use q = deque()
  - q.popleft()
  - BFS in tree:

    - To keep track of level, pass in param as (root, level) tuple then level + 1 each iteration
    - To get right-most of each level, simply keep hashmap and then update with each new value

    ```
    def bfs(root):
        if not root:
            return

        queue = deque([root])

        while queue:
            node = queue.popleft()
            print(node.val)  # Process node

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    ```

  - BFS in tree, tracking depth:

  ```
    def BFS(node):
        if not node:
            return None

        q = deque([node])
        d = 1

        while q:
            if d == depth - 1:
                for i in range(len(q)):
                    curr = q.popleft()
                    if curr.left:
                        curr.left = TreeNode(val, left=curr.left)
                    if curr.right:
                        curr.right = TreeNode(val, right=curr.right)
                break

            else:
                for i in range(len(q)):
                    curr = q.popleft()
                    if curr.left:
                        q.append(curr.left)
                    if curr.right:
                        q.append(curr.right)

                d += 1
  ```

  - BFS in Adjacency List Graph

    ```
    time = 0
    	  queue = deque([start])
    	  visited = set([start])

    	  while queue:
    	      level_size = len(queue)
    	      while level_size > 0: #need to keep track of how many are left
    															#in each level to properly increment time
    	          curr = queue.popleft()
    	          for neighbor in newGraph[curr]:
    	              if neighbor not in visited:
    	                  queue.append(neighbor)
    	                  visited.add(neighbor)
    	          level_size -= 1
    	      time += 1

    	  return time - 1 #**cuz BFS runs 1 more time at the end**
    ```

  - BFS in 2D Graph:

    - append —> while q, pop —> for directions, etc.
    - 4 conditions:
      - In range (row, col)
      - value == 1
      - not in visited

    ```
    def bfs(row, col):
        visited.add((row, col))
        q.append((row, col))

        while q:
            currRow, currCol = q.popleft()
            directions = [[-1, 0], [1, 0], [0, 1], [0, -1]]

            for dr, dc in directions:
                newRow = currRow + dr
                newCol = currCol + dc

                if newRow in range(len(grid)) and newCol in range(len(grid[0])) and grid[newRow][newCol] == "1" and (newRow, newCol) not in visited:
                    visited.add((newRow, newCol))
                    q.append((newRow, newCol))
    ```

## Problems:

### Graph BFS:

#### 694. Number of Distinct Islands

```
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:
        # 1. Find the islands -> DFS or BFS will work. Let's do BFS. We'll be using a queue to expand the islands one layer at a time.
        # 2. Store the shape of each island we find
        #       -> We want the shape to be stored in a hashable way
        #       -> We can store the coordinates of each island, then normalize them by subtracting the x and y of each!
        #       -> Then, we sort the coordinates and store in a hashset as a tuple.
        # 3. Return unique shapes (length of hashset)

        # Initialize Variables:
        rows = len(grid)
        cols = len(grid[0])

        # Store our next levels to search for
        q = deque()
        visited = set()
        unique_islands = set()

        directions = [-1, 0], [1, 0], [0, -1], [0, 1]


        # BFS Helper Function. We call this when we see a 1, so guaranteed to be an island of some sort.
        # Everything in the queue is already guaranteed to be 1.
        def BFS(row: int, col: int):
            curr_island = []
            min_row = float('inf')
            min_col = float('inf')

            visited.add((row, col))
            q.append((row, col))
            curr_island.append((row, col))
            min_row = min(min_row, row)
            min_col = min(min_col, col)

            while q:
                curr_row, curr_col = q.popleft()

                for direction in directions:
                    new_row = curr_row + direction[0]
                    new_col = curr_col + direction[1]

                    if 0 <= new_row < rows and 0 <= new_col < cols and grid[new_row][new_col] == 1 and (new_row, new_col) not in visited:
                        visited.add((new_row, new_col))
                        q.append((new_row, new_col))
                        curr_island.append((new_row, new_col))
                        min_row = min(min_row, new_row)
                        min_col = min(min_col, new_col)

            # Normalize the island
            normalized_island = []
            for coordinate in curr_island:
                new_row = coordinate[0] - min_row
                new_col = coordinate[1] - min_col
                new_coordinate = (new_row, new_col)
                normalized_island.append(new_coordinate)

            normalized_island.sort()

            unique_islands.add(tuple(normalized_island))


        # Call BFS on each coordinate

        for row in range(rows):
            for col in range(cols):
                if (row, col) not in visited and grid[row][col] == 1:
                    BFS(row, col)

        return len(unique_islands)
```

#### 778. Swim in Rising Water

**Code:**

```
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        '''
        Can move if: elevation <= t. Basically, cannot move to a cell until t == elevation

        An easier question to answer: Can we reach the end at "t" time? -> Binary search for t

        Range: (0, max(grid))

        BFS to check whether we can reach the end

        Interview: Walk through "can_reach" a single time
        '''
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        rows = len(grid)
        cols = len(grid[0])
        max_value = 0

        for row in range(rows):
            for col in range(cols):
                max_value = max(max_value, grid[row][col])

        def can_reach(t: int) -> bool: # O(m * n), O(m * n)
            if grid[0][0] > t:
                return False

            q = deque([(0, 0)])
            visited = set()

            while q:
                row, col = q.popleft()

                if row == rows - 1 and col == cols - 1:
                    return True

                for direction in directions:
                    new_row, new_col = row + direction[0], col + direction[1]

                    if 0 <= new_row < rows and 0 <= new_col < cols and (new_row, new_col) not in visited and grid[new_row][new_col] <= t :
                        q.append((new_row, new_col))
                        visited.add((new_row, new_col)) # Do this immediately, or else lots of repeated work as the same row, col will get added multiple times as two different nodes may add the same neighbor to the q before that neighbor is processed.

            return False

        left, right = 0, max_value # we can't swim if t < grid[0][0]
        while left <= right: # O(log(max_value - grid[0][0]) * O(m*n)), O(m*n)
            mid = (left + right) // 2
            if can_reach(mid):
                right = mid - 1

            else:
                left = mid + 1

        # We will reach our answer, then right will move again. So we return left
        return left
```
