# Backtracking

## Notes:

- Basically go forward and then recursive call. After that, reverse with pop() or something
  - Here, return is used to break out of a single path, not the whole backtracking function.
- Use ans.appenarra(curr.copy()) because we’re modifying curr while we append.
  - Since curr (array) is mutable, the array stored in curr is also modified when curr is modified with .pop() in future iterations.
- To remove duplicates, usually sort and then check previous to see if it’s the same
- Combination Sum:
  - Make the decision on whether to include each index repeatedly until index ≥ len or total > target, branch out from there
- Need to return out of base case

### Problems:

#### 90. Subsets II

**Code:**

```
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        '''
        If we need to return all subsets -> backtracking (has to be brute force)

        2 choices -> either use or dont use each

        To avoid duplicate subsets, when we choose to skip a single value, we should skip ALL those values in nums (or else we will make the same choice again later, which will cause a duplicate)
        '''
        ans = []
        nums.sort() #n log n
        curr = []

        def backtrack(index: int):
            nonlocal curr
            if index == len(nums):
                ans.append(curr[:])
                return

            curr.append(nums[index])
            backtrack(index + 1)
            curr.pop()

            # skip
            next_index = index + 1
            while next_index < len(nums) and nums[index] == nums[next_index]:
                next_index += 1
            backtrack(next_index)

        backtrack(0)
        return ans
```
