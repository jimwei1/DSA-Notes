# Prefix Sum

## Notes

### Problems:

#### 560. Subarray Sum Equals K

**Code:**

```
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        '''
        Nums can be negative, so we can't sliding window

        We could brute force and check every subarray -> O*(n^2), O(1)

        We can avoid brute forcing with:
            - prefix / postfix sum

        Let's say we have a prefix sum -> we can avoid having to recalculate

        So at each index j, we have a prefix sum up to the current index

        We want to find how many times a prefix sum of `prefix[j] - k` has occured before, which tells us how many subarrays ending at j sum to k.

        This is because:
            The value of each subarray between index i to j is: `prefix[j] - prefix[i] = k`
            Rewrite this as: `prefix[i] = prefix[j] - k `

        Essentially, we want to **count how many prefix arrays we can remove from our current subarray and still get the answer**
        '''

        # Key: Prefix Sum, Value: Count
        prefix = defaultdict(int)

        curr_prefix = 0
        ans = 0
        prefix[0] += 1 # The empty prefix (assuming we don't chop any prefix) is 1

        for j in range(len(nums)):
            curr_prefix += nums[j]

            ans += prefix[curr_prefix - k]

            prefix[curr_prefix] += 1

        return ans
```
