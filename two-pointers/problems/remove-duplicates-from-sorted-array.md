---
pattern: Two pointers
variation: Same direction
difficulty: Easy
solvedDate: 2026-07-17
link: https://leetcode.com/problems/remove-duplicates-from-sorted-array/
---
## Problem Summary:
- Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates in-place.
- The relative order of the elements should be maintained.
- Return the number of unique elements.
- Array length range 1 to 3 * 10 ^ 4.
- Values range -100 to 100.
## What made me recognize the pattern?
- Given input is array.
- Need to swap values at two positions.
- Need to move two pointers at different conditions. which points towards [same-direction](../concepts/same-direction.md).
## Brute Force:
- Traverse the array from left to right.
- Store only the unique elements in a temporary list (or array).
- Copy all the unique elements back into `nums`.
- Return the number of unique elements.
## Observation:
- The array is already sorted in ascending order. Same values will be adjacent to each other.
- Here we can use two pointers.
	- One for pointing where the next unique element should be placed.
	- Another one is to traverse the array.
## Intuition:
Initialize two pointers:
- left = 1; - the first element will be always unique, so we are starting from second element.
- right = 1;

Traverse the array up to last element.
- If `nums[right] != nums[right - 1]` it means we found out a new unique value, store it at `nums[left]` which points where the next unique element should be placed.

At last return the left pointer value. It represent number of unique elements in the array.
## Complexity:
|                 | Time | Space                                                                       |
| --------------- | ---- | --------------------------------------------------------------------------- |
| **Brute Force** | O(n) | O(k) - the temp list holds only the unique elements, which is `k` elements. |
| **Optimized**   | O(n) | O(1)                                                                        |
## What would break this approach?
- If the array weren't sorted, duplicates wouldn't be adjacent, so comparing `nums[right]` to `nums[right-1]` would fail to detect all duplicates — this approach fundamentally depends on sorted order placing equal values next to each other.
## Code I wrote:
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int left = 1;
        for (int right = 1; right < nums.length; right++) {
            if (nums[right] != nums[right - 1])
                nums[left++] = nums[right];
        }
        return left;
    }
}
```