---
pattern: Sliding window
variation: Variable-size window
difficulty: Medium
createdDate: 2026-07-22
link: https://leetcode.com/problems/longest-repeating-character-replacement/description/
---
## Problem Summary:
- Given a string `s` consisting of uppercase English letters, and an integer `k`, return the length of the longest substring containing the same letter, after replacing at most `k` characters.
## What made me recognize the pattern?
- Problem involves a contiguous substring.
- We need the **longest** window satisfying a condition.
- Window size is decided based on a condition — points towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible substring (all start/end pairs).
- For each one, find its most frequent character's count, and check if `(window length - most frequent count) ≤ k`.
- Track the max length among valid substrings.
## Observation:
- To make a window all one character with the fewest replacements, always convert everything to the character that **already appears most often** in the window.
- So the number of replacements needed for a window = `(window length) - (count of most frequent character in window)`.
- We don't need to rescan the whole frequency array to find the current max every step — track a running `maxFreq` that only ever **increases**, updated whenever the newly added character's count grows past it.
## Intuition:
Initialize:
- `freq[26]` — frequency count of characters in the current window.
- `l = 0`, `maxFreq = 0`, `maxLen = 0`.

Traverse the string with `r`:
- Add `s.charAt(r)` into `freq`.
- Update `maxFreq = Math.max(maxFreq, freq[s.charAt(r) - 'A'])`.
- If `(r - l + 1) - maxFreq > k` (window needs more than `k` replacements):
    - Remove `s.charAt(l)` from `freq`, move `l++`.
- Update `maxLen = Math.max(maxLen, r - l + 1)`.

At the end, return `maxLen`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n * 3) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- Relies on the guarantee that `s` contains only uppercase English letters (fixed 26-size array). If other characters were allowed, the array size and index formula (`charAt(i) - 'A'`) would need to change.
## Code I wrote:
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int l = 0;
        int maxFreq = 0;
        int maxLen = 0;

        for (int r = 0; r < s.length(); r++) {
            freq[s.charAt(r) - 'A']++;
            maxFreq = Math.max(maxFreq, freq[s.charAt(r) -'A']);
            if ((r - l + 1) - maxFreq > k) {
                freq[s.charAt(l) - 'A']--;
                l++;
            }
  
            maxLen = Math.max(maxLen, r - l + 1);
        }
  
        return maxLen;
    }
}
```