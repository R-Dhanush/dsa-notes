# Boundary Search (First/Last Valid Position)
- Goal: Find the first or last position that satisfies a condition, rather than just finding any matching element.
- The search space must be monotonic (e.g., `false false false true true true`).
- Even after finding a valid candidate, continue searching to the left or right boundary.
- Unlike standard binary search, we don't immediately return when the condition is satisfied.
- The loop usually ends when the search space is reduced to a single candidate, which is the required boundary.
## Problems
- [first-bad-version](../problems/first-bad-version.md)
- [search-insert-position](../problems/search-insert-position.md)