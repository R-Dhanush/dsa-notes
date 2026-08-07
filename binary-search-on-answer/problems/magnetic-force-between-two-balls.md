---
pattern: Binary Search on Answer
variation: Maximize Minimum
difficulty: Medium
createdDate: 2026-08-07
link: https://leetcode.com/problems/magnetic-force-between-two-balls/
---
## Problem Summary:
- Given an integer array `position`, where `position[i]` represents the location of the `i`th basket.
- Given an integer `m`, representing the number of balls to place.
- Place the `m` balls into the baskets such that the **minimum distance between any two balls is as large as possible**.
- Return the maximum possible minimum distance.
## What made me recognize the pattern?
- Need to maximize the minimum distance between placed balls.
- The answer is not an element of the array, but a possible **distance**.
- If a minimum distance `d` is possible, then every smaller distance is also possible.
- This monotonic property points towards **Binary Search on Answer - Maximize Minimum variation**.
## Brute Force:
- Try every possible minimum distance from `1` to the maximum possible distance.
- For each distance, greedily check whether all `m` balls can be placed.
- Return the largest distance that works.
## Observation:
- The basket positions must be sorted so that we can greedily place balls from left to right.
- The smallest possible answer is `1`.
- The largest possible answer is `position[max] - position[min]`.
- If we can place all `m` balls with a minimum distance of `mid`, then every smaller distance is also possible.
- If we cannot place all `m` balls with a minimum distance of `mid`, then every larger distance is also impossible.
- Since we want the **largest** feasible distance, when `mid` works we continue searching to the right.
## Intuition:
Sort the `position` array.

Initialize:
- `low = 1` - smallest possible minimum distance.
- `high = position[last] - position[0]` - largest possible minimum distance.

Traverse while `low < high`:
- Calculate the upper middle value:
    - `mid = low + (high - low + 1) / 2`.
- If `isPossible(position, m, mid)` is true:
    - Distance `mid` works.
    - Try a larger minimum distance: `low = mid`.
- Else:
    - Distance `mid` is too large.
    - Search smaller distances: `high = mid - 1`.

Once `low == high`, that value is the largest feasible minimum distance.

**isPossible(position, m, gap):**
- Place the first ball in the first basket.
- Traverse the remaining baskets.
- Whenever the distance from the last placed ball is at least `gap`, place another ball.
- Count how many balls are placed.
- Return whether at least `m` balls can be placed.
## Complexity:
|                 | Time                                                                        | Space    |
| --------------- | --------------------------------------------------------------------------- | -------- |
| **Brute Force** | O(n log n + n × d) `n` = number of baskets, `d` = maximum possible distance | O(log n) |
| **Optimized**   | O(n log n + n log d)                                                        | O(log n) |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int maxDistance(int[] position, int m) {
        Arrays.sort(position);
  
        int firstPosition = position[0];
        int lastPosition = position[position.length - 1];
  
        int low = 1;
        int high = lastPosition - firstPosition;
  
        while (low < high) {
            int mid = low + (high - low + 1) / 2;
            if (isPossible(position, m, mid)) {
                low = mid;
            } else {
                high = mid - 1;
            }
        }
  
        return low;
    }
    public boolean isPossible(int[] position, int m, int gap) {
        int last = position[0];
        int count = 1;
        for (int i = 1; i < position.length; i++) {
            if (position[i] - last >= gap) {
                count++;
                last = position[i];
            }
        }
        return count >= m;
    }
}
```