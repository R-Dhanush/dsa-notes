---
pattern: Sliding window
variation: Variable-size window
difficulty: Medium
solvedDate: 2026-07-21
link: https://leetcode.com/problems/max-consecutive-ones-iii/description/
---
## Problem Summary:
- Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`'s in the array, we can flip at most `k` `0`'s.
- Array length range will be from 1 to 10 ^ 5.
- `k` value range will be from 0 to array length.
## What made me recognize the pattern?
- Problem involves a contiguous subarray.
- We need to find Longest window satisfying a condition.
- Here the window size is decided based on a condition. it point's towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible subarray (all start/end pairs).
- For each one, count how many 0's it contains.
- If the count of 0's is ≤ k, it's valid track the max length among valid ones.
## Observation:
- We can allow at most `k` `0`'s in the window.
- We need to return the maximum window size consist of `1`'s + at most `k` `0`'s.
## Intuition:
Initialize:
- `left = 0`, `right = 0` - window boundaries.
- `maxLen = 0` - tracks the longest valid window seen so far.
- `count = 0` - tracks number of `0`'s in window.

Traverse the array with `right`:
- If `right`'s current value is `0` then increase the `count`.
- While the window consist `0`'s more than `k`, we need to shrink the window:
	- If `left`'s current value is `0` then decrease the `count`.
	- Move the `left` pointer forward.
- Update the `maxLen`.
- Move the `right` pointer forward.

At last return the `maxLen`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- None
## Code I wrote:
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int right = 0;
        int maxLen = 0;
        int count = 0;
  
        while (right < nums.length) {
            if (nums[right] == 0) {
                count++;
            }
  
            while (count > k) {
                if (nums[left] == 0) {
                    count--;
                }
                left++;
            }
  
            maxLen = Math.max(maxLen, right - left + 1);
            right++;
        }
  
        return maxLen;
    }
}
```