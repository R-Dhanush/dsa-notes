---
pattern: Prefix sum
variation: Prefix sum
difficulty: Medium
createdDate: 2026-07-28
link: https://leetcode.com/problems/product-of-array-except-self/
---
## Problem Summary:
- Given an integer array `nums`, return _an array_ `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
- The input is generated such that `answer[i]` is **guaranteed** to fit in a **32-bit** integer.
## What made me recognize the pattern?
- Need to find the product of everything except the current index — a mix of "everything before" and "everything after" a position, which points towards Prefix Product + Suffix Product.
## Brute Force:
- For every index `i`, loop through the array and multiply all elements except `nums[i]`.
- Store the result in a new array.
## Observation:
- The naive shortcut - multiply everything once, then divide by `nums[i]` for each index, it breaks the moment a `0` appears in the array (division by zero), and gives wrong results with multiple zeros. This approach needs to avoid division entirely.
- `product except self at i` = `(product of everything before i)` × `(product of everything after i)`.
- Instead of recomputing these products from scratch at each index, build them incrementally: a prefix product pass (left to right), then a suffix product pass (right to left).
- The output array itself can double as the prefix-product storage during the first pass, avoiding the need for a third array.
## Intuition:
Initialize:
- `int[] answer = new int[nums.length]`
- `answer[0] = 1` - there's nothing before index 0, so its prefix product is 1.
First pass (prefix product, left to right):
- `answer[i] = answer[i - 1] * nums[i - 1]` — running product of everything strictly before index `i`.
Second pass (suffix product, right to left):
- `suffix = 1` — nothing after the last index.
- At each index (from the end backward): `answer[i] = answer[i] * suffix` (multiply the existing prefix product by the running suffix product), then `suffix *= nums[i]` (extend the suffix product to include this element for the next index to the left).

At last return the `answer` array.
## Complexity:
|                 | Time                                    | Space                                                                                                                  |
| --------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Brute Force** | O(n ^ 2)                                | O(1)                                                                                                                   |
| **Optimized**   | O(n) - two linear passes over the array | O(1) - not counting the output array itself (required by the problem), only one additional variable (`suffix`) is used |
## What would break this approach?
- None.
## Code I wrote:
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] answer = new int[nums.length];
        answer[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            answer[i] = answer[i - 1] * nums[i - 1];
        }
  
        int suffix = 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            answer[i] = answer[i] * suffix;
            suffix *= nums[i];
        }
  
        return answer;
    }
}
```