# Binary Search on Answer
- Search over the **space of possible answers**, not the array itself — `low`/`high` represent candidate answer values, not indices.
- Requires a function `isFeasible(x)` — a yes/no check answering "does this candidate answer `x` actually satisfy the problem's requirement?" Usually O(n) to evaluate.
- Requires the feasibility to be **monotonic**: once it flips from false→true (or true→false) as `x` increases, it never flips back. Without this, binary search can't safely eliminate half the search space.
## Variations
1. [minimize-maximum](minimize-maximum.md)
