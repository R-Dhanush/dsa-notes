---
pattern: Prefix sum
variation: Prefix sum
difficulty: Medium
createdDate: 2026-07-23
link: https://leetcode.com/problems/subarray-sum-equals-k/description/
---
## Problem Summary:
- Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.
- Array length range from 1 to 2 * 10 ^ 4.
- Values range from -1000 to 1000.
## What made me recognize the pattern?
- Contiguous subarray + sum - smells like Sliding Window at first glance but array as negative numbers.
- Need to count the subarrays which matches the condition.
## Brute Force:
- For each index i, loop through all elements from 0 to i and sum them.
- If the sum equals to `k` then count it.
## Observation:
- Each running sum value only depends on the previous running sum plus the current element — no need to re-sum from scratch each time.
- If we need to find out sum of subarray (i , j) then `runningSum(j) - runningSum(i - 1)`.
- If we need to find out subarray sum equals to `k` then `runningSum(j) - runningSum(i - 1) == k`.
- We are going to store the current `runningSum()` in a HashMap and the Frequency. if HashMap contains `currentRunningSum - k` then will we add the frequency to our count. Because if the frequency is 4 then we consider it as 4 subarrays who sum equals to `k`.
## Intuition:
Initialize:
- `map` — HashMap tracking how many times each running sum has occurred so far. Pre-seed with `{0: 1}` — represents "a running sum of 0 occurred once, before any elements were added" 
- `runningSum = 0`, `count = 0`. 

Traverse the array:
- Add `nums[i]` to `runningSum`. 
- If `map` contains a key equal to `runningSum - k`, add its stored frequency to `count` — each occurrence marks a valid subarray ending at the current index. 
- Record the current `runningSum` in `map`, incrementing its frequency. 

Return `count`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(n)  |
## What would break this approach?
- None
## Code I wrote:
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
  
        int runningSum = 0;
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            runningSum += nums[i];
  
            if (map.containsKey(runningSum - k)) {
                count += map.get(runningSum - k);
            }
  
            map.merge(runningSum, 1, Integer::sum);
        }
  
        return count;
    }
}
```