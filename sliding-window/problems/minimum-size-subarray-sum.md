---
pattern: Sliding window
variation: Variable-size window
difficulty: Medium
solvedDate: 2026-07-21
link: https://leetcode.com/problems/minimum-size-subarray-sum/
---
## Problem Summary:
- Given an array of positive integers `nums` and a positive integer `target`, return the **minimal length** of a contiguous subarray whose sum is greater than or equal to `target`.
- If no such subarray exists, return 0.
## What made me recognize the pattern?
- Problem involves a contiguous subarray.
- We need the **shortest** window satisfying a condition (sum ≥ target).
- Window size is decided based on a condition, not fixed upfront — points towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible subarray (all start/end pairs), calculate its sum.
- Track the minimum length among all subarrays whose sum ≥ target.
## Observation:
- Since all values are **positive**, the window sum grows as the window expands and shrinks as it contracts.
- Once a window's sum reaches or exceeds `target`, it's worth trying to shrink it from the left to see if a smaller valid window still satisfies the condition.
## Intuition:
Initialize:
- `left = 0`, `sum = 0`, `minLen = Integer.MAX_VALUE`.
Traverse the array with `right`:
- Add `nums[right]` into `sum`.
- **While** `sum >= target` (window is currently valid):
    - Update `minLen` with the current window size (`right - left + 1`), if smaller.
    - Remove `nums[left]` from `sum`, and move `left++` — try shrinking further to see if an even smaller window still works.

At the end, if `minLen` was never updated (no valid window found), return 0; otherwise return `minLen`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- This approach would break if negative numbers were allowed in `nums`.
## Code I wrote:
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int minLen = Integer.MAX_VALUE;
        
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            
            while (sum >= target) {
                minLen = Math.min(minLen, right - left + 1);
                
                sum -= nums[left++];
            }
        }
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```