---
pattern: Two pointers
variation: Two arrays
difficulty: Easy
solvedDate: 2026-07-19
link: https://leetcode.com/problems/intersection-of-two-arrays-ii/description/
---
## Problem Summary:
- Given two integer arrays `nums1` and `nums2`, return an array of their intersection.
- Each element in the result must appear as many times as it shows in both arrays.
- Arrays length range from 1 to 1000.
- Values range from 0 to 1000.
## What made me recognize the pattern?
- Given input is arrays.
- Need to compare values across arrays, which points towards [two-arrays](../concepts/two-arrays.md) variation.
## Brute Force:
- Build a frequency count of nums2 (value → how many times it appears).
- For each element in nums1, check if its count in the frequency map is > 0. If so, add it to the result list and decrement its count.
## Observation:
- We need to sort both the arrays.
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
- If `nums1[pointer1] < nums2[pointer2]` move `pointer1++` forward.
- If `nums1[pointer1] == nums2[pointer2]` store that element in a list and move both pointers forward.
- If `nums1[pointer1] > nums2[pointer2]` move `pointer2++` forward.

At last all the interesting elements will be in the list.
Copy the elements in the list to a new array.
## Complexity:
|                 | Time                 | Space                                       |
| --------------- | -------------------- | ------------------------------------------- |
| **Brute Force** | O(n * m)             | O(m) for the frequency map, O(k) for output |
| **Optimized**   | O(n log n + m log m) | O(log n + log m + k)                        |
## What would break this approach?
- None, but there is better approach we will see in upcoming topics.
## Code I wrote:
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
  
        int pointer1 = 0;
        int pointer2 = 0;
  
        List<Integer> list = new ArrayList<>();
  
        while (pointer1 < nums1.length && pointer2 < nums2.length) {
            if (nums1[pointer1] < nums2[pointer2]) {
                pointer1++;
            } else if (nums1[pointer1] == nums2[pointer2]) {
                list.add(nums1[pointer1]);

                pointer1++;
                pointer2++;
            } else {
                pointer2++;
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