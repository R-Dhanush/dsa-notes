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
- We only need two pointers:
	- `left` -> traverses from the front, looking for an odd number.
	- `right` -> marking where the next odd number should go.
- If `nums[left]` is already even, it's correctly placed — just move `left` forward.
- If `nums[left]` is odd, swap it with whatever is at `right` and shrink `right`. We don't need to check what's at `right` first — if the swapped-in value is still odd, the next loop iteration will catch it and swap again.
## Intuition:
Initialize two pointers:
- `left = 0` 
- `right = nums.length - 1` 

Traverse while `left < right`:
- If `nums[left]` is even, it's already correctly placed — move `left++`.
- Else (`nums[left]` is odd), swap `nums[left]` with `nums[right]`, then move `right--`. Don't advance `left` yet — the newly swapped-in value at `left` hasn't been checked, so it must be examined on the next iteration.

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
        
        while (left < right) {
            if (nums[left] % 2 == 0) {
                left++;
            } else {
                swap(nums, left, right--);
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