---
pattern: Binary search
variation: Boundary search
difficulty: Easy
createdDate: 2026-08-03
link: https://leetcode.com/problems/first-bad-version/description/
---
## Problem Summary:
- Given `n` versions labeled 1 to `n`, and an API `isBadVersion(version)` that returns whether a given version is bad, find the **first** bad version. Once a version is bad, all following versions are also bad.
## What made me recognize the pattern?
- The `isBadVersion(version)` result is monotonic: it is `false` for all good versions and `true` for all bad versions. Once it becomes `true`, it never becomes `false` again.
- Need to find the first version where the condition becomes `true`, which points to the Binary Search - Boundary Search (First True / Lower Bound) variation.
## Brute Force:
- Check versions one by one, starting from 1, calling `isBadVersion()` until the first bad one is found.
## Observation:
- Since "bad" versions never revert back to "good," once `isBadVersion(mid)` is true, the answer is at `mid` or somewhere to its left — the entire right side can be discarded.
- If `isBadVersion(mid)` is false, the answer must be somewhere to the right — the entire left side (including `mid`) can be discarded.
- This is Boundary Search: even after finding a bad version, we keep narrowing to find the _earliest_ one, rather than stopping immediately.
## Intuition:
Initialize:
- `oldVersion = 1`, `newVersion = n`.

Traverse while `oldVersion <= newVersion`:
- `mid = oldVersion + (newVersion - oldVersion) / 2` — avoids overflow.
- If `isBadVersion(mid)` is true: this could be the first bad version, or the answer could be earlier — narrow the search left, `newVersion = mid - 1`.
- Else: `mid` is good, the first bad version must be later — narrow right, `oldVersion = mid + 1`.

Once the loop ends, `oldVersion` has converged to the first bad version — return it.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n)     | O(1)  |
| **Optimized**   | O(log n) | O(1)  |
## What would break this approach?
- Relies on the monotonic guarantee (once bad, always bad afterward) — if versions could be "good" again after being "bad," this approach breaks entirely, since discarding a half based on one check would no longer be safe.
## Code I wrote:
```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int oldVersion = 1;
        int newVersion = n;
        while (oldVersion <= newVersion) {
            int mid = oldVersion + (newVersion - oldVersion) / 2;
            if (isBadVersion(mid)) {
                newVersion = mid - 1;
            } else {
                oldVersion = mid + 1;
            }
        }
        return oldVersion;
    }
}
```