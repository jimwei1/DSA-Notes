# Python

## Range(start, end)

- Inclusive start, exclusive end

## For Loops

- If you want to change the values in an array, you have to loop through the index, not the values.

## F strings:

- To pad int in f string with 0’s in front: (at least 2 digits)
  ```
  f"{hours:02}:{mins:02}"
  ```

## if vs elif

- Let’s say:

```
if
if
if
else <-- this is tied to the 3rd if

on the other hand,

if
elif
elif
else <-- this is tied to the first if
```

## zip()

- zip(string1, string2) [can also use lists ofc]
  - Basically iterate through both at once as tuple (string1[0],string2[0])
- Let’s say you have 3 arrays that all map the same things, like startTime, endTime, and profit.

```
intervals = sorted(zip(startTime, endTime, profit)

#IT WILL SORT BASED ON STARTTIME, FIRST ONE IN ZIP TUPLE
```

## Letter Mapping (for array)

- `ord(letter) - ord('a')`
  - Maps between 0 (a) and 25 (z)
- Also, **26** letters in the alphabet.
- To retrieve:
  - `chr(index + ord('a'))`

## Tuple Assignment

- Also, tuples are immutable but can have any size when first created.
- Instead of
  ```
  temp = nums[l]
  nums[l] = nums[r]
  nums[r] = temp
  ```
- We can do:
  ```
  nums[l], nums[r] = nums[r], nums[l]
  ```
