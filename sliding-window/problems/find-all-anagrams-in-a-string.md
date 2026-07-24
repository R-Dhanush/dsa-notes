---
pattern: Sliding window
variation: Fixed-size window
difficulty: Medium
createdDate: 2026-07-20
link: https://leetcode.com/problems/find-all-anagrams-in-a-string/description/
---
## Problem Summary:
- Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`.
- `s` and `p` consist of lowercase letters.
- Strings length range from 1 to 3 * 10 ^ 4.
## What made me recognize the pattern?
- Problem involves a contiguous substring.
- Need to find positions where a window matches a specific pattern. it points towards [fixed-size-window](../concepts/fixed-size-window.md) variation.
## Brute Force:
- For each starting position in `s`, extract the substring of length equal to `p`'s length.
- Check if that substring is an anagram of `p` by building a fresh frequency count and comparing.
## Observation:
- The window size is fixed.
- Window size is `p` string length.
- Two strings are anagrams if and only if their character frequencies are identical.
- When the window shifts by one position, only one character leaves and one character enters. 
- Therefore, we can update the window frequency in O(1) time instead of recomputing it.
- If `s` length is less than `p`, then we can state that there is no `p`'s anagram in `s`.
## Intuition:
If `s.length() < p.length()` return empty list.

Initialize two integer arrays:
- `int[] pFreq = new int[26]` - to store `p`'s frequency count.
- `int[] windowFreq = new int[26]` - to track window's frequency count.

Build `p`'s frequency count once.

Initialize two pointers:
- `int left = 0` - point's left most character of the window.
- `int right = 0` - point's right most character of the window, and used to traverse the string.

Traverse the string until `right < s.length()`:
- Add incoming character - `windowFreq[s.charAt(right) - 'a']++`.
- Once window exceeds size `p`'s length, remove the outgoing character - `windowFreq[s.charAt(left) - 'a']--`. and move the left pointer forward.
- Compare once the window has reached size `p`'s length.
	- `right - left + 1 == n && Arrays.equals(pFreq, windowFreq)` - if yes add the starting index of the anagram to list. `list.add(left)`.

At last return the list.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n * k) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- The 26-size frequency arrays rely on the guarantee that s and p contain only lowercase letters. If the problem allowed uppercase, digits, or other characters, the array size (and index formula `charAt(i) - 'a'`) would need to change.
## Code I wrote:
```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int n = p.length();
        List<Integer> list = new ArrayList<>();
  
        if (s.length() < p.length()) {
            return list;
        }
  
        int[] pFreq = new int[26];
        int[] windowFreq = new int[26];
  
        for (int i = 0; i < n; i++) {
            pFreq[p.charAt(i) - 'a']++;
        }
  
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            windowFreq[s.charAt(right) - 'a']++;
  
            if (right - left + 1 > n) {
                windowFreq[s.charAt(left++) - 'a']--;
            }
  
            if (right - left + 1 == n && Arrays.equals(pFreq, windowFreq)) {
                list.add(left);
            }
        }
        return list;
    }
}
```