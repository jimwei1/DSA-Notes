# Statistics

## Statistics

## Combinatorics

### Stars and Bars Theorem

#### Question

This theorem answers the question: How many non-negative integer solutions are there to the equation

$$x_1 + x_2 + \dots + x_k = n$$

where:

- $n$ is a non-negative integer
- $k$ is the number of bins
- Each $x_i \geq 0$

#### Theorem

The number of non-negative integer solutions is given by:

$$\binom{n + k - 1}{k - 1}$$

#### Intuition

- Think of the $n$ units as **stars:**
  - $\star \star \star \dots \star$ (total of $n$)
- You want to divide these into $k$ groups using $k - 1$ dividers
- Each placement of the bars splits the stars into bins, which are the values of $x_1, x_2, \dots, x_k$
- Since there are $n$ stars and $k - 1$ dividers to place, the total number of arrangements of these $ n + k - 1$ symbols (stars and bars)
  is:

$$\binom{n + k - 1}{k - 1}$$

#### Positive Integers only

If we're only looking for positive integer solutions, i.e. $x_i \geq 1$, then we have to first give one item to each bin.

$$x_1 + x_2 + \dots + x_k = n \text{, with } x_i \geq 1$$

Let:

$$x_i' = x_i - 1 \quad \text{so that} \quad x_i' \geq 0$$

Then:

$$x_1' + x_2' + \dots + x_k' = n - k$$

Now we can apply the non-negative form:

$$\binom{(n - k) + k - 1}{k - 1} = \binom{n - 1}{k - 1}$$

#### Examples

- **Example 1:** How many ways to distribute 5 identical candies among 3 kids?

  $$
  \binom{5 + 3 - 1}{3 - 1} = \binom{7}{2} = 21
  $$

- **Example 2:** How many ways to write 10 as the sum of 4 **positive** integers?

  $$
  \binom{10 - 1}{4 - 1} = \binom{9}{3} = 84
  $$

## Summation

`1 + 2 + ... + k = (k(k + 1)) / 2`

1. `m + 2m + 3m + ... + km`

2. `m(1 + 2 + 3 + ... + k)`

3. `m * (k(k + 1)) / 2`

### Problems:

#### Count Number of Trapezoids I

```
class Solution:
    def countTrapezoids(self, points: List[List[int]]) -> int:
        '''
        given:
            - points --> 2d int array
                - points[i] = [x_i, y_i]

        return:
            - # of unique horizontal trapezoids from 4 distinct points from points

        horizontal trapioezoid -> same y value for pairs

        for pairs = [p1, p2, ..., pk]

        1. Group into y values
        2. for each y value, ans *= # of pairs

        we want to find:
            sum over all 1<i<j<k: p_i * p_j

        to calculate p_i -->
            1. Find c, count
            2. p_i = c(c-1) // 2

        Need to do this in 1 pass -> running prefix

        sum of all p_i * p_(i-1) --> p_i * (p_1 + ... + p*(i-1)) --> p_i * running sum
        '''
        MOD = 10**9 + 7
        dict = defaultdict(int)

        for x, y in points:
            dict[y] += 1

        ans = 0
        r = 0

        for c in dict.values():
            if c < 2:
                continue
                
            p = ((c * (c - 1)) // 2) % MOD

            ans += (p * r) % MOD
            r += p

        return ans % MOD

```
