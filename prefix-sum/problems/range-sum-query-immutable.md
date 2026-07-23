---
pattern: Prefix sum
variation: Prefix sum
difficulty: Easy
createdDate: 2026-07-23
link: https://leetcode.com/problems/range-sum-query-immutable/description/
---
## Problem Summary:
- Given an integer array `nums`, implement the NumArray class: 
	- `NumArray(int[] nums)` initializes the object with the array.
	- `int sumRange(int left, int right)` returns the sum of the elements between indices `left` and `right` inclusive.
- Multiple `sumRange` queries will be made on the same array.
## What made me recognize the pattern?
- Need to answer same kind of range question repeatedly on fixed array.
## Brute Force:
- For every query recompute the sum for given range.
## Observation:
- We no need to directly store the given array.
- We can store the precomputed cumulative sum in a new array. so that we can answer every range query in O(1).
## Intuition:
Constructor: 
- build `prefixSum` with size `nums.length + 1`. 
- `prefixSum[0] = 0` - represents "sum of zero elements," acts as padding. 
- For each index `i`, `prefixSum[i + 1] = prefixSum[i] + nums[i]` - cumulative sum up through index `i`. 

sumRange(left, right): 
- Return `prefixSum[right + 1] - prefixSum[left]`. `prefixSum[right + 1]` is the total sum from the start through `right`. `prefixSum[left]` is the total sum from the start up to (but not including) `left`. Subtracting removes everything before `left`, leaving exactly the sum of [left, right]. 
- Because of the padding (`prefixSum[0] = 0`), this formula works even when `left = 0` — no special-case branch needed.
## Complexity:
|                 | Time                                                    | Space                            |
| --------------- | ------------------------------------------------------- | -------------------------------- |
| **Brute Force** | O(n x q) - O(n) per query, so for q queries is O(n x q) | O(1)                             |
| **Optimized**   | O(n) - for calculating prefix sum, O(1) per query       | O(n) - space to store prefix sum |
## What would break this approach?
- Relies on the array being fixed after construction — if elements needed to change then this approach breaks.
- No explicit bounds-checking is done in this code.
## Code I wrote:
```java
class NumArray {
    int[] prefixSum;
  
    public NumArray(int[] nums) {
        this.prefixSum = new int[nums.length + 1];
  
        for (int i = 0; i < nums.length; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        return prefixSum[right + 1] - prefixSum[left];
    }
}
```