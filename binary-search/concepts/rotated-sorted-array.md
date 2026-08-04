# Rotated Sorted Array
- The array was originally sorted, but rotation breaks the normal binary search property that the left half is always smaller than the right half.
- Before deciding which half to discard, first determine which half is still sorted.
- Once the sorted half is identified, check whether the target lies within that sorted range.
    - If yes, discard the other half.
    - Otherwise, discard the sorted half.
## Problems
- [search-in-rotated-sorted-array](../problems/search-in-rotated-sorted-array.md)