# String

## Properties

- **strings are immutable, cannot modify them in place**
  - Instead, use:
  ```
  listString = list(string)
  “”.join(listString)
  ```

### Reverse

`string[::-1]`

### Split

`string.split()`

- By default, splits a string at its spaces
- This is NOT in place

### Convert to List

`list(string)`

### Append to string:

`string += append`

- Length of a substring:
  - Start is inclusive, end is exclusive, so the length of ao substring is r - l
  - However, if you’re manually entering two index, they would both be inclusive, so the length would be r - l + 1
- Check if letter:
  - letter.isalpha()
- Check if number:
  - letter.isdigit()
  - only checks whole, positive numbers
- Remove parenthesis from string:
  - s.replace("(", "").replace(")", "")
- **If you slice a string OR array past its end, it will return up until the end!**

### Check Substring:

`if string1 in string2`

#### Capitalize letter

`letter.upper()`

#### Remove dashes

`string = string.replace('-', '')`

## Letters

### Check if Letter is Alphabetical Letter / Digit

`string.isalnum()`
