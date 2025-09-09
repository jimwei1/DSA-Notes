# Binary Search

## Notes:

**Problems that require maximizing the minimum or minimizing the maximum often suggest a binary search approach.**

### Left < Right

- If we're using `while left < right`, we can return either left or right as `left == right`
  - Usually will need to perform a final check in this case

### Left <= Right (Left / Right Bias)

- However, if we're using `while left <= right`

  - If we're adjusting `right` when we find a match, it will be left biased and we return `left`
    - This is usually when we're looking for a first (minimal) value, where everything below it fails.
    - When you adjust the right pointer on a match (or when the condition is satisfied), you narrow the search toward the smallest value that works. At the end, left will be at the minimal valid value.
  - If we're adjusting `left` when we find a match, it will be right biased and we return `right`
    - This is usually when we're looking for a last (maximal) value, where everything above it fails.
    - When you adjust the left pointer on a match, you narrow the search toward the largest value that works. At the end, right will be at the maximal valid value.
  - **Recommended to just do a manual run to make sure**

### Runtime

- Runtime is O(log n) because while loop will run as many times as you can divide n by 2, which is log2(n)
- l, r pointers, // by 2 to get m
- if target > m, l = m + 1
- if target < m, r = m - 1
- repeat until found
- while l <= r, as we still need to check if there’s only 1 left (l = r)
- For 2d matrix:
  - The row index for the matrix is **`i // number_of_columns`**
  - The column index for the matrix is **`i % number_of_columns`**
- divmod(m, n)
  - returns (quotient, remainder) as a tuple
- TO DO BINARY SEARCH WITHOUT IMPLEMENTING:

```
bisect.bisect(a, x)

a = sorted array
x = position of element to be found

#example:
intervals = sorted(zip(start, end, profit)
bisect.bisect(intervals, (intervals[i][2], -1, -1) )
#this will compare i's end to the others start
#Maximum Profit in Job Scheduling

```

## Problems:

### 2529. Maximum Count of Positive Integer and Negative Integer

```
class Solution:
    def maximumCount(self, nums: List[int]) -> int:
        '''
        Brute force: Time complexity is O(n)
        To optimize, we can use binary search to cut the time complexity to O(log(n))

        Binary Search:
            - Already sorted
            - Find the pivot index (when one side is positive and one side is negative)
            - If we find a 0, have to expand the search on one side...
            - Essentially we're just trying to find the boundaries between pos to 0/neg, and neg to 0/pos.
            - Once we find the first negative and the first positive, we can easily calculate pos/neg.

        Since we're trying to find two pivot indecies, we should probably just run two binary searches, one for neg and one for right

        Examples:
        [-2,-1,-1,1,2,3]
         l      m     r
                  l m r
                  l,r

        Pass 1:
        l       m     r
                  l m r
                 l,r
        '''

        left, right = 0, len(nums) - 1

        pos, neg = 0, 0

        # Find first negative number

        # If we find a non-negative number, we shift left.
        # If we find a negative number, we shift right
        # We will end with left = first non negative number. Since we're 0 indexed, neg = left.
        while left <= right:
            mid = (left + right) // 2

            # Shift left
            if nums[mid] >= 0:
                right = mid - 1

            # Shift right if we find a negative number
            else:
                left = mid + 1

        neg = left

        # Find first positive number

        # If we find a positive number, we shift left
        # If we find a non-positive number, we shift right
        # We will end with left = first non positive number. Since we're 0 indexed, pos = len(nums) - left


        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            # Shift left
            if nums[mid] > 0:
                right = mid - 1

            # Shift right
            else:
                left = mid + 1

        pos = len(nums) - left

        return max(pos, neg)
```

### 2226. Maximum Candies Allocated to K Children

```
class Solution:
    def maximumCandies(self, candies: List[int], k: int) -> int:
        '''
        1. Since we can't merge two piles together, the max would be the max value of (candies)
            a. It's the MAX, not min because k can be less than len(candies)
        2. Can we just start at the max value, then decrement by 1 each time, seeing if we can properly split?
        3. In these kinds of problems, we can often also binary search to optimize, so let's try that.

        The psuedocode for this would be like:

        helper function - can_split(n: int)
            1. if n > max_value, return false
            2. init piles
            3. for each value in candies > n, candies[value] // n -> add to piles
            4. see if piles >= k -> return

        left, right pointers at 1, max(candies)

        midpoint -> check can_split
            If can -> left = m + 1
            If can't -> right = m - 1

        Once we're done, it should be left = the ans
        '''
        max_value = max(candies)

        def can_split(n: int) -> bool:
            if n > max_value:
                return False

            piles = 0

            for candy in candies:
                piles += candy // n

            return piles >= k

        # Start at 1, max(candies). It cannot be 0
        left, right = 1, max(candies)

        # We're adjusting left when we find target, so it will be right biased -> return right
        while left <= right:
            m = (left + right) // 2

            if can_split(m):
                left = m + 1

            else:
                right = m - 1

        return right
```

### 2560. House Robber IV

**Code:**

```
class Solution:
    def minCapability(self, nums: List[int], k: int) -> int:
        '''
        We're trying to minimize the max, so we can just binary search the range of (min(nums), max(nums)) to find the minimum value that we can get to k houses robbed with.

        Time complexity:
            - Binary Search: M = max - min, O(n logM)
            - DP: O(n * k), which rounds to O(n^2)

        So let's go with Binary Search:

        Search range: min(nums), max(nums) -> left, right

        Helper function:
            def can_rob(num: int, index: int, count: int) -> bool:
                calculate if we can rob k number of houses by using a max of num, greedy will work because:

                Why would we ever skip a house we can rob? We should never, because even if we can rob the next house, then we'd just be locking ourselves out of the house after that for no reason.

        Binary Search:
        while left <= right:
            mid = (left + right) // 2

            if can_rob(mid, 0, 0):
                right = mid - 1

            else:
                left = mid + 1

        quick run:
        [2,3,5,9]


        2, 3, 4, 5, 6, 7, 8, 9
        l        m           r
        l     r  l

        1. check 5 -> True -> right = 4
        2. Check 3 -> False -> left = 4
        3. Check 4 -> False -> left = 5 -> return left
        '''

        def can_rob(num: int) -> bool:
            count = 0
            index = 0
            while index < len(nums):
                if nums[index] <= num:
                    count += 1
                    if count == k:
                        return True

                    index += 2
                else:
                    index += 1

            return False

        left, right = min(nums), max(nums)

        while left <= right:
            mid = (left + right) // 2

            if can_rob(mid):
                right = mid - 1

            else:
                left = mid + 1

        return left
```

### 778. Swim in Rising Water

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
