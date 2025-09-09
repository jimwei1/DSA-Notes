# Neetcode 150

## Bit Manipulation

### 338. Counting Bits

#### Tags:

- [[Bit Manipulation]]
- [[1D Dynamic Programming]]

#### Brute force --> `O(nlogn)`

TC: `O(nlogn)`

```
class Solution:
    def countBits(self, n: int) -> List[int]:
        '''
        TC: O(nlogn)
        - # of bits in i equals log(i) + 1, n times --> nlogn
        '''
        def countOnes(m: int) -> int:
            count = 0
            while m:
                m &= (m-1)
                count +=1
            return count
        
        ans = [0] * (n+1)

        for i in range(n+1):
            ans[i] = countOnes(i)
        
        return ans
```

#### DP (MSB)--> `O(n)`

TC: `O(n)`

```
0 - 0000 --> 0
1 - 0001 --> 1
2 - 0010 --> 1
3 - 0011 --> 2
// Now, next 4 is just a repeat of 0 to 3 except with another 1
4 - 0100 --> 1 + dp[n-4]
5 - 0101 --> 1 + dp[n-4]
6 - 0110 --> 1 + dp[n-4]
7 - 0111 --> 1 + dp[n-4]
8 - 1000 --> 1 + dp[n-8]
```

- The offset is just the most significant 1. Subtracting offset clears the MSB, so we're counting 1 plus the remaining bits.
- Base Case: `dp[0] = 0`
- Recurrence Relation: Let offset be the most significant bit in integer representation
	- `dp[i] = 1 + dp[i-offset]`
- Offset increases when `i == offset * 2` --> `offset = i`

```
class Solution:
    def countBits(self, n: int) -> List[int]:
        '''
        DP --> TC, SC = O(n)

        - offset = MSB
        - Subtracting the offset clears the MSB, and we already know the 1's from remaining bits.

        Base Case: dp[0] = 0
        RR: dp[i] = 1 + dp[i-offset]

        when i == offset * 2, offset = i
        '''
        dp = [0] * (n + 1)
        offset = 1

        for i in range(1, n + 1):
            if i == (offset * 2):
                offset = i
            
            dp[i] = 1 + dp[i - offset]
        
        return dp
```

#### DP (LSB)

- Right shift (dropping LSB) `i >> 1` is the same as `i // 2` (floor division)
- `i & 1` is LSB (even or odd)
- `i = (i >> 1) * 2 + (i & 1)
- Number of set bits in `i` is `i >> 1 + (1 if last bit is 1, else 0)`
- Base Case: `dp[0] = 0`
- RR: `dp[i] = dp[i >> 1] + (i & 1)`

```
class Solution:
    def countBits(self, n: int) -> List[int]:
        '''
        DP --> TC, SC = O(n)
        '''
        dp = [0] * (n + 1)
        
        for i in range(1, n + 1):
            dp[i] = dp[i >> 1] + (i & 1)
        
        return dp
```

#### DP(Last SET Bit)

- For `i > 0`:
	- `i & (i-1)` removes the last set bit.
- Base Case: `dp[0] = 0`
- RR: `dp[i] = dp[i & (i-1)] + 1`
	- `dp[i & (i - 1)]` has exactly 1 fewer 1 bit than `i`
