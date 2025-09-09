# Arrays

## Notes:

### Make an ascending list up to a number

`array = list(range(nums))`

### Max Value:

`max(array)`

### Slicing:

- `array[:-k]`
  - Excludes last k elements
  - `array[:-1]` gets rid of last element
- `array[3:8]`
  - returns index 3 to 7
- `array[3:]`
  - returns index 3 to end
- `array[:5]`
  - returns up to index 4
- `array[::2]`
  - every other element
- `array[::-1]`
  - reverse the list

#### Reversing:

- `reversed(range(len(list))` to iterate through array from the back
- `array.reverse()`
  - Reverses array in place, returns None.
  - For not in place, use:
    - `reversed(array)`
    - `array[::-1]`

#### Removing:

- In place:
  - `del array[index]` OR ` array.pop(index)`
    **- DO NOT MODIFY A LIST WHILE YOU’RE ITERATING THROUGH IT**
  - Can use a stack in this case sometimes
- To remove duplicates:
  1. Sort
  2. Create 2 pointers, compare to the previous -> ` array.remove(value) —> O(n) time`

#### Insert at beginning

```Python3
array.insert(0, value)
```

#### Check for if index is out of range:

```Python3
if index ≥ len(array):
    return
```

#### Check for index:

```Python3
mid = array.index(value);
```

### Append and Remove Operations

```Python3
array.append()
array.remove()
```
  - Remove removes the first instance, `O(n)` time complexity worst case

### Loop in Reverse Order:

```Python3
range(m - 1, -1, -1)
```
- m-1 is start value (last index)
- -1 is stop value (exclusive)
- -1 is step value (negative)

### Reverse a section:

```Python3
s[start+1:end] = s[start+1:end][::-1]  # Reverse the section immediately
```

### Check for duplicates in array:

```Python3
if len(nums) > len(set(nums))
```

- Remove first 3 elements:
  ```Python3
  nums = nums[3:]
  ```
  - **NOTE:** If len(nums) < 3 and we do `nums = nums[3:]`, nums will become empty.

### Pick random element in an array

```Python3
random.choice(array)
```

### Sorting (row, col) based on its grid[row][col] value

```Python3
vals = [(row, col) for row in range(rows) for col in range(cols)]
vals.sort(key = lambda x: grid[x[0]][x[1]], reverse = True)
```

## Problems:

# Matrix / 2D Arrays

## Notes:

- To loop through all submatricies of size `k * k` in a `m * n` grid, we have to do:

```
for row in range(m - k + 1):
    for col in range(n - k + 1):
```

### Histogram Method

- Solves problems involving submatricies in a 2D grid
- Convert each row into a height histogram (each cell represents the **consecutive** count of 1's above it)
-



## Create Empty Array of Size Rows, Cols

- **NOTE:** The outer list comprehension creates rows, inner creates cols.

```
    ans = [[0 for _ in range(cols)] for _ in range(rows)]
```

### Brute Force Every Subarray

```
    m = len(mat)
    n = len(mat[0])
    for top in range(m):
    for left in range(n):
        for bottom in range(top, m):
            for right in range(left, n):
```

## Problems:

### 1504. Count Submatrices With All Ones

**Idea:**

- Use Histogram method, create a heights array where each value is the # of consecutive 1's above it
- Then, loop through each possible combo of column, and add the min_height of the histogram. Break if min_height hits 0.
  - Min_height = 0 means not a valid histogram
  - For each column range (j to k), min_height is the number of submatrices with those exact columns as boundaries
- Return total submatricies

**Code:**

```
class Solution:
    def numSubmat(self, mat: List[List[int]]) -> int:
        # Height Histogram

        m = len(mat)
        n = len(mat[0])

        heights = [0] * n

        count = 0

        for i in range(m):
            for j in range(n):
                # Increase consecutive 1's
                if mat[i][j] == 1:
                    heights[j] += 1

                # Reset if 0
                else:
                    heights[j] = 0

            # Go through each possible combination of columns
            for a in range(n):
                min_height = float('inf')
                for b in range(a, n):
                    # Find min height of the histogram in question
                    min_height = min(min_height, heights[b])

                    # If we reach a 0, no longer valid
                    if min_height == 0:
                        break
                    count += min_height

        return count
```
