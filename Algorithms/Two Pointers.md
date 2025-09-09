# Two Pointers

- While loop: while l < r
- Or, if r is going through every value anyways:
  - for r in range(len(nums))

## Problems

### 1498. Number of Subsequences That Satisfy the Given Sum Condition

```
class Solution:
    def numSubseq(self, nums: List[int], target: int) -> int:
        '''
        input:
            nums -> array, int, 1 <= len <= 10^5
                1 <= num <= 10^6
            target -> int
                1 <= target <= 10^6
        
        output:
            ans -> int, MOD 10^9 + 7
        _____________________________________________
        # of non-empty subsequences of nums such that the sum of min and max in sub is <= target
        
            - Sort the array
                - For each index, check where the max index can be
                - Then, each index between can be either included or excluded, so 2^(j-i)
                    - Don't exclude the empty subarray, because we're starting from i+1
                - Precompute 2^x
                - Two pointer -> Start right pointer at the very end and shrink as needed. It will never have to go to the right.

        TC: O(nlog(n))
            - Sort -> nlogn
            - Sliding window -> O(n)
        SC: O(n)
            
        '''
        n = len(nums)
        pow2 = [1] * n
        MOD = 10**9 + 7

        for i in range(1, n):
            pow2[i] = (pow2[i-1] * 2) % MOD

        nums.sort()
        ans = 0

        right = n - 1

        for left in range(n):
            while left <= right and nums[left] + nums[right] > target:
                right -= 1
            
            if right >= left:
                ans += pow2[right - left]
        
        return ans % MOD
```
