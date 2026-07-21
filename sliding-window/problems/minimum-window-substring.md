---
pattern: Sliding window
variation: Variable-size window
difficulty: Hard
solvedDate: 2026-07-21
link: https://leetcode.com/problems/minimum-window-substring/description/
---
## Problem Summary:
- Given two strings `s` and `t`, return the **minimum window substring** of `s` such that every character in `t` (including duplicates) is included in the window.
- If there is no such substring, return the empty string `""`.
- `s` and `t` consist of English letters.
## What made me recognize the pattern?
- Problem involves a contiguous substring.
- We need the **shortest** window satisfying a condition (contains all of `t`'s characters).
- Window size is decided based on a condition, not fixed upfront — points towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible substring of `s` (all start/end pairs).
- For each one, verify if it contains all characters of `t` (with correct counts) by building a fresh frequency count and comparing.
- Track the shortest substring that passes.
## Observation:
- We need to track two things:
    - `t`'s required frequency of each character (built once, upfront).
    - The current window's frequency of each character (updated incrementally as the window slides).
- Track a single counter (`formed`) — how many _distinct_ characters in `t` currently have **enough** count in the window — and compare that to `required` (how many distinct characters `t` needs). This correctly allows extra characters and correctly requires enough duplicates when `t` itself has repeated characters.
## Intuition:
Initialize:
- `tFreq[128]` — frequency count of `t`, built once.
- `windowFreq[128]` — frequency count of the current window.
- `required` — number of **distinct** characters in `t` (count of non-zero entries in `tFreq`).
- `formed` — number of distinct characters currently satisfied in the window (starts at 0).
- `left = 0`, `minLen = Integer.MAX_VALUE`, `index[2]` — to store the best window found.

Traverse the string with `right`:
- Add `s.charAt(right)` into `windowFreq`.
- If this character is required by `t`, and its window count **just reached exactly** `tFreq`'s requirement (`windowFreq[c] == tFreq[c]`), increment `formed` — one more requirement satisfied.
- **While** `formed == required` (window is currently fully valid):
    - If this window is shorter than the best found so far, update `minLen` and record `left`/`right` in `index`.
    - Remove `s.charAt(left)` from `windowFreq`.
    - If this character was required, and its count **just dropped below** `tFreq`'s requirement, decrement `formed` — one requirement broken.
    - Move `left++`.

At the end, if `minLen` was never updated, return `""`; otherwise return the substring from `index[0]` to `index[1]` (inclusive).
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 3) | O(k)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- If `t` is longer than `s`, no valid window can exist.
## Code I wrote:
```java
class Solution {
    public String minWindow(String s, String t) {
        int[] tFreq = new int[128];
        int[] windowFreq = new int[128];
        
        for (char ch : t.toCharArray()) {
            tFreq[ch]++;
        }
        
        int required = 0;
        for (int f : tFreq) {
            if (f > 0) required++;
        }
        
        int formed = 0;
        int left = 0;
        int minLen = Integer.MAX_VALUE;
        int[] index = new int[2];
        
        for (int right = 0; right < s.length(); right++) {
            char rightChar = s.charAt(right);
            windowFreq[rightChar]++;
            
            if (tFreq[rightChar] > 0 && windowFreq[rightChar] == tFreq[rightChar]) {
                formed++;
            }
            
            while (formed == required) {
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    index[0] = left;
                    index[1] = right;
                }
                
                char leftChar = s.charAt(left);
                windowFreq[leftChar]--;
                
                if (tFreq[leftChar] > 0 && windowFreq[leftChar] < tFreq[leftChar]) {
                    formed--;
                }
                
                left++;
            }
        }
        
        return minLen == Integer.MAX_VALUE ? "" : s.substring(index[0], index[1] + 1);
    }
}
```