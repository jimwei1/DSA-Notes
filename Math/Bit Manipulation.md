# Bit Manipulation

## Notes:

- bin(num) —> 0b…
  - As a string
- bit(num)[2:] —> just the binary, as a string
- Convert binary to decimal:
  - `int(binary, 2)`
- To fill zeros at the beginning until 32:
  - `bin(n).zfill(32)`
- Is odd:
  - `n % 2 == 1`
- Right-Most bit of binary string:
  - `n % 2`
- Divide by 2:
  - right shift by 1
  ```Python3
  divide_by_2 = num >> 1;
  ```
- Add 1:
  - Start at right
  - If char == 1, mark as 0
  - If char == 0, mark as 1 and end
  - If we reach end, append 1 to left

## Bit Shifting

- Time complexity is always O(1), single CPU instruction

### Floor division by 2 is the same as right shifting by 1

- `n // 2 = n >> 1`
- `i = (i // 2) + (i & 1) = (i >> 2) + (i & 1)`

### Find LSB

`lsb = n & 1`

### Remove LSB

 `i & (i-1)` removes the last set bit.

### Find MSB

- `msb = 1 << (num.bit_length() - 1)`
- Make a new binary by shifting `1` left by `bit_length - 1` times

### Setting Right-Most Bit / Least Set Bit to 0

`n & (n-1)`

```Python3
1100 --> 12
1011 --> 11
----
1000   ← same as n but with the lowest 1 bit cleared
```

### ## Number of Bits to Represent a Positive Integer `i`

`bits(i) = log(i) + 1`

### Count # of 1's in Binary Representation of Integer

#### Brian Kernighan's Algorithm

```
def count_ones(n):
	count = 0
	while n:
		n &= (n-1) # drops lowest 1
		count += 1
	return count
```

#### Mod 2

- `n % 2` gives the least significant bit (either 0 or 1)
- `n // 2` shifts the number right by one bit (floor division)

```
def count_ones(n):
    count = 0
    while n > 0:
        count += n % 2   # check last bit
        n //= 2          # shift right
    return count
```

### Binary Index Value Calculation

#### Notes:

- Finding the value of the `i` index of a binary value with pow:
	- `pow(2, i)` -> TC: `O(log(i))`
- With bit shifting:
	- `1 << i` -> TC: `O(1)`

#### Problems:

##### 2311. Longest Binary Subsequence Less Than or Equal to K

```
class Solution:
    def longestSubsequence(self, s: str, k: int) -> int:
        '''
        Greedy:
            - Right to left
            - Always take 0
            - Take 1 while sum <= k
        For a 1 bit, the contribution is 2^(n-i-1)
        TC: O(n)
        SC: O(1)
        pow(k) is O(logk * n), k is worst case = n -> O(nlogn)
        Bit shift is O(1 * n)
        1 << (n-1-i)
        '''
        n = len(s)
        ans = 0
        curr = 0
        for i in range(n-1, -1, -1):
            letter = s[i]
            if letter == "0":
                ans += 1
            else:
                add = 1 << (n-1-i)
                if curr + add <= k:
                    ans += 1
                    curr += add
        return ans
```

### [[Binary Math]]

### [[Bitwise Operations]]

### [[Bit Masking]]

## Bitwise Operations

- Do these on integers!!

### AND

- For each position, both have to be 1 to return 1.

```Python3
x & y
x &= y
```

### OR

- For each position, either one or both has to be 1 to return 1.

```Python3
x | y
x |= y
```

### NOT

- Returns the complement (opposite) of each bit
- Python's `~x` gives `-(x+1)`, so we tend not to use this in bit manpulation.

```Python3
~x
```

### XOR

- For each position, only one can be 1 to return 1.

```Python3
x ^ y
x ^= y
```

### Right Shift

- Shifts the bits of the number to the right and fills in 0
- Same as dividing by a power of `2^y`

```Python3
x >> y
```

### Left Shift

- Shifts the bits of the number to the left and fills in 0
- Same as multiplying by a power of `2^y`

```Python3
x << y
```
