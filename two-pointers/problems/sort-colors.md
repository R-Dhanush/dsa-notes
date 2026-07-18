---
pattern: Two pointers
variation: Partition
difficulty: Medium
solvedDate: 2026-07-18
link: https://leetcode.com/problems/sort-colors/
---
## Problem Summary:
- Given an array `nums` with `n` objects colored red, white, or blue. `0`, `1`, and `2` to represent the color red, white, and blue.
- Need to sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
- Array length range from 1 to 300.
## What made me recognize the pattern?
- Given input is an array.
- We can't not use `Arrays.sort()`.
- Need to swap elements into correct position in other words correct partitions, which points towards the [partition](../concepts/partition.md) variation.
## Brute Force:
- Using `Arrays.sort()` we can sort the array. 
## Observation:
- Array as only three values `0`, `1`, and `2`.
- We need to partition the array into three parts `low`, `mid`, `high`.
- We need three pointers:
	- `left` -> points the `low` partition.
	- `pointer` -> traverse the array.
	- `right` -> points the `high` partition.
## Intuition:
Initialize three pointers:
- `left = 0` -> points where the next `0` should be placed.
- `pointer = 0` -> traverse the array.
- `right = nums.length - 1` -> points where the next `2` should be placed.

Traverse the array using `pointer` until it cross `right`:
- Case 1 -> if `nums[pointer] == 0`
	- `swap(nums, left++, pointer++)`
	- place `0` at its correct position and move both the pointers.
	- Since everything before `pointer` is already processed, the element swapped into `pointer` is already known to be `1` (or `left == pointer`), so it's safe to move both pointers.
- Case 2 -> if `nums[pointer] == 1`
	- No need to do anything.
	- Move `pointer++`.
- Case 3 -> if `nums[pointer] == 2`
	- `swap(nums, pointer, right--)`.
	- Place `2` at its correct position and move `right--`.
	- Don't move `pointer`, because the element come from `right` has not been processed yet. It could be `0`, `1`, `2` so we need to examine before moving `pointer` forward.

At last every element will be place at correct partition.
## Complexity:
|                 | Time       | Space |
| --------------- | ---------- | ----- |
| **Brute Force** | O(n lon n) | O(1)  |
| **Optimized**   | O(n)       | O(1)  |
## What would break this approach?
- This code relies on the guarantee that only `0`, `1`, `2` appear in the array — the `else` branch doesn't actually check for `2`, it just catches anything that isn't `0` or `1`. If the array contained another value (e.g. `3` or `-1`), the code would silently mishandle it instead of throwing an error.
## Code I wrote:
```java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0;
        int pointer = 0;
        int right = nums.length - 1;
  
        while (pointer <= right) {
            if (nums[pointer] == 0) {
                swap(nums, left++, pointer++);
            } else if (nums[pointer] == 1) {
                pointer++;
            } else {
                swap(nums, pointer, right--);
            }
        }
    }
  
    public void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
```