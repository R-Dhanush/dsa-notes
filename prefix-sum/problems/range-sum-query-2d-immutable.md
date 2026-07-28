---
pattern: Prefix sum
variation: Prefix sum
difficulty: Medium
createdDate: 2026-07-28
link: https://leetcode.com/problems/range-sum-query-2d-immutable/description/
---
## Problem Summary:
- Given a 2D matrix `matrix`, implement the NumMatrix class:
    - `NumMatrix(int[][] matrix)` initializes the object with the matrix.
    - `int sumRegion(int row1, int col1, int row2, int col2)` returns the sum of the elements inside the rectangle defined by its upper-left corner `(row1, col1)` and lower-right corner `(row2, col2)`, inclusive.
- Multiple `sumRegion` queries will be made on the same matrix.
## What made me recognize the pattern?
- Need to answer repeated range-sum queries on a fixed 2D grid — same shape as Range Sum Query - Immutable, extended to two dimensions.
## Brute Force:
- For each `sumRegion` query, loop through every cell in the rectangle `[row1..row2][col1..col2]` and sum them directly.
## Observation:
- Same padding trick as the 1D version: build `prefix` with size `(rows+1) × (cols+1)`, so `prefix[0][*]` and `prefix[*][0]` act as a zero-padded border — no special-casing needed for regions touching row 0 or column 0.
- `prefix[i][j]` represents the sum of the rectangle from `(0,0)` to `(i-1,j-1)` in the original matrix.
- Building each cell requires **inclusion-exclusion**: adding the region above (`top`) and the region to the left (`left`) double-counts the top-left overlapping corner, so it must be subtracted once (`topLeft`).
- The same inclusion-exclusion idea applies when _querying_ a sub-rectangle — not just when building the prefix array. To get the sum strictly inside `[row1,col1]` to `[row2,col2]`, subtract the region above `row1` and the region left of `col1`, but the top-left corner beyond both gets subtracted twice, so it must be added back once.
## Intuition:
**Constructor:**
- Guard against an empty/null matrix.
- Build `prefix` sized `(matrix.length+1) × (matrix[0].length+1)`, all zero-initialized.
- For each cell `(i, j)` from `(1,1)` onward:
    - `top = prefix[i-1][j]`, `left = prefix[i][j-1]`, `topLeft = prefix[i-1][j-1]`.
    - `prefix[i][j] = matrix[i-1][j-1] + top + left - topLeft`.
**sumRegion(row1, col1, row2, col2):**
- `prefix[row2+1][col2+1]` — sum of everything from `(0,0)` through `(row2,col2)`.
- Subtract `prefix[row1][col2+1]` — removes everything above the target region.
- Subtract `prefix[row2+1][col1]` — removes everything to the left of the target region.
- Add back `prefix[row1][col1]` — the top-left corner outside the region was subtracted twice (once in each of the two subtractions above), so add it back once.
- Return the result.
## Complexity:
|                 | Time                                                 | Space                              |
| --------------- | ---------------------------------------------------- | --------------------------------- |
| **Brute Force** | O(rows × cols) per query                             | O(                                 |
| **Optimized**   | O(rows × cols) one-time construction, O(1) per quer O(rows x cols) - for prefix array s)  |
## What would break this approach?
## Code I wrote:
```java
class NumMatrix {
    private int[][] prefix;
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return;
        }
        prefix = new int[matrix.length + 1][matrix[0].length + 1];\
        for (int i = 1; i < prefix.length; i++) {
            for (int j = 1; j < prefix[0].length; j++) {
                int top = prefix[i - 1][j];
                int left = prefix[i][j - 1];
                int topLeft = prefix[i - 1][j - 1];
                prefix[i][j] = matrix[i - 1][j - 1] + top + left - topLeft;
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return prefix[row2 + 1][col2 + 1] - prefix[row1][col2 + 1] - prefix[row2 + 1][col1] + prefix[row1][col1];
    }
}
```