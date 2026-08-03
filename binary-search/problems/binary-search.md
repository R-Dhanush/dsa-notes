---
pattern: Binary search
variation: Exact value search
difficulty: Easy
createdDate: 2026-08-03
link: https://leetcode.com/problems/binary-search/description/
---
## Problem Summary:
- Given a sorted array of integers `nums` and a target value `target`, return the index of `target` if found, or -1 if not present.
## What made me recognize the pattern?
- Array is sorted.
- Need to find a specific value's index — points towards Binary Search, Exact Value Search variation.
## Brute Force:
- Traverse the array linearly, comparing each element to `target`, return the index on a match.
## Observation:
- Since the array is sorted, comparing `nums[mid]` to `target` tells us which half can be safely discarded — no need to check every element.
- If `nums[mid] > target`, everything from `mid` onward is even larger (sorted), so the answer (if it exists) must be in the left half.
- If `nums[mid] < target`, the answer (if it exists) must be in the right half.
- If `nums[mid] == target`, we've found it.
## Intuition:
Initialize:
- `low = 0`, `high = nums.length - 1`.

Traverse while `low <= high`:
- `mid = low + (high - low) / 2` — avoids overflow compared to `(low + high) / 2`.
- If `nums[mid] > target`: discard the right half, `high = mid - 1`.
- Else if `nums[mid] == target`: found it, return `mid`.
- Else (`nums[mid] < target`): discard the left half, `low = mid + 1`.

If the loop ends without a match, return -1.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
- If the array weren't sorted, this approach breaks entirely — discarding a half based on a single comparison only works because sorted order guarantees everything on one side is uniformly larger or smaller.
## Code I wrote:
```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] > target) {
                high = mid - 1;
            } else if (nums[mid] == target) {
                return mid;
            } else {
                low = mid + 1;
            }
        }
  
        return -1;
    }
}
```