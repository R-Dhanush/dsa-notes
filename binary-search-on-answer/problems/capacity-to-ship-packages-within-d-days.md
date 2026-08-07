---
pattern: Binary Search on Answer
variation: Minimize Maximum
difficulty: Medium
createdDate: 2026-08-07
link: https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/
---
## Problem Summary:
- Given an array `weights` representing package weights, and an integer `days`, packages must be shipped in the given order, some number per day, without exceeding a chosen ship capacity per day.
- Return the least weight capacity that ships all packages within `days` days.
## What made me recognize the pattern?
- Same shape as Koko Eating Bananas and Minimum Days to Make m Bouquets — searching over a range of possible capacities, not the array itself, checking feasibility at each candidate. Points to Binary Search on Answer — Minimize Maximum variation.
## Brute Force:
- Try every possible capacity starting from the minimum required, checking at each whether all packages can ship within `days` days. Return the first capacity that works.
## Observation:
- Packages must ship in order — a "day" is just a running sum of consecutive weights, and a new day starts the moment adding the next package would exceed the chosen capacity.
- Lowest possible capacity isn't 1 — it must be at least the **heaviest single package**, since no capacity smaller than that could ever ship that package at all (it wouldn't fit even alone on a day). Starting `low` there (instead of 1) eliminates a whole range of guaranteed-infeasible candidates.
- Highest possible capacity is the **sum of all weights** (shipping everything in a single day).
- If `isPossible(mid)` is true, `mid` could still be the minimum — keep it in range (`high = mid`), same reasoning as Koko and Bouquets.
## Intuition:
Initialize:
- `low` = max single package weight (via a pass through `weights`).
- `high` = sum of all weights (accumulated in the same pass).

Traverse while `low < high`:
- `mid = low + (high - low) / 2`.
- If `isPossible(weights, days, mid)`: keep `mid` in range, `high = mid`.
- Else: `low = mid + 1`.

Return `low`.

**isPossible(weights, days, shipCapacity):** simulate loading day by day. Track `todayLoad`; for each weight, if adding it would exceed `shipCapacity`, that day is full — start a new day (`daysRequired++`, reset `todayLoad` to just this package), otherwise add it to the current day's load. After the loop, add 1 for the final in-progress day (never explicitly "closed" by an overflow), and check if the total days used is within the limit.
## Complexity:
|                 | Time                | Space |
| --------------- | ------------------- | ----- |
| **Brute Force** | O(n × (sum − max))  | O(1)  |
| **Optimized**   | O(n log(sum − max)) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int low = 0;
        int high = 0;
        for (int weight : weights) {
            low = Math.max(low, weight);
            high += weight;
        }
  
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (isPossible(weights, days, mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
    public boolean isPossible(int[] weights, int days, int shipCapacity) {
        int todayLoad = 0;
        int daysRequired = 0;
        for (int weight : weights) {
            if (todayLoad + weight > shipCapacity) {
                todayLoad = weight;
                daysRequired++;
            } else {
                todayLoad += weight;
            }
        }
        return daysRequired + 1 <= days;
    }
}
```