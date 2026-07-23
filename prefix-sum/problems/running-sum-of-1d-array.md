---
pattern: Prefix sum
variation: Prefix sum
difficulty: Easy
createdDate: 2026-07-23
link: https://leetcode.com/problems/running-sum-of-1d-array/description/
---
## Problem Summary:
- Given an array `nums`. return the running sum of `nums`.
## What made me recognize the pattern?
- Need to return prefix sum array.
## Brute Force:
- For each index i, loop through all elements from 0 to i and sum them.
## Observation:
- Each running sum value only depends on the previous running sum plus the current element — no need to re-sum from scratch each time.
## Intuition:
For each index i from 1 to end: 
- nums[i] = nums[i-1] + nums[i] 

Return nums.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- This approach modifies the input array in place. If the problem required preserving the original nums array, a separate output array would be needed instead (O(n) space).
## Code I wrote:
```java
class Solution {
    public int[] runningSum(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            nums[i] = nums[i - 1] + nums[i];
        }
        return nums;
    }
}
```