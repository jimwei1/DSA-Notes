# Line Sweep

## Problems:

### 253. Meeting Rooms II

**Code:**

```
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        #Go through each interval.
        #Add (start, 0), (end, 1)
        #Sort
        #If start -> += 1
        #if End -> -= 1
        array = []
        for interval in intervals:
            array.append((interval[0], 1))
            array.append((interval[1], 0))

        array.sort()

        curr = 0
        ans = 0

        for element in array:
            if element[1] == 1:
                curr += 1
                ans = max(ans, curr)
            if element[1] == 0:
                curr -= 1

        return ans
```

### 759. Employee Free Time

**Code:**

```
"""
# Definition for an Interval.
class Interval:
    def __init__(self, start: int = None, end: int = None):
        self.start = start
        self.end = end
"""

class Solution:
    def employeeFreeTime(self, schedule: '[[Interval]]') -> '[Interval]':
        '''
        Given our set of intervals, find all places where there are no intervals
        If busy = 0 when we check a new event, then clearly prev_event to new event must all be no interval.
            - Note busy will only be 0 when we end an interval. So it will be between the end of an interval and the start of a new interval.
        Any time between the end of one busy period and the start of the next is considered free time.

        Sweep line, + 1 at the start and -1 at the end of a schedule so we know how many busy workers there are.

        Keep track of busy workers. When busy = 0, all workers are free

        Each employee has non overlapping, so we dont need to track duplicates for an employee
        '''
        array = []

        for employee_interval in schedule:
            for curr_interval in employee_interval:
                start = curr_interval.start
                end = curr_interval.end

                array.append((start, 1))
                array.append((end, - 1))

        array.sort()
        busy = 0
        prev_time = None
        ans = []

        for time, delta in array:
            if busy == 0 and prev_time is not None and prev_time < time:
                new_interval = Interval(prev_time, time)
                ans.append(new_interval)

            if delta == 1:
                busy += 1
            else:
                busy -= 1

            prev_time = time


        return ans
```
