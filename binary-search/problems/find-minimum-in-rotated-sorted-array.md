---
pattern: Binary search
variation: Rotated Sorted Array
difficulty: Medium
createdDate: 2026-08-04
link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/
---
## Problem Summary:
- Given a sorted array `nums` that has been rotated at an unknown pivot, find the minimum element.
- All values are distinct.
## What made me recognize the pattern?
- Need to converge to a single boundary (the minimum element), rather than searching for a given target.
- This points towards the Binary Search → Boundary Search variation, combined with Rotated Array reasoning.
## Brute Force:
- Traverse the array linearly, tracking the smallest value seen.
## Observation:
- Compare `nums[mid]` to `nums[high]` (not `nums[low]`, unlike Search in Rotated Sorted Array) — this tells us whether the minimum lies in the left portion (including `mid`) or strictly in the right portion.
- If `nums[mid] <= nums[high]`, the right portion (from `mid` to `high`) is already sorted with no rotation break in it — meaning the minimum is either `mid` itself, or somewhere to its left. `mid` must stay in the search range, not be excluded.
- If `nums[mid] > nums[high]`, the rotation break is somewhere between `mid` and `high` — meaning the minimum is strictly to the right of `mid`, so `mid` itself can be safely excluded.
- This uses the Boundary Search loop shape (`low < high`, no early return) — narrowing continues until `low == high`, and that final position holds the minimum.
## Intuition:
Initialize:
- `low = 0`, `high = nums.length - 1`.

Traverse while `low < high`:
- `mid = low + (high - low) / 2`.
- If `nums[mid] <= nums[high]`: the minimum is at `mid` or to its left — keep `mid` in range, `high = mid`.
- Else: the minimum is strictly to the right of `mid` — exclude `mid`, `low = mid + 1`.

Once `low == high`, that position holds the minimum — return `nums[low]`.

**Why `high = mid` (not `mid - 1`) when the condition is true:** unlike Search in Rotated Sorted Array (where `mid` is checked directly against `target` and can be excluded once ruled out), here `mid` might genuinely _be_ the answer itself — excluding it prematurely (via `mid - 1`) can skip past the actual minimum, as verified by tracing `[3,1,2]`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int findMin(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] <= nums[high]) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return nums[low];
    }
}
```