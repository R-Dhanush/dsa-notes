---
pattern: Two pointers
variation: Same direction
difficulty: Easy
solvedDate: 2026-07-17
link: https://leetcode.com/problems/move-zeroes/
---
## Problem Summary:
- Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
- Must do it in-place without making a copy of the array.
- Array length range 1 to 10^4.
- Values range -2^31 to 2^31 - 1.
## What made me recognize the pattern?
- Given input is array.
- Need to swap values at two positions.
- Need to move two pointers at different conditions. which points towards [same-direction](app://obsidian.md/concepts/same-direction.md).
## Brute Force:
- Create a new array which as same length as `nums`.
- Store all the non-zero elements first in new array.
- Copy all the elements from new array to `nums`.
## Observation:
- We need two pointers:
    - One pointer (`right`) to traverse the array.
    - Another pointer (`left`) to mark the position where the next non-zero element should be placed.
- Everything before `left` is guaranteed to already be non-zero and in correct relative order.
## Intuition:
Initialize two pointers:
- left = 0;
- right = 0;

Traverse the array with `right`:
- If `nums[right] != 0` then swap `nums[right]` with  `nums[left]`, the increment `left`.
- Always increment `right`.

**Why does swapping work?**  
Since `left` only advances when a non-zero element is placed, every position before `left` is already a correctly-placed non-zero value. Swapping the current non-zero (`nums[right]`) into position `left` puts it in the right place, while whatever was at `left` (a zero, or a value already moved elsewhere) gets pushed toward `right` — which naturally shifts zeros toward the end without disturbing the relative order of non-zero elements, and without needing extra space.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- None.
## Code I wrote:
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int left = 0;
        int right = 0;
        while (right < nums.length) {
            if (nums[right] != 0)
                swap(nums, right, left++);
            right++;
        }
    }

    public void swap(int[] nums, int right, int left) {
        int temp = nums[right];
        nums[right] = nums[left];
        nums[left] = temp;
    }
}
```