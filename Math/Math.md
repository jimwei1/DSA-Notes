# Math

## Numbers

### Notes:

- Truncating a number:
  - `int(x / y)`
- Check if a number is a whole number:
  - `n % 1 == 0`
- Log Base 4:
  - `log(n, 4)`
  - Log cannot be called on 0 or negative numbers.
  - `/` is division, `//` is floor division

## Log

## Prime Numbers

- A number is prime if not cleanly divisible by any number between `[2, int(sqrt(num))]`

1. [Numbers](#numbers)
2. [Log](#log)
3. [Prime Numbers](#prime-numbers)
4. [Summation](#summation)

## Summation

`1 + 2 + ... + k = (k(k + 1)) / 2`

1. `m + 2m + 3m + ... + km`
2. `m(1 + 2 + 3 + ... + k)`
3. `m * (k(k + 1)) / 2`

## Powers

### Power of Two

#### Bit Manipulation

A power of two in binary:

```Python3
1 --> 0001
2 --> 0010
4 --> 0100
8 --> 1000
16 --> 10000
```

Therefore, if we subtract 1 from a power of 2, we'll get all 1's before it.

```Python3
n:     1000
n - 1: 0111
----------------
n & (n - 1): 0000
```

  Therefore:

```Python3
def isPowerOfTwo(n: int) -> bool:
	return n > 0 and (n & (n - 1)) == 0
```

### Power of Four

#### Bit Manipulation

```
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
		return n > 0 and n & (n-1) == 0 and n % 3 == 1
```

## Hexadecimal / Hexatrigesimal Conversion

```
def to_hexatrig(x: int) -> str:
	ret = []
	while x > 0:
		x, rem = divmod(x, 36)
		curr = -1

		if 0 <= rem <= 9:
			curr = rem
		else:
			curr = chr(ord('A') + (rem - 10))
		ret.append(str(curr))

	return ''.join(reversed(ret))

def to_hexadec(x: int) -> str:
	ret = []
	while x > 0:
		x, rem = divmod(x, 16)
		curr = -1

		if 0 <= rem <= 9:
			curr = rem
		else:
			curr = chr(ord('A') + (rem - 10))
		ret.append(str(curr))

	return ''.join(reversed(ret))
```

## Subsequences

### # of subsequences for length n

- `2^n, including the empty subsequence -> `2^(n-1)` to exclude subsequence

### Problems:

#### 2894. Divisible and Non-divisible Sums Difference

```
class Solution:
    def differenceOfSums(self, n: int, m: int) -> int:
        '''
        k = n / m

        num2 -> kth such number divisible by m is k * m

        num2 = m + 2m + ... + k*m = (1 + 2 .... + k)m = (k(k+1)/2) * m

        num1 is just the sum of the first n integers minus num2

        -> (n(n+1)) / 2 - 2*num2
        '''
        k = n // m

        num2 = (k * (k + 1) // 2) * m

        num1 = ((n * (n + 1)) // 2) - num2

        return num1 - num2

```

### Notes:

#### Find all Prime Numbers in a Range (Sieve of Eratosthenes)

**Idea:**

- Instead of finding all the primes in the range, just find all the non primes.

**Pseudocode:**

1. Create array of nums `range(0, right)`. Assume all to be true for now.
2. `0, 1` are not prime.
3. Starting at 2, mark all of its multiples as non prime.
   1. If a number is already marked, skip marking multiples
   2. Go until `sqrt(n)`.
      1. We go until sqrt(n) as all composite numbers after sqrt(n) will already be marked previously.
      2. A composite number x must have at least one prime factor <= sqrt(x)
      3. We've already identified all primes <= sqrt(n), and marked all of their multiples.
      4. Therefore, any composite numbers remaining in the array would have to have a prime factor > sqrt(n).
      5. Since all numbers in the array are <= n, this cannot be possible.

##### Problems:

- **2523. Closest Prime Numbers in Range**

```
class Solution:
    def closestPrimes(self, left: int, right: int) -> List[int]:
        # For each number in [left, right], find primes
        # Then, find minimum diff of primes

        # Optimize with Sieve of Eratosthenes

        prime_array = [True] * (right + 1)
        prime_array[0] = prime_array[1] = False

        # For each number from [2, sqrt(n)], mark all of its multiples as False
        # We go until sqrt(n) as all composite numbers after sqrt(n) will already be marked previously.
        # A composite number x must have at least one prime factor <= sqrt(x)
        # We've already identified all primes <= sqrt(n), and marked all of their multiples.
        # Therefore, any composite numbers remaining in the array would have to have a prime factor > sqrt(n).
        # Since all numbers in the array are <= n, this cannot be possible.

        for n in range(2, int(sqrt(right)) + 1):

            # Skip if the current number is already composite, as that means we've already checked all of its multiples previously
            if prime_array[n] == False:
                continue
            curr = n** 2
            while curr <= right:
                prime_array[curr] = False
                curr += n

        primes = []

        for i in range(len(prime_array)):
            if prime_array[i] == True and (left <= i <= right):
                primes.append(i)

        min_diff = float('inf')
        ans = [-1, -1]

        for j in range(len(primes) - 1):
            diff = primes[j + 1] - primes[j]

            if diff < min_diff:
                min_diff = diff
                ans = [primes[j], primes[j + 1]]


        return ans
```
