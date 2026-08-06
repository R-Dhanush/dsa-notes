# Minimize Maximum
- Find the **smallest** X such that `isFeasible(X)` is true.
- Feasibility goes **false → true** as X increases (small X = not enough, large X = enough).
- When `isFeasible(mid)` is true, `mid` could still be the answer — keep it in range: `high = mid` (not `mid - 1`). Excluding it risks losing the true answer if nothing smaller ever works.
## Problems
- [koko-eating-bananas](../problems/koko-eating-bananas.md)