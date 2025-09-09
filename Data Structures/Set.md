# Set

## Properties

- No duplicates
- Can check membership in O(1) time
- `set.add()` and `set.remove()`
  - Both O(1)
- Can’t put an array in a set, as arrays are mutable. However, you can convert the array into a tuple first

## Fast way to remove duplicates in array:

- unique = set(array)

## Combine two sets (Set Union)

```
digits = set(str(hours)).union(set(str(mins)))
```

## Check if all items in set x are in set y

```
x = {"a", "b", "c"}
y = {"f", "e", "d", "c", "b", "a"}

z = x.issubset(y)
```
