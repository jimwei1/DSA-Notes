# Sorting Algorithms

## Merge Sort

1. Split the list in half
2. Recursively sort each half
3. Merge the sorted halves

TC: O(nlogn)

- Merging two lists of length n --> O(n)
- Recursion is O(logn) as it's split in half
- Slicing costs O(n)

SC: O(n)

- Call stack: O(logn)
- ret array: max O(n)

```
def mergeSort(nums: list[int]) -> list[int]:
	n = len(nums)
	if n == 1:
		return nums
	
	mid = n // 2

	left_half = mergeSort(nums[:mid])
	right_half = mergeSort(nums[mid:])

	return merge(left_half, right_half)

def merge(left: list[int], right: list[int]) -> list[int]:
	ret = []
	l = 0
	r = 0

	while l < len(left) and r < len(right):
		if left[l] < right[r]:
			ret.append(left[l])
			l += 1
		else:
			ret.append(right[r])
			r += 1
	
	while l < len(left):
		ret.append(left[l])
		l += 1

	while r < len(right):
		ret.append(right[r])
		r += 1
	return ret

return mergeSort(nums)
                
```
