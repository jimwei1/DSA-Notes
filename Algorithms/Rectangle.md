# Rectangle Problems

- Finding overlapping intervals/rectangles
- Computing area covered by multiple rectangles
- Finding intersections between geometric objects
- Calculating the union of rectangles

## Tools:

- Line Sweep
- Corner tracking

## Problems:

### 391. Perfect Rectangle

```
class Solution:
    def isRectangleCover(self, rectangles: List[List[int]]) -> bool:
        # We could also do a line sweep, making sure that for each x coordinate, the line going up has no overlaps nor gaps.

        # Corner approach
        # 1. The sums of the areas have to sum up to the total (which we can calculate with min/max width/height)
        # 2. Track the frequency of each edge. Edge corners should be at most 1, and every other corner should be even.

class Solution:
    def isRectangleCover(self, rectangles: List[List[int]]) -> bool:

        min_x = min_y = float('inf')
        max_x = max_y = 0

        corners = defaultdict(int)
        area = 0

        for x1, y1, x2, y2 in rectangles:
            min_x = min(min_x, x1)
            min_y = min(min_y, y1)
            max_x = max(max_x, x2)
            max_y = max(max_y, y2)

            area += (y2 - y1) * (x2 - x1)

            corners[(x1, y1)] += 1
            corners[(x2, y1)] += 1
            corners[(x1, y2)] += 1
            corners[(x2, y2)] += 1

        expected_area = (max_y - min_y) * (max_x - min_x)

        if expected_area != area:
            return False

        expected_corners = {
            (min_x, min_y): 1,
            (min_x, max_y): 1,
            (max_x, min_y): 1,
            (max_x, max_y): 1
        }

        edges = [(min_x, min_y), (min_x, max_y), (max_x, min_y), (max_x, max_y)]

        for corner, count in corners.items():
            if corner in edges:
                if count != 1:
                    return False
            else:
                if corners[corner] % 2 != 0:
                    return False

        return True
```
