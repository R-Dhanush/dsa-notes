---
pattern: Binary search
variation: Boundary search
difficulty: Easy
createdDate: 2026-08-04
link: https://leetcode.com/problems/search-insert-position/description/
---
## Problem Summary:
- Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
## What made me recognize the pattern?
- Array is sorted.
- Need the position where a value exists, or where it _should_ go if missing — points towards Binary Search, Boundary Search variation.
## Brute Force:
- Traverse the array linearly, returning the index of the first element ≥ target. If no such element exists, return the array's length.
## Observation:
- This can be solved as a straightforward Exact Value Search (return `mid` on match), but when no match exists, the loop needs to end at exactly the correct insertion point.
- The condition "`nums[mid] >= target`" is monotonic across the array (false, false, ..., true, true), which is exactly what the Boundary Search shape relies on — though this particular implementation uses the three-way branch style instead, converging `high` to just before the insertion point.
## Intuition:
Initialize:
- `low = 0`, `high = nums.length - 1`.

Traverse while `low <= high`:
- `mid = low + (high - low) / 2` — avoids overflow.
- If `nums[mid] < target`: target belongs further right, `low = mid + 1`.
- Else if `nums[mid] == target`: found it, return `mid` directly.
- Else (`nums[mid] > target`): target belongs further left (or here), `high = mid - 1`.

If the loop ends without a match, `high` has converged to the position just **before** where the target should be inserted — so the correct insertion index is `high + 1`.

**Why `high + 1` is correct:** every time the search moves `high` left, it's because `nums[mid]` was too large to be the target — meaning the target could still belong at or before `mid`. By the time the loop ends, `high` sits at the last position confirmed to be too large to insert before, and `low` has moved past all positions confirmed too small — so `high + 1` (equivalently `low`, since they converge to be equal at loop end) is the correct gap where the target belongs.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
- If the array weren't sorted, this approach breaks entirely — same reasoning as plain Binary Search.
## Code I wrote:
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < target) {
                low = mid + 1;
            } else if (nums[mid] == target) {
                return mid;
            } else {
                high = mid - 1;
            }
        }
  
        return high + 1;
    }
}
```