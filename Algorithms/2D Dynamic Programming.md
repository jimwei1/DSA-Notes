# 2D Dynamic Programming

## Table of Contents:

1. [Table of Contents:](#table-of-contents)
2. [Notes:](#notes)
3. [Problems:](#problems)

## Notes:

### Unbounded Knapsack

- Item can be chosen an unlimited number of times
- Order doesn't matter

**Examples:**

- **518. Coin Change 2**

## Problems:

### NC 150

#### Longest Palindromic Substring

- 2D dp array, n x n size
- cell is true if substring i to j (inclusive) is palindrome

#### 1143. Longest Common Subsequence

**Idea:**

- Create 2D DP array, one string per side
- **Subproblem:**
  - Do we want to remove the letter? (For each string in both strings)
- Each step is a decision tree. We decide whether to shift the subproblem or not
- Let `i, j = index of string1, index of string2`
- Movements:
  - Go diagonally on the DP array if `i == j`. Add 1 here.
  - Else, Go either right or down.
  - Either way, we take the max of the movement and put it as the value of the original cell
  - If we go out of bounds, return 0 -> **Base Case: 0 for all out of bounds**
- **We extend both suffixes if it matches. Otherwise, we skip one of the two letters so we can go through the decision tree.**

**Psudocode:**

1. Initialize 2D DP array, all == 0. Length should be the length of strings **+ 1**
2. Loop through starting from bottom right, work our way up
3. If `text1[i] == text2[j]`, check the diagonal and + 1
4. Else, check the right and down and take the max.
5. Return `dp[0][0]`

**Code:**

```
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # 1. Initialize 2D DP Array
        height = len(text1) + 1
        width = len(text2) + 1

        dp = []

        for _ in range(height):
            row = [0] * width
            dp.append(row)

        # 2. Loop bottom up

        for i in range(len(text1) - 1, -1, -1):
            for j in range(len(text2) - 1, -1, -1):
                a = text1[i]
                b = text2[j]
                if a == b:
                    dp[i][j] = dp[i+1][j+1] + 1
                else:
                    right = dp[i][j+1]
                    down = dp[i+1][j]
                    dp[i][j] = max(right, down)

        return dp[0][0]
```

#### 309. Best Time to Buy and Sell Stock with Cooldown

**Idea:**

- Decision Tree:
  - 1. Cooldown (Skip)
  - 2. Buy (Subtract cost) / Sell (Add cost)
- Sell requires 1 cooldown after
- Keep track of total while we go
- We can do this recursively, then add caching to make it optimal.
  - The cache would be `(index, bool)`, where bool is decision `1 = False`, or `2 = True`.

##### DFS (Recursive):

**Psudocode:**

1. Create recursive helper function with parameters: `index: int, buy: bool, choice: bool, total: int`
2. Recursively call, and pass up the max total as a return

**Code:**

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Cache: dp = [index, choice]

        dp = []

        def dfs(index: int, buy: bool, cooldown: bool) -> int:

            if index >= len(prices):
                return 0

            # Cooldown = True -> Can't buy
            if cooldown == True:
                return dfs(index + 1, True, False)

            # buy = True -> Can buy
            elif buy == True:
                # Choice 1: Buy
                a = dfs(index + 1, False, False) - prices[index]

                # Choice 2: Skip
                b = dfs(index + 1, True, False)

                return max(a, b)

            # buy = False -> Can sell
            elif buy == False:
                # Choice 1: Sell
                a = dfs(index + 1, True, True) + prices[index]

                # Choice 2: Skip
                b = dfs(index + 1, False, False)

                return max(a, b)


        return dfs(0, True, False)
```

##### DFS: Add Caching:

**Code:**

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Cache: dp = [index, choice]

        dp = {}

        def dfs(index: int, buy: bool, cooldown: bool) -> int:

            if (index, buy, cooldown) in dp:
                return dp[(index, buy, cooldown)]

            if index >= len(prices):
                return 0

            # Cooldown = True -> Can't buy
            if cooldown == True:
                ret = dfs(index + 1, True, False)
                dp[(index, buy, cooldown)] = ret
                return ret

            # buy = True -> Can buy
            elif buy == True:
                # Choice 1: Buy

                a = dfs(index + 1, False, False) - prices[index]

                # Choice 2: Skip
                b = dfs(index + 1, True, False)

                dp[(index, buy, cooldown)] = max(a, b)

                return max(a, b)

            # buy = False -> Can sell
            elif buy == False:
                # Choice 1: Sell
                a = dfs(index + 1, True, True) + prices[index]

                # Choice 2: Skip
                b = dfs(index + 1, False, False)

                dp[(index, buy, cooldown)] = max(a, b)

                return max(a, b)


        return dfs(0, True, False)
```

**Note:** Can slightly optimize this with 2 parameters instead of 3, and just use a `state: int` variable for the cooldown phase.

#### 518. Coin Change 2

**Idea:**

- Unbounded Knapsack
  - Item can be chosen an unlimited number of times
  - Order doesn't matter
- **Avoid duplicates by only choosing between coins to the right of the index**
- 2D array - `(index, total)`
- **Base Case:** `total = 0, value = 1`
- Bottom up DP

##### DFS (Recursive):

**Psudocode:**

1. ans = 0
2. Create dfs function with parameters: `index: int, total: int`
3. Options:
   1. Stay on the same index
   2. Go to the next index
4. Stop when amount > total. If amount = total, ans += 1.

**Code:**

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # Keep track of remaining instead of total

        def dfs(index: int, remaining: int) -> int:
            if remaining == 0:
                return 1

            if remaining < 0 or index >= len(coins):
                return 0

            # Use coin
            use_coin = dfs(index, remaining - coins[index])

            # Skip coin
            skip_coin = dfs(index + 1, remaining)

            return use_coin + skip_coin

        return dfs(0, amount)
```

##### DFS (Top Down DP)

**Code:**

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # Keep track of remaining instead of total

        # dp keys are index, remaining
        dp = {}

        def dfs(index: int, remaining: int) -> int:
            if (index, remaining) in dp:
                creturn dp[(index, remaining)]

            if remaining == 0:
                dp[(index, remaining)] = 1
                return 1

            if remaining < 0 or index >= len(coins):
                dp[(index, remaining)] = 0
                return 0

            # Use coin
            use_coin = dfs(index, remaining - coins[index])

            # Skip coin
            skip_coin = dfs(index + 1, remaining)

            dp[(index, remaining)] = use_coin + skip_coin
            return use_coin + skip_coin

        return dfs(0, amount)
```

##### Optimal (Bottom Up DP)

**Idea:**

- **Base Case:** if amount = 0, value = 1

**Notes:**

- Bottom up and top down have same time complexity, as we have to fill out the whole array either way.
- Lower space complexity though

#### Minimum Cost Path with Alternating Directions II

```
class Solution:
    def minCost(self, m: int, n: int, waitCost: List[List[int]]) -> int:
        '''
        input:
            m -> rows
            n -> cols
            waitCost -> 2D int array, cell defines cost to wait on the cell

        - To enter a cell, (i+1) * (j+1) cost
        - Odd:
            Move right or down, pay entry cost
        - Even:
            Wait in place, pay waitCost[i][j]

        return min cost to reach (m - 1, n - 1)
        ________________________________________________________________________
        We don't need to BFS or DFS, since it's a DAG + topological order

        1 Pass DP:
            - dp[i][j] = min cost to reach cell
                Reachable from dp[i-1][j], dp[i][j-1]

            Base Case: dp[0][0] = 1
            Recurrence Relation: dp[i][j] = min(dp[i-1][j], dp[i][j-1])
        '''

        def cell_weight(i: int, j: int) -> int:
            entry = (i+1) * (j+1)
            wait = waitCost[i][j]

            if i == m - 1 and j == n - 1:
                return entry
            else:
                return entry + wait
                
        dp = [[float('inf')] * n for _ in range(m)]
        dp[0][0] = 1

        for i in range(m):
            for j in range(n):
                cell_cost = cell_weight(i, j)

                # Only pull from up if i > 0
                if i > 0:
                    dp[i][j] = min(dp[i][j], dp[i-1][j] + cell_cost)

                # Only pull from left if j > 0
                if j > 0:
                    dp[i][j] = min(dp[i][j], dp[i][j-1] + cell_cost)

        return dp[m-1][n-1]



            
                ©leetcode
```
