# Kadane’s Algorithm:

- Finding the max sum of contiguous subarray
- O(n) time

1. Start with maxSum and currSum
2. Loop through Array, add each element to currSum
3. If currSum < 0, set it back to 0 (more optimal to start new subarray)
4. Update maxSum
