# Trie

## Notes:

- A Trie is a tree-like DS that stores strings / sequences wher each node represents a character.
  - Useful for:
    - Prefix-based searches
    - Autocompletion
    - Spell checkers
    - Dictionary implementations
- Implemented with a dictionary of dictionaries, and a `self.is_end` special character / attribute that marks the end of a word.

  ```
    class TrieNode:
        def __init__(self):
            # Each node has a dictionary of children and a flag indicating the end of a word.
            self.children = {}
            self.is_end = False

            or

            self.root = {}
            self.end_char = "*"
  ```

### Use Cases

- Autocomplete
- Spellchecker

## Problems:

### Neetcode 150:

#### 212. Word Search II (Trie + Graph DFS)

**Code:**

```
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        '''
        Since we're constructing words one letter at a time -> Trie to store all words
        Then, DFS / Backtrack and store a visited set (2D -> 1D mapping). Pop from visited if we go back.

        Time Complexity: w = len(word), m * n = board dimensions, L is max length of a word, x is all letters in the word
            - Insert is O(w) per word
            - Calling DFS m*n times, worst case is 4^L -> O(m*n*4^L)

        Space Complexity:
            - Board in-place modification, no extra space complexity
            - Trie is O(x)
            - Max recursion depth is L
            - Therefore, O(x + L)
        '''

        root = {}
        end_char = "*"
        rows = len(board)
        cols = len(board[0])
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        ans = set()

        def insert(word: str) -> None:
            node = root
            for letter in word:
                if letter not in node:
                    node[letter] = {}
                node = node[letter]

            node[end_char] = word

        def dfs(curr_row: int, curr_col: int, node: dict) -> None:
            curr_letter = board[curr_row][curr_col]

            if curr_letter not in node:
                return

            # Move to next node on the new dfs call
            node = node[curr_letter]

            if end_char in node:
                ans.add(node[end_char])

            board[curr_row][curr_col] = "#"

            for direction in directions:
                new_row, new_col = curr_row + direction[0], curr_col + direction[1]

                if 0 <= new_row < rows and 0 <= new_col < cols and board[new_row][new_col] in node:
                    dfs(new_row, new_col, node)


            board[curr_row][curr_col] = curr_letter

        # Populate Trie
        for word in words:
            insert(word)

        for row in range(rows):
            for col in range(cols):
                if board[row][col] in root:
                    dfs(row, col, root)

        return list(ans)
```

### Other:

#### 211. Design Add and Search Words Data Structure

**Code:**

```
class WordDictionary:
    '''
    Some kind of tree -> start at a letter and link it to future letters... if we search for a ., we can search all the letters (like put them in a queue)

    The search would be like, a DFS and the add would just be an insert into the tree

    Trie -> Dictionary where each node has character keys for its child nodes, and end with "*"
    '''

    def __init__(self):

        self.root = {}
        self.end = "*"


    def addWord(self, word: str) -> None:
        node = self.root

        for letter in word:
            if letter not in node:
                node[letter] = {}
            node = node[letter]

        # Add a * to mark end of word
        node[self.end] = {}


    def search(self, word: str) -> bool:
        node = self.root

        # Recursive DFS to return whether we find
        def dfs(node: dict, index: int) -> bool:
            if index == len(word):
                return self.end in node

            elif word[index] == '.':
                for letter in node.keys():
                    if dfs(node[letter], index + 1):
                        return True
                return False

            elif word[index] in node:
                return dfs(node[word[index]], index + 1)

            else:
                return False

        return dfs(self.root, 0)

# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```
