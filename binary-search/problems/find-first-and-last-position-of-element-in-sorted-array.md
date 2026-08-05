---
pattern: Binary search
variation: Boundary search
difficulty: Medium
createdDate: 2026-08-05
link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/
---
## Problem Summary:
- Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.
- If `target` is not found in the array, return `[-1, -1]`.
## What made me recognize the pattern?
- Array is sorted.
- Need to find the starting and ending position of the given `target`, we need to find the boundary which points to Binary search - Boundary search variation.
## Brute Force:
- Traverse the array and compare every element with `target`.
- Note down the first index where the `target` appears and last index where the `target` appears last.
- Return the indices, if target not found return {-1, -1}.
## Observation:
- We can find the leftmost and rightmost index of the `target` by two passes with help of helper function.
- Helper function returns the index where the `target` is found or where it need to be.
- First passe find out the leftmost index, after the index returned from the helper function we need to make sure it points to the correct `target`.
- If it is not pointing to correct `target` value return `{-1, -1}`.
- Second pass find out the rightmost index.
## Intuition:
**findIndex(nums, target)** (the lowerBound helper):
- `left = 0`, `right = nums.length` (note: `nums.length` — this lets the function correctly return an out-of-bounds index when `target` is larger than every element).
- While `left < right`: 
	- `mid = left + (right-left)/2`. 
	- If `nums[mid] < target`, `left = mid + 1`. 
	- Else, `right = mid`.
- Return `left`.

**searchRange(nums, target):**
- Guard against an empty array upfront.
- `left = findIndex(nums, target)`.
- **Validate:** if `left == nums.length` (target bigger than everything) OR `nums[left] != target` (target simply isn't present), return `[-1, -1]`.
- Otherwise, `right = findIndex(nums, target + 1) - 1`.
- Return `[left, right]`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = findIndex(nums, target);
  
        if (left == nums.length || nums[left] != target) {
            return new int[]{-1, -1};
        }
        return new int[]{left, findIndex(nums, target + 1) - 1};
    }
    public int findIndex(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
  
        return left;
    }
}
```