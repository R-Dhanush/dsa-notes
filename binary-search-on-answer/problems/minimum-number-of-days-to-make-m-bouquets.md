---
pattern: Binary Search on Answer
variation: Minimize Maximum
difficulty: Medium
createdDate: 2026-08-06
link: https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/description/
---
## Problem Summary:
- Given an integer array `bloomDay`, an integer `m` and an integer `k`.
- Want to make `m` bouquets. To make a bouquet, need to use `k` **adjacent flowers** from the garden.
- The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in **exactly one** bouquet.
- Return _the minimum number of days you need to wait to be able to make_ `m` _bouquets from the garden_. If it is impossible to make m bouquets return `-1`.
## What made me recognize the pattern?
- Similar to **First Bad Version**, but instead of searching an array, we search over the range of possible days.
- Need to find the **minimum day** for which making `m` bouquets becomes possible.
- This points towards **Binary Search on Answer - Minimize Minimum variation**.
## Brute Force:
- Check every possible day from the earliest bloom day to the latest bloom day.
- For each day, determine whether it is possible to make `m` bouquets.
- Return the first day that works.
## Observation:
- No need to check every possible day.
- The earliest possible answer is the minimum value in `bloomDay`; the latest possible answer is the maximum value.
- If it is **not possible** to make `m` bouquets on day `mid`, then it is also impossible on every earlier day (fewer flowers have bloomed) — discard the lower half.
- If it **is possible** on day `mid`, then every later day is also possible (more flowers have bloomed) — but since we need the **minimum** day, keep `mid` as a candidate and continue searching to the left.
- If `m * k > bloomDay.length`, it is impossible to make enough bouquets regardless of the day.
## Intuition:
Before starting:
- If `(long) m * k > bloomDay.length`, return `-1`.

Initialize:
- `low` = minimum value in `bloomDay`.
- `high` = maximum value in `bloomDay`.

Traverse while `low < high`:
- `mid = low + (high - low) / 2`.
- If `isPossible(bloomDay, m, k, mid)` is true:
    - Day `mid` works and could still be the minimum.
    - Keep it in the search space: `high = mid`.
- Else:
    - Day `mid` is too early.
    - Discard it and all earlier days: `low = mid + 1`.

Once `low == high`, that day is the minimum feasible day — return `low`.

**isPossible(bloomDay, m, k, currentDay):**
- Traverse the array.
- Count consecutive flowers whose bloom day is less than or equal to `currentDay`.
- Whenever `k` consecutive flowers are collected:
    - Form one bouquet.
    - Reset the consecutive flower count.
- If an unbloomed flower is encountered:
    - Reset the consecutive flower count because bouquets require adjacent flowers.
- Return whether at least `m` bouquets can be formed.
## Complexity:
|                 | Time                                                              | Space |
| --------------- | ----------------------------------------------------------------- | ----- |
| **Brute Force** | O(n * d) n - length of the array, d - maximum value in `bloomDay` | O(1)  |
| **Optimized**   | O(n log d)                                                        | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {
        if ((long) m * k > bloomDay.length) {
            return -1;
        }
  
        int low = Integer.MAX_VALUE;
        int high = Integer.MIN_VALUE;
  
        for (int day : bloomDay) {
            low = Math.min(low, day);
            high = Math.max(high, day);
        }
  
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (isPossible(bloomDay, m, k, mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
  
        return low;
    }
  
    public boolean isPossible(int[] bloomDay, int m, int k, int currentDay) {
        int bouquetCount = 0;
        int flowerCount = 0;
        for (int day : bloomDay) {
            if (day <= currentDay) {
                flowerCount++;
                if (flowerCount == k) {
                    bouquetCount++;
                    flowerCount = 0;
                }
            } else {
                flowerCount = 0;
            }
        }
        return bouquetCount >= m;
    }
}
```