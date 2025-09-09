# Stack

## Problems:

### 388. Longest Absolute File Path

```
class Solution:
    def lengthLongestPath(self, input: str) -> int:
        # Probably can't skip steps as the file name will impact the length
        # Stack to keep track of lengths, add 1 to each for the \

        max_length = 0

        stack_length = 0
        stack = []

        # Split string into \n's

        input_array = input.split('\n')

        for line in input_array:

            # level = # of \t's. If curr_level < stack_level, have to pop until equal.
            curr_level = 0
            stack_level = len(stack)

            i = 0
            # Calculate # of levels first, and isolate name
            while line[i] == '\t':
                curr_level += 1
                i += 1

            line_name = line[i:]

            if curr_level < stack_level:
                level_diff = stack_level - curr_level

                for _ in range(level_diff):
                    stack_length -= stack.pop()

            if "." in line_name:
                curr_length = stack_length + len(line_name) + len(stack) # This is number of \'s
                max_length = max(max_length, curr_length)
                continue

            else: # Need this so we don't append a file
                stack.append(len(line_name))
                stack_length += (len(line_name))

        return max_length
```

### 3561. Resulting String After Adjacent Removals

```
class Solution:
    def resultingString(self, s: str) -> str:
        '''
        while len(s) >= 2
            remove leftmost pair of adjacent characters that are consecutive
            shift the remaining characters left to fill the gap

        return the resulting string

        TC O(n^2) -> brute force, probably not optimal

        A doubly linked list would be useful, but O(n) space complexity...
        A stack on both sides

        Or, a single stack
        '''
        if len(s) == 1:
            return s

        def is_consecutive(a: str, b: str) -> bool:
            return abs(ord(a) - ord(b)) == 1 or (a == 'a' and b == 'z') or (a == 'z' and b == 'a')


        stack = []

        for letter in s:
            stack.append(letter)

            while len(stack) >= 2 and is_consecutive(stack[-1], stack[-2]):
                stack.pop()
                stack.pop()

        return ''.join(stack)
```
