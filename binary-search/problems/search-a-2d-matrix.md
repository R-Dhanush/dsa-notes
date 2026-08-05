---
pattern: Binary search
variation: Exact value search
difficulty: Medium
createdDate: 2026-08-05
link: https://leetcode.com/problems/search-a-2d-matrix/description/
---
## Problem Summary:
- You are given an `m x n` integer matrix `matrix` with the following two properties:
	- Each row is sorted in non-decreasing order.
	- The first integer of each row is greater than the last integer of the previous row.
- Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.
## What made me recognize the pattern?
- The matrix is globally sorted because:
    - Each row is sorted.
    - The first element of every row is greater than the last element of the previous row.
- Need to determine whether an exact target exists, which points towards Binary Search → Exact Match variation.
## Brute Force:
- Traverse the matrix and compare all the elements with given `target`, if the value present in the matrix return true else return false.
## Observation:
- The matrix can be viewed as one sorted 1D array without actually flattening it.
- Any virtual index can be converted back to a matrix position:
    - row = index / numberOfColumns
    - col = index % numberOfColumns
- Since the matrix is sorted, comparing `matrix[row][col]` to `target` tells us which half can be safely discarded — no need to check every element.
- If `matrix[row][col] > target`, everything from `mid` onward is even larger (sorted), so the answer (if it exists) must be in the left half.
- If `matrix[row][col] < target`, the answer (if it exists) must be in the right half.
- If `matrix[row][col] == target`, we've found it.
## Intuition:
Initialize:
- `low = 0`.
- `high = matrix.length * matrix[0].length - 1`.

While `low <= high`:
- Calculate `mid = low + (high - low) / 2`.
- Calculate `row = mid / matrix[0].length`.
- Calculate `col = mid % matrix[0].length`.
- If `matrix[row][col] < target`, `low = mid + 1`.
- Else if `matrix[row][col] == target`, we've found it. Return true.
- Else `high = mid - 1`.

If the `target` is not present in `matrix` return false.
## Complexity:
|                 | Time          | Space |
| --------------- | ------------- | ----- |
| **Brute Force** | O(m * n)      | O(1)  |
| **Optimized**   | O(log(m * n)) | O(1)  |
## What would break this approach?
## Code I wrote:
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int low = 0;
        int high = matrix.length * matrix[0].length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int row = mid / matrix[0].length;
            int col = mid % matrix[0].length;
            if (matrix[row][col] < target) {
                low = mid + 1;
            } else if (matrix[row][col] == target) {
                return true;
            } else {
                high = mid - 1;
            }
        }
        return false;
    }
}
```