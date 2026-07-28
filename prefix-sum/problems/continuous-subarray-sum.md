---
pattern: Prefix sum
variation: Prefix sum
difficulty: Medium
createdDate: 2026-07-24
link: https://leetcode.com/problems/continuous-subarray-sum/
---
## Problem Summary:
- Given an integer array `nums` and an integer `k`, return `true` if `nums` has a good subarray, otherwise return `false`.
- A good subarray is a contiguous subarray where:
    - Its length is at least 2.
    - The sum of its elements is a multiple of `k`.
- In other words, `sum % k == 0`.
## What made me recognize the pattern?
- Need to determine whether a subarray sum satisfies a mathematical condition.
- The condition involves checking if a subarray sum is divisible by `k`.
- [prefix-sum](../concepts/prefix-sum.md) with modulo is useful when checking divisibility properties of subarray sums.
## Brute Force:
- Generate every possible subarray of length at least 2.
- Calculate its sum and check whether `sum % k == 0`.
## Observation:
- If two prefix sums have the same remainder when divided by `k`, then the sum of the elements between them is divisible by `k`.
- Suppose:
    - `prefixSum(i) % k == prefixSum(j) % k`
    - Then, `(prefixSum(j) - prefixSum(i)) % k == 0`.
- Since the difference of two prefix sums gives the subarray sum, the subarray between indices `i + 1` and `j` is divisible by `k`.
- Therefore, we only need to remember the first index at which every remainder was seen.
- If we encounter the same remainder again and the distance between the two indices is at least 2, we have found a valid subarray.
## Intuition:
Initialize:
- `map` - stores `{remainder -> first index where it occurred}`.
- Insert `{0 : -1}` into the map. This represents a prefix sum of zero before the array starts, allowing subarrays beginning at index 0 to be handled naturally.
- `runningSum = 0`.

Traverse the array:
- Add `nums[i]` to `runningSum`.
- Compute `remainder = runningSum % k`.
- If `remainder` has been seen before:
    - Check whether `i - previousIndex >= 2`.
    - If true, return `true`.
- Otherwise, store the remainder and its index in the map.
    - We store only the first occurrence because it gives the maximum possible subarray length.
- If no valid subarray is found after traversing the array, return `false`.
## Complexity:
|                 | Time                        | Space                                                                                                         |
| --------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Brute Force** | O(n ^ 2)                    | O(1)                                                                                                          |
| **Optimized**   | O(n) - traversed the array. | O(min(n, k)) - the map can hold at most `k` distinct remainders, or at most `n` entries, whichever is smaller |
## What would break this approach?
- None.
## Code I wrote:
```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> index = new HashMap<>();
        index.put(0, -1);
  
        int prefixSum = 0;
        for(int i = 0; i < nums.length; i++) {
            prefixSum += nums[i];
            int reminder = prefixSum % k;
            if(index.containsKey(reminder)) {
                if(i - index.get(reminder) >= 2) {
                    return true;
                }
            }else {
                index.put(reminder, i);
            }
        }
  
        return false;
    }
}
```