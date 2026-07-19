---
pattern: Two pointers
variation: Two arrays
difficulty: Easy
solvedDate: 2026-07-19
link: https://leetcode.com/problems/intersection-of-two-arrays/description/
---
## Problem Summary:
- Given two integer arrays `nums1` and `nums2`, return an array of their intersection.
- Arrays length range from 1 to 1000.
- Values range from 0 to 1000.
## What made me recognize the pattern?
- Given input is arrays.
- Need to compare values across arrays, which points towards [two-arrays](../concepts/two-arrays.md) variation.
## Brute Force:
- For each element in nums1, use a nested loop to check if it exists anywhere in nums2.
- If found, add it to a Set (to naturally avoid duplicate entries in the result) rather than a plain list.
- Convert the Set to an array at the end.
## Observation:
- We need to sort both the arrays, so that we can skip the duplicates.
- We need to use two pointers.
	- `pointer1` - to traverse the `nums1`.
	- `pointer2` - to traverse the `nums2`.
- We need to return the values which are present in both the arrays.
## Intuition:
We need to sort the both arrays:
- `Arrays.sort(nums1)`
- `Arrays.sort(nums2)`

Initialize two pointers:
- `pointer1 = 0` - to traverse the `nums1`.
- `pointer2 = 0` - to traverse the `nums2`.

Traverse the arrays until `pointer1 < nums1.length && pointer2 < nums2.length`:
- If `nums1[pointer1] < nums2[pointer2]` move `pointer1++` forward and skip all the duplicates.
- If `nums1[pointer1] == nums2[pointer2]` store that element in a list and move both pointers forward and skip all the duplicates.
- If `nums1[pointer1] > nums2[pointer2]` move `pointer2++` forward and skip all the duplicates.

At last all the interesting elements will be in the list.
Copy the elements in the list to a new array.
## Complexity:
|                 | Time                 | Space                |
| --------------- | -------------------- | -------------------- |
| **Brute Force** | O(n * m)             | O(k)                 |
| **Optimized**   | O(n log n + m log m) | O(log n + log m + k) |
## What would break this approach?
- None, but there is better approach we will see in upcoming topics.
## Code I wrote:
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
  
        int pointer1 = 0;
        int pointer2 = 0;
  
        List<Integer> list = new ArrayList<>();
  
        while (pointer1 < nums1.length && pointer2 < nums2.length) {
            if (nums1[pointer1] < nums2[pointer2]) {
                pointer1++;
  
                while (pointer1 < nums1.length && nums1[pointer1] == nums1[pointer1 - 1]) {
                    pointer1++;
                }
            } else if (nums1[pointer1] == nums2[pointer2]) {
                list.add(nums1[pointer1]);
  
                pointer1++;
                pointer2++;
  
                while (pointer1 < nums1.length && nums1[pointer1] == nums1[pointer1 - 1]) {
                    pointer1++;
                }
                while (pointer2 < nums2.length && nums2[pointer2] == nums2[pointer2 - 1]) {
                    pointer2++;
                }
            } else {
                pointer2++;
  
                while (pointer2 < nums2.length && nums2[pointer2] == nums2[pointer2 - 1]) {
                    pointer2++;
                }
            }
        }
  
        int[] arr = new int[list.size()];
        int index = 0;
        for (int num : list) {
            arr[index++] = num;
        }
  
        return arr;
    }
}
```