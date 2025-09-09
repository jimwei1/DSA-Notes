# Heap / Priority Queue

## Notes

**MIN HEAP HAS MIN AT THE TOP, MAX HEAP HAS MAX AT TOP**

### Properties:

- Ordered binary tree
- Implemented as array:
  - Left child is position 2\*i
  - Right is position 2\*i + 1
  - Parent node is position i//2
- Height: O(logn)
- Only guarantees the root is the smallest/largest. SortedContainer allows for whole array/

### Uses:

- Heap Sort
- Priority Queues

### Types:

- **Max-Heap**
  - Binary tree, value of i ≤ value of parent
  - Max at top
  - Heap Sort
- **Min-Heap**
- Binary tree, value of i ≥ value of parent - Min at top - Priority queues
  dictionary to be sorted.
- **Min/Max heap uses the next value of tuples for tiebreakers!**

#### Max Heap:

- Python doesn’t have Max heap, need to make a min heap and use negative values to get the lowest (highest), then abs() it
- To get a max heap, multiply all numbers by -1 when going into heap
- Take abs() when popping heap

#### Heap Queue / Priority Queue Algorithm (Min Heap)

```
stones = [0, 1, 2]
heapq.heapify(stones)
first = heapq.heappop(stones)

heapq.heapify(iterable) --> transforms iterable into heap, O(n) time (linear)
heapq.heappop(heap)  --> pops smallest element from the heap, O(logn) time
heapq.heappush(heap, element) --> add element to heap, O(logn) time
```

- Creates min heap from list (linear time)
- Heapify uses the first element, so you can heapfiy array of arrays and it will sort by first element

- Can also create a heap like:

  ```
  heap = []
  heappush(heap, value)
  val = heappop(heap)
  ```

- To check the top of a heap, simply `heap[0]`

#### Heap Sort

- Using heap to sort something
- Calling `heappop()` repeatedly is basically heap sort

#### Priority Queue

- Uses Min Heap
- First in, First out, but Serves elements with highest priorities first before elements with lower priorities

## Problems:

### 1102. Path with Maximum Minimum Value (Graph BFS + Priority Queue)

**Code:**

```
class Solution:
    def maximumMinimumPath(self, grid: List[List[int]]) -> int:
        '''
        Brute force is to DFS and check every single path. Time Complexity: very exponential

        An optimization would be to BFS and only pick the highest candidates.
        In case of a tie, we just store them both in the queue.
        This way, we can skip obviously bad paths that will hit a minimum and instantly remove the path from consideration.

        If we run into a duplicate, it's possible that we'll go down the wrong path, but that doesn't matter as it won't affect the minimum of the path (since the other path will be in the priority queue still, so once the dead end path is traversed, it will go down the other path.)

        + Keep a visited set to avoid repeating

        Time Complexity: Worst case scenario, we have to traverse the whole matrix, so m * n. We also have to push into the heap, which is log(m*n) -> O(m * n * log(m * n))
        '''

        ans = grid[0][0]
        rows = len(grid)
        cols = len(grid[0])

        q = [(-grid[0][0], 0, 0)]
        heapq.heapify(q)
        visited = set()

        row, col = 0, 0
        directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]

        while not (row == (rows - 1) and col == (cols - 1)):
            val, row, col = heapq.heappop(q)
            visited.add((row, col))
            ans = min(ans, -val)
            for direction in directions:
                new_row = row + direction[0]
                new_col = col + direction[1]

                if 0 <= new_row < rows and 0 <= new_col < cols and (new_row, new_col) not in visited:
                    tup = (-grid[new_row][new_col], new_row, new_col)
                    heapq.heappush(q, tup)

        return ans
```

### 295. Find Median From Data Stream (2 Heaps)

**Code:**

```
class MedianFinder:
    '''
    Brute force: We can sort and return the median, which is O(n logn)

    We can go faster with two heaps - max heap for the lower half and min heap for the upper half.
    - These heaps must be balanced, they cannot have a length difference > 1.
        - If the len diff goes over 1, we must move one to the other
    - To add, always add to the left heap (max-heap)
        - After adding to left heap, if max(left heap) > min(right heap), remove max(left) and add to right.

    - To find median, if one heap has a greater length than the other, we return the top of that heap
    - Otherwise, find the top of both heaps and return the middle of that.

    Time complexity:

    Each Add: O(log(n))
    Each Find: O(1)
    '''

    def __init__(self):

        self.left = []
        self.right = []

    def addNum(self, num: int) -> None:

        heapq.heappush(self.left, -num)

        # Ensure we're pushing to the correct heap
        if self.right and -self.left[0] > self.right[0]:
            val = -heapq.heappop(self.left)
            heapq.heappush(self.right, val)

        # Right is too big
        if (len(self.right) - len(self.left)) > 1:
            val = heapq.heappop(self.right)
            heapq.heappush(self.left, -val)

        # Left is too big
        elif (len(self.left) - len(self.right)) > 1:
            val = -heapq.heappop(self.left)
            heapq.heappush(self.right, val)


    def findMedian(self) -> float:
        if len(self.left) > len(self.right):
            return -self.left[0]

        elif len(self.right) > len(self.left):
            return self.right[0]
        else:
            return (-self.left[0] + self.right[0]) / 2
```

### 3170. Lexographically Minimum String After Removing Stars

```
class Solution:
    def clearStars(self, s: str) -> str:
        '''
        heap (letter, -index) -> max heap for the index as we want to pop rightmost
        '''

        heap = []
        n = len(s)
        remove = set()

        for index, letter in enumerate(s):
            if letter != '*':
                heapq.heappush(heap, (letter, -index))
            else:
                remove.add(-heapq.heappop(heap)[1])

        ans = []
        for i in range(n):
            if s[i] == '*' or i in remove:
                continue

            ans.append(s[i])

        return ''.join(ans)
```
