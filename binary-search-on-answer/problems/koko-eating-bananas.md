---
pattern: Binary Search on Answer
variation: Minimize Maximum
difficulty: Medium
createdDate: 2026-08-06
link: https://leetcode.com/problems/koko-eating-bananas/
---
## Problem Summary:
- There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas.
- The guards have gone and will come back in `h` hours.
- Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile.
- If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.
- Need to finish eating all the bananas before the guards return.
- Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.
## What made me recognize the pattern?
- It is like First bad version problem. but, we need to search over the space of possible answer, not the array itself.
- Which points to Binary Search on Answer - Minimize Maximum variation.
## Brute Force:
- Gradually increase the eating speed from 1, checking at each speed whether it's possible to eat all the bananas within `h` hours. Return the first speed that works.
## Observation:
- No need to check every possible speed.
- Lowest possible speed is 1; highest useful speed is the maximum pile size — a faster speed than that is wasted, since one hour is enough to clear even the biggest pile at that rate.
- If the speed at `mid` is **not** possible to finish within `h` hours, every speed slower than `mid` is also not possible (slower speed only takes longer) — discard the lower half.
- If the speed at `mid` **is** possible to finish within `h` hours, every speed faster than `mid` is also possible — but since we want the **minimum** speed, we don't take the faster half either; we keep `mid` as a candidate and keep searching for something even slower that still works.
## Intuition:
Initialize:
- `low = 1` (slowest possible speed).
- `high` = the maximum value in `piles` (fastest speed ever needed).

Traverse while `low < high`:
- `mid = low + (high - low) / 2`.
- If `isPossible(piles, h, mid)` is true: `mid` works and could still be the minimum — keep it in range, `high = mid`.
- Else: `mid` is too slow — discard it and everything below it, `low = mid + 1`.

Once `low == high`, that value is the minimum feasible speed — return `low`.

**isPossible(piles, h, k):** for each pile, compute the hours needed to finish it at speed `k` using ceiling division (`(pile + k - 1) / k)`, since a partial pile still costs a full hour), sum across all piles, and check if the total is within `h`.
## Complexity:
|                 | Time       | Space |
| --------------- | ---------- | ----- |
| **Brute Force** | O(n * m)   | O(1)  |
| **Optimized**   | O(n log m) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int low = 1;
        int high = 0;
        for (int pile : piles) {
            if (pile > high) {
                high = pile;
            }
        }
  
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (isPossible(piles, h, mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
  
        return low;
    }
    public boolean isPossible(int[] piles, int h, int k) {
        int totalHours = 0;
        for (int pile : piles) {
            totalHours += (pile + k - 1) / k;
        }
        return totalHours <= h;
    }
}
```