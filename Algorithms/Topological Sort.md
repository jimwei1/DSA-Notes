# Topological Sort

- To build the topological order of a Directed Acyclic Graph.
- Essentially, the order of previous nodes will always point to nodes further down the graph (for every directed edge `u -> v`, node u comes before v).

## Notes (Kahn's Algorithm, BFS)

- Can also detect cycles: If `len(topological order) != n`, then there's a cycle.

**Code:**

```
def topologicalSort(numCourses, prerequisites):
    # Step 1: Build the adjacency list and in-degree array
    graph = defaultdict(list)
    in_degree = [0] * numCourses

    for dest, src in prerequisites:
        graph[src].append(dest)
        in_degree[dest] += 1

    # Step 2: Initialize the queue with all nodes having in-degree 0
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    topo_order = []

    # Step 3: Process the graph
    while queue:
        node = queue.popleft()
        topo_order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    # If we processed all nodes, return the topological order
    if len(topo_order) == numCourses:
        return topo_order
    else:
        # Cycle detected
        return []
```

## Problems

### Neetcode 150

#### 269. Alien Dictionary

**Code:**

```
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        '''
        Topological sort
            - Create directional paths for each letter order we know
            - Run topological sort to get the topological order

        1. For each adjacent word, find the first difference and create a directed edge
        3. Run topological sort on this adj list
        '''

        adj_list = defaultdict(list)
        in_degree = {}

        for word in words:
            for letter in word:
                in_degree[letter] = 0

        for i in range(len(words) - 1):
            curr_word = words[i]
            next_word = words[i + 1]

            first_diff = None

            for j in range(min(len(curr_word), len(next_word))):
                if curr_word[j] != next_word[j]:
                    first_diff = j
                    break

            # Edge Case: prefix comes after word
            if first_diff == None and len(curr_word) > len(next_word):
                return ""

            if first_diff != None:
                adj_list[curr_word[j]].append(next_word[j])


        # Run topological sort on adj_list

        q = deque()

        for src in adj_list.keys():
            for dest in adj_list[src]:
                in_degree[dest] += 1

        for letter in in_degree.keys():
            if in_degree[letter] == 0:
                q.append(letter)

        topo_order = []

        while q:
            curr = q.popleft()
            topo_order.append(curr)

            for neighbor in adj_list[curr]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    q.append(neighbor)

        if len(topo_order) < len(in_degree.keys()):
            return ""

        return "".join(topo_order)
```
