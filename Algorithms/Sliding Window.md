# Sliding Window

## Notes:

- When while looping through count, don't initialize each count as a variable or they won't be updated.
  - e.g.
    ```
    # this is bad
    while a_count > 0 and b_count > 0 and c_count > 0
    # this is good
    while counts['a'] > 0 and counts['b'] > 0 and counts['c'] > 0:
    ```
- Usually the pattern is:

  ```
  while right < length:
    Expand window by including right pointer
    Update window state, process current window

    while condition_to_shrink:
        ...
        left += 1

    Update result based on current valid window

    right += 1
  ```

- Can use “matches” as another variable instead of comparing two arrays of letter counts
- Even if the window size is constant, it might make more sense just to start the window size at 0 anyways and check for when its constant
- To save time complexity, instead of rebuilding the answer, just remove the last element and add the first when you shift sliding window
  - Remove, shift window, then add
- **Better to, instead of r += 1, just for r in range(len(r))**

## Problems:

### 1358. Number of Substrings Containing All Three Characters

```
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        # Sliding window with count of each character
        # As long as each character's count > 0, left += 1, ans += (length - right), as this is all the possible combinations after this window
        # Else, right += 1

        length = len(s)
        left, right = 0, 0
        ans = 0
        counts = defaultdict(int)


        while right < length:
            counts[s[right]] += 1

            while counts['a'] > 0 and counts['b'] > 0 and counts['c'] > 0:
                length_diff = length - right
                ans += length_diff
                counts[s[left]] -= 1
                left += 1

            right += 1

        return ans
```

## Manacher’s Algorithm

- Finding Longest Palindromic substring
  1. Insert # between each character, and start and end
  2. Iterate through transformed string, treating each character as potential center. Keeps track of length/2 for each center
  3. Check for symmetry
