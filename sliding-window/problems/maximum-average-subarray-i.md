---
pattern: Sliding window
variation: Fixed-size window
difficulty: Easy
solvedDate: 2026-07-20
link: https://leetcode.com/problems/maximum-average-subarray-i/description/
---
## Problem Summary:
- Given an integer array `nums` consisting of `n` elements, and an integer `k`.
- Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return the value.
- Array length and k range from 1 to 10 ^ 5.
- Values range from -10 ^ 4 to 10 ^ 4.
## What made me recognize the pattern?
- Problem involves contiguous subarray.
- Need to find maximum average of a window.
- The window size is fixed. it points towards [fixed-size-window](../concepts/fixed-size-window.md) variation.
## Brute Force:
- Traverse every possible subarray of size `k` and calculate its sum by iterating through its `k` elements.
- Keep track of the maximum subarray sum and return `maxSum / k` as the maximum average.
## Observation:
- The window size is fixed at k. 
- When the window shifts by one position, only one element leaves and one element enters. 
- Therefore, we can update the window sum in O(1) time instead of recomputing it.
- Here `k` is fixed for every window, we only need to track the maximum window sum and divide by k once at the end.
## Intuition:
Initialize pointer:
- `pointer = 0` -> used to traverse the array.

First loop traverse the array up to `pointer < k` to set the first window.
- calculate `sum += nums[pointer++]`

Initialize `maxSum`;
- `maxSum = sum` - initialize `maxSum` with first window sum.

Second loop to traverse the remaining windows.
- Every time the window shifts:
	- Remove the element which is going out of the window, `sum - nums[pointer - k]`.
	- Add the new element which is coming in, `sum + nums[pointer]`.
	- Update `maxSum = Math.max(sum, maxSum)`.

At last we will have max sum, return average of it `(double) maxSum / k` - both the `maxSum` and `k` values are integer so if we need average we need to convert any of one value to `double` first.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n * k) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- This approach relies on the subarray being contiguous. If the problem instead asked for a subsequence (elements not required to be adjacent), this fixed-window technique wouldn't apply.
## Code I wrote:
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int pointer = 0;
  
        int sum = 0;
        while (pointer < k) {
            sum += nums[pointer++];
        }
  
        int maxSum = sum;
  
        while (pointer < nums.length) {
            sum = sum - nums[pointer - k] + nums[pointer++];
            maxSum = Math.max(sum, maxSum);
        }
        return (double) maxSum / k;
    }
}
```