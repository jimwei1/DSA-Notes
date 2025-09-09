# Hashmap

- Sorting keys then getting top keys:
  - sorted_keys = sorted(dictionary, key=dictionary.get, reverse=False)
  - return sorted_keys[:k]
- dictionary.values()
  - returns a list of all values
- Finding max count and then returning the key:
  - **max(dictionary, key=dictionary.get)**
  - min(dictionary, key=dictionary.get)
- Finding max key and returning the key:
  - max(dictionary.keys())
- Have to initialize everything before attempting to call it in any way (even if you’re checking for its existence)
- An easy way to set something to 0 if it doesn’t exist, or increment by 1:
  - dictionary[key] = dictionary.get(key, 0), + 1
  - If 0 is not yet a key, it will be initialized with value 0
  - For an array, just [0] \* n, then += 1
- **defaultdict(int) makes default 0 as well.**
- Checking for membership in hashmap is O(1) time, while array is O(n)
- hashmap.pop(key, None) removes it from dictionary.
- defaultdict(list) makes a list by default
- Looping through dictionary:
  - Can’t have “for index in sums.values() cuz dictionaries don’t guarantee order

```
curr = 0
        while curr in sums:
            ans.extend(sums[curr])
            curr += 1
```

- To use an array as a key:
  - Since arrays are mutable, they cannot be used as a key
  - So, we do tuple(array) —> tuple is immutable, the hash of this is then the key
  - e.g. dictionary[tuple(array)]y
- To check if all values are 0:
  ```
  all(value == 0 for value in dictionary.values())
  or
  sum(dictionary.values()) == 0
  ```

## Dictionary Comprehension

- To make a count dictionary:
  - d = {0: i for i in range(len(nums))}
- To set to 0 or add 1:
  - count[s[r]] = 1 + count.get(s[r], 0)
- To get max value of dictionary:
  - count = {}
  - max(count.values()
- To make a matrix of same size:
  ```
  m = len(matrix)
  n = len(matrix[0])
  counts = [[0] * n for _ in range(m)]
  ```

## Deleting from Dictionary

- `del dictionary[val]`
- `dictionary.pop(val)`

## List Comprehension

- To remove “(” and “)” from a list:
  ```
  filtered_word = [letter for letter in word if letter not in ["(",")"]]
  ```
- To convert string to list:
  ```
   word = [letter for letter in s]
  ```
