# Linked List

## Notes:

- Create with dummy node:
  - newHead = ListNode(0)
  - If you want to make a new list object (don’t have to:
    - self.newList = newHead
  - tail is a pointer to last node of list
- Expand infinitely to the right:
  ```jsx
  for letterNum in new:
              curr.next = ListNode(int(letterNum))
              curr = curr.next
  ```
- Get middle of linked list or check for cycle:
  - Slow and fast pointer (moves 2x speed)
  - Slow is now the middle
  - Cycle works because:
    - If its a cycle, the fast pointer will either be 2 or 1 behind the slow pointer.
      - If 1 behind, they will catch up.
      - If 2 behind, it will become 1 behind.
- Get middle:

```
      slow = head
      fast = head

      while fast and fast.next:
          slow = slow.next
          fast = fast.next.next
```

- Reverse in place

  ```
  while curr is not None:
          forward = curr.next
          curr.next = prev

          prev = curr
          curr = forward
  ```

  - In this code, prev is now the front.
  - Use two pointers: prev and curr
    1. Save curr.next
    2. Point [curr.next](http://curr.next) to Prev
    3. Set prev to curr
    4. Set curr to the saved curr.next

### Floyd’s Algorithm (Finding Start of a Linked List Cycle)

- To find the start of a cycle in a linked list
  1. Do fast and slow pointer until you find the cycle (they intersect)
  2. Now, start two slow pointers, one from the intersection and one from the start of the linked list
  3. Once these two slow pointers intersect, you arrive at the start of the cycle
