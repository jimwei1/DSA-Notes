# 1D Dynamic Programming

## Notes:

### Memoization:

- Usually implemented after a brute-force recursive DFS
- Steps:
  1. Create self.memo = {}
  2. Check if the problem we’re doing is already in memo. If so, return that value
  3. Otherwise, do the problem and put answer in self.memo
- Keep an array of all previous work
- Climbing stairs / Fib

## Problems:

### 0/1 Knapsack

#### Target Sum

#### 416. Partition Equal Subset Sum

**Intuition:**

1. So we can backtrack then memoize, but that's:
   1. Time Complexity: `O(n * target_sum)`
   2. Space Complexity: `O(n * target_sum)`
2. Instead, we can:

   1. Keep a set of current sums, then add each new value to each in the set.

      1. Time Complexity: `n * target_sum`
      2. Memory Complexity: `target_sum`

      ```Python3
        target_sum = sum_nums // 2

        seen = set([0])

        for num in nums: # O(n)
            curr_seen = seen.copy()
            for prev_value in curr_seen: # Worst case O(target_sum)
                seen.add(prev_value + num)
            if target_sum in seen:
                return True

        return False
      ```

   2. Keep a dp array of length `target_sum + 1`. Then, for each num, loop from target_sum down to num and set it equal to `dp[j] or dp[j - num]`. This is whether or not we can reach the new number.

      1. This will build a bool array of whether the next sum is reachable
      2. Time Complexity: `n * target_sum`
      3. Memory Complexity: `target_sum`

      ```Python3
        for num in nums:
            for j in range(target, num - 1, -1):
                dp[j] = dp[j] or dp[j - num]
      ```

**Code:**

```Python3
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        '''
        Target sum of nums // 2

        Backtracking + cache
            Time Complexity: n * target_sum
            Memory Complexitity: n * target_sum

        To solve target sum, keep a set of current sums, then add the new value to each in the set
            Time Complexity: n * target_sum
            Memory Complexity: target_sum


        for num in nums:
            for j in range(target, num - 1, -1):
                dp[j] = dp[j] or dp[j - num]

        [1, 5, 11, 5]

        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22]
        [1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1,  1,  1,  0   0,  0,  0,  0,  0,  0,  0   1,  1, ]
        '''
        sum_nums = sum(nums)
        if sum_nums % 2 != 0:
            return False

        target_sum = sum_nums // 2

        seen = set([0])

        for num in nums: # O(n)
            curr_seen = seen.copy()
            for prev_value in curr_seen: # Worst case O(target_sum)
                seen.add(prev_value + num)
            if target_sum in seen:
                return True

        return False
```

### DP + Binary Search

#### 3557. Find Maximum Number of Non Intersecting Substrings

```Python3
class Solution:
    def maxSubstrings(self, word: str) -> int:
        '''
        We want to maximize the count of non-overlapping valid substrings ending at or before i. Note only the rightmost j, where `j ≤ i-3 with word[j] == word[i]` is required. dp is non decreasing.

        dp[k] = maximum number of valid substrings in the prefix word[0..k-1]
        dp[0] = 0

        For each i from 0 to n-1 (processing word[i]):

            # 1) Option: don’t end at i
            dp[i+1] = dp[i]

            # 2) Option: end a valid substring at i
            #    A valid substring starts at some j ≤ i-3 with word[j] == word[i].
            #    Among all those, we only need the rightmost j because dp is non-decreasing.
            last_j = the largest index ≤ i-3 with word[last_j] == word[i]
            if last_j exists:
                dp[i+1] = max(dp[i+1], dp[last_j] + 1)

        Answer = dp[n]
        '''

        def bin_search_rightmost(array: List[int], target: int) -> int:
            '''
            Input: Array of all indecies of word[i], target index
            Output: returns rightmost index from the target, or -1 if not found
            '''

            left, right = 0, len(array) - 1
            ret = -1

            while left <= right:
                mid = (left + right) // 2

                if array[mid] <= target: # look for better one
                    ret = array[mid]
                    left = mid + 1

                else:
                    right = mid - 1

            return ret

        n = len(word)
        indecies = defaultdict(list)

        for index, letter in enumerate(word):
            indecies[letter].append(index)

        dp = [0] * (n + 1) # dp[i] = max valid substrings that end by i
        ans = 0

        for i in range(1, n):
            curr_letter = word[i]
            search = indecies[curr_letter]
            target = i - 3
            j = bin_search_rightmost(search, target)

            if j != -1:
                dp[i + 1] = max(dp[i], dp[j] + 1)
            else:
                dp[i + 1] = dp[i]

        return dp[n]
```
