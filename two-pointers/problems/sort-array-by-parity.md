---
pattern: Two pointers
variation: Partition
difficulty: Easy
solvedDate: 2026-07-18
link: https://leetcode.com/problems/sort-array-by-parity/description/
---
## Problem Summary:
- Given an integer array `nums`, move all the even integers to the beginning of the array, followed by all the odd integers.
- Return any array that satisfies this condition — relative order **does not** need to be preserved.
- Array length range 1 to 5000.
## What made me recognize the pattern?
- Given input is an array.
- Need to rearrange elements in-place based on a condition (even/odd).
- No requirement to preserve relative order, and only two groups — which points towards the [partition](../concepts/partition.md) variation.
## Brute Force:
- Traverse the array, collect even numbers in one list and odd numbers in another.
- Concatenate the even list followed by the odd list, copy back into `nums`.
## Observation:
- Only two categories exist: even and odd.
- We need three pointers:
	- `left` -> points to where the next even number should be placed.
	- `pointer` -> traverses the array.
	- `right` -> points to where the next odd number should be placed.
## Intuition:
Initialize three pointers:
- `left = 0` -> points where the next even number should be placed.
- `pointer = 0` -> traverses the array.
- `right = nums.length - 1` -> points where the next odd number should be placed.

Traverse the array using `pointer` until it crosses `right`:
- The loop continues while `pointer <= right` because everything after `right` is already correctly placed (processed), and the element exactly at `right` hasn't been checked yet — so it must be included, not skipped, which is why `<=` is used instead of `<`.
- Case 1 -> if `nums[pointer]` is even
    - `swap(nums, left++, pointer++)`
    - Place the even number at its correct position and move both pointers.
    - Everything before `pointer` is already processed, so the element swapped into `pointer` is safe to move past.
- Case 2 -> if `nums[pointer]` is odd
    - `swap(nums, pointer, right--)`
    - Place the odd number at its correct position and move `right--`.
    - Don't move `pointer`, because the element that comes from `right` hasn't been processed yet — it could be even or odd, so it must be examined before `pointer` moves forward.

At last, all evens will be at the front and all odds at the back.
## Complexity:
|                 | Time | Space |
| --------------- | ---- | ----- |
| **Brute Force** | O(n) | O(n)  |
| **Optimized**   | O(n) | O(1)  |
## What would break this approach?
- This approach does NOT preserve relative order — if a problem required preserving the original order of evens/odds, this swap-based technique would break that requirement and a different approach (extra arrays) would be needed instead.
## Code I wrote:
```java
class Solution {
    public int[] sortArrayByParity(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int pointer = 0;
        
        while (pointer <= right) {
            if (nums[pointer] % 2 == 0) {
                swap(nums, left++, pointer++);
            } else {
                swap(nums, pointer, right--);
            }
        }
        return nums;
    }
    
    public void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
```