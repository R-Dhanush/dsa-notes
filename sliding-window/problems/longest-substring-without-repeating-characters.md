---
pattern: Sliding window
variation: Variable-size window
difficulty: Medium
solvedDate: 2026-07-20
link: https://leetcode.com/problems/longest-substring-without-repeating-characters/description/
---
## Problem Summary:
- Given a string `s`, find the length of the **longest** **substring** without duplicate characters.
- `s` consists of English letters, digits, symbols and spaces.
- String length range from 0 to 5 * 10 ^ 4.
## What made me recognize the pattern?
- Problem involves a contiguous substring.
- We need to find Longest window satisfying a condition.
- Here the window size is decided based on a condition. it point's towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible substring (all pairs of start/end positions), and for each one, verify if all characters are unique (using a Set).
- Track the maximum length among all substrings that pass.
## Observation:
- We need to track which characters are currently inside our window — a `Set` is a natural fit since it tells us instantly whether a character is already present.
- A duplicate can only be caused by the character currently entering at `right`. So we only need to check `s.charAt(right)` against the set, not the whole window.
- When a duplicate is found, we don't know exactly where the earlier copy is — so we remove characters from `left` one at a time until the duplicate is gone from the set.
## Intuition:
Initialize:
- `left = 0`, `right = 0` — window boundaries.
- `maxLen = 0` — tracks the longest valid window seen so far.
- `unique` — a `HashSet` tracking characters currently in the window.

Traverse the string with `right`:
- **While** `s.charAt(right)` is already in `unique` (window is invalid — contains a duplicate):
    - Remove `s.charAt(left)` from `unique`, and move `left++`.
    - Keep repeating until the duplicate character is gone from the set.
- Once the window is valid, add `s.charAt(right)` to `unique`.
- Update `maxLen = Math.max(maxLen, right - left + 1)` — current window size.
- Move `right++`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 3) | O(k)  |
| **Optimized**   | O(n)     | O(k)  |
## What would break this approach?
- None.
## Code I wrote:
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> unique = new HashSet<>();
        int left = 0;
        int right = 0;
        int maxLen = 0;

        while (right < s.length()) {
            while (unique.contains(s.charAt(right))) {
                unique.remove(s.charAt(left++));
            }
  
            unique.add(s.charAt(right));
  
            maxLen = Math.max(maxLen, right - left + 1);
  
            right++;
        }
  
        return maxLen;
    }
}
```