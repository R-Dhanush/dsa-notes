---
pattern: Two pointers
variation: Two arrays
difficulty: Easy
solvedDate: 2026-07-19
link: https://leetcode.com/problems/merge-sorted-array/description/
---
## Problem Summary:
- Given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.
- Need to merge `nums1` and `nums2` into a single array `nums1` sorted in **non-decreasing order**.
- `nums1` length `m + n`.
- `nums2` length `n`.
- Arrays length range from 0 to 200.
- Values range from -10 ^ 9 to 10 ^ 9.
## What made me recognize the pattern?
- Given input is arrays.
- Both arrays are sorted.
- Need to compare values across arrays, which points towards [two-arrays](../concepts/two-arrays.md) variation. 
## Brute Force:
- Copy all n elements from `nums2` into the empty slots at the end of `nums1`. 
- Sort the entire `nums1` array using `Arrays.sort()`.

## Observation:
- Arrays already sorted in ascending order. so,
	- Larger values will be at end.
	- Smaller values will be at beginning.
- We need to merge both arrays into single array in ascending order.
- So we need to use three pointers.
	- `pointer1` — points to `nums1`'s last actual element (`m - 1`).
	- `pointer2` — points to `nums2`'s last element (`n - 1`).
	- `pointer3` — points to `nums1`'s last index overall, used to fill the array.
- `nums1`'s real elements sit at the beginning of the array. If we filled from the beginning, we'd overwrite values in `nums1` before reading them. So it's best to fill from the back instead.
- If `nums2` still has elements left after `nums1` runs out, they must be copied over — they're guaranteed to be in correct final position already since they're the smallest remaining values.
- If `nums1` still has elements left after `nums2` runs out, nothing needs to be done — those elements are already in their correct final position, since they started at the front and never needed to move.
## Intuition:
Initialize three pointers:
- `pointer1 = m - 1` — points to `nums1`'s last element.
- `pointer2 = n - 1` — points to `nums2`'s last element.
- `pointer3 = nums1.length - 1` — points to where the next (largest remaining) value should be placed.

Traverse both arrays using until `pointer1 >= 0` and `pointer2 >= 0`:
- If `nums1[pointer1] > nums2[pointer2]` then fill `nums1[pointer3] = nums1[pointer1]` and decrement both the pointers.
- If `nums1[pointer1] < nums2[pointer2]` then fill `nums1[pointer3] = nums2[pointer2]` and decrement both the pointers.

Suppose if still there are elements `nums2` then fill all them into `nums1`.
At last all the elements will be added to `nums1` in ascending order.
## Complexity:
|                 | Time                 | Space         |
| --------------- | -------------------- | ------------- |
| **Brute Force** | O((m +n) log(m + n)) | O(log(m + n)) |
| **Optimized**   | O(m + n)             | O(1)          |
## What would break this approach?
- If `nums2` is empty (`n = 0`) — the first while loop never runs since `pointer2 = -1` fails `pointer2 >= 0` immediately, and the second while loop also never runs for the same reason. `nums1` is returned unchanged, which is correct since there's nothing to merge in.
- If `nums1`'s initial `m` elements aren't actually sorted, or `nums2` isn't sorted — this approach breaks entirely, since the comparison logic assumes both are already in sorted order.
## Code I wrote:
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int pointer1 = m - 1;
        int pointer2 = n - 1;
        int pointer3 = nums1.length - 1;
  
        while (pointer1 >= 0 && pointer2 >= 0) {
            if (nums1[pointer1] > nums2[pointer2]) {
                nums1[pointer3--] = nums1[pointer1--];
            } else {
                nums1[pointer3--] = nums2[pointer2--];
            }
        }
  
        while (pointer2 >= 0) {
            nums1[pointer3--] = nums2[pointer2--];
        }
    }
}
```