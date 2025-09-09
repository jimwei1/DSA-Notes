# Bit Masking

## Notes:

- To flip the `i-th` bit
  - `mask |= (1 << i)`
- To check if all bits are 1:
  - `mask == (1 << n) - 1`
- Return the complement of a number (all bits flipped, XOR)

```Python3
#Compute bit length of n
l = n.bit_length()

## Compute 1-bits bitmask of length n
bitmask = (1 << l) - 1

return n ^ bitmask
```

## Problems:

### Minimum Moves to Clean the Classroom

```Python3
class Solution:
    def minMoves(self, classroom: List[str], energy: int) -> int:
        '''
        m * n grid classroom

        S -> starting
        L -> Litter
        R -> Reset to restore to full capacity
        X -> cannot pass
        . -> empty

        Moving costs 1 energy
        Going to R resets to energy

        dp -> only go to a new cell if we have greater than the last best energy we've seen
        '''

        m = len(classroom)
        n = len(classroom[0])
        e = energy

        start = (-1, -1)
        litter = []
        reset = set()
        obstacles = set()

        directions = [(-1, 0), (1, 0), (0, 1), (0, -1)]

        for row in range(m):
            for col in range(n):
                if classroom[row][col] == "S":
                    start = (row, col)
                elif classroom[row][col] == "L":
                    litter.append((row, col))
                elif classroom[row][col] == "R":
                    reset.add((row, col))
                elif classroom[row][col] == "X":
                    obstacles.add((row, col))

        if len(litter) == 0:
            return 0
        litter_index = {}

        for i in range(len(litter)):
            litter_index[litter[i]] = i

        q = deque([(start[0], start[1], e, 0, 0)]) # (row, col, curr_energy, litter_mask, steps)

        dp = {}
        dp[(start[0], start[1], 0)] = energy # (row, col, mask)

        while q:
            curr_row, curr_col, curr_e,mask, curr_s = q.popleft()

            if (curr_row, curr_col) in litter_index:
                i = litter_index[(curr_row, curr_col)]
                mask |= (1 << i)

                if mask == (1 << len(litter)) - 1:
                    return curr_s

            for direction in directions:
                new_row, new_col = curr_row + direction[0], curr_col + direction[1]

                if not (0 <= new_row < m and 0 <= new_col < n):
                    continue

                if (new_row, new_col) in obstacles:
                    continue

                if curr_e <= 0:
                    continue

                new_e = curr_e - 1

                if (new_row, new_col) in reset:
                    new_e = energy

                new_s = curr_s + 1
                key = (new_row, new_col, mask)

                prev_best = dp.get(key, -1)

                if new_e <= prev_best:
                    continue

                dp[key] = new_e
                q.append((new_row, new_col, new_e, mask, new_s))

        return -1
            ©leetcode
```
