---
pattern: Binary search
variation: Rotated Sorted Array
difficulty: Medium
createdDate: 2026-08-04
link: https://leetcode.com/problems/search-in-rotated-sorted-array/description/
---
## Problem Summary:
- Given a sorted array `nums` (possibly rotated at an unknown pivot) and a target value, return the index of the target if found, or -1 if not present.
- All values are distinct.
## What made me recognize the pattern?
- Array is sorted, but rotated — a single comparison of `nums[mid]` to `target` isn't enough to decide which half to discard, since only one half is properly sorted at any given step.
- Points towards Binary Search, Rotated Sorted Array variation.
## Brute Force:
- Traverse the array linearly, comparing each element to `target`, return the index on a match.
## Observation:
- In a rotated sorted array, at every `mid`, **at least one half is guaranteed to be properly sorted** (either the left half `[low..mid]` or the right half `[mid..high]`).
- Compare `nums[low]` to `nums[mid]` to determine which half is sorted: if `nums[low] <= nums[mid]`, the left half is sorted; otherwise the right half is sorted.
- Once the sorted half is identified, check whether `target` falls within that half's value range. If it does, search that half; otherwise the answer must be in the other half.
## Intuition:
Initialize:
- `low = 0`, `high = nums.length - 1`.

Traverse while `low <= high`:
- `mid = low + (high - low) / 2`.
- If `nums[mid] == target`: found it, return `mid`.
- If `nums[low] <= nums[mid]` (left half is sorted):
    - If `target` falls within `[nums[low], nums[mid])`: search left, `high = mid - 1`.
    - Else: search right, `low = mid + 1`.
- Else (right half is sorted):
    - If `target` falls within `(nums[mid], nums[high]]`: search right, `low = mid + 1`.
    - Else: search left, `high = mid - 1`.

If the loop ends without a match, return -1.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[low] <= nums[mid]) {
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
}
```