---
pattern: Sliding window
variation: Fixed-size window
difficulty: Medium
createdDate: 2026-07-22
link: https://leetcode.com/problems/permutation-in-string/description/
---
## Problem Summary:
- Given two strings `s1` and `s2`, return `true` if one of `s1`'s permutations is the substring of `s2`, or `false` otherwise.
- Strings length range from 1 to 10 ^ 4.
- `s1` and `s2` consist of lowercase English letters.
## What made me recognize the pattern?
- Problem involves a contiguous substring.
- Need to find whether a fixed window matches a specific pattern. it points towards [fixed-size-window](../concepts/fixed-size-window.md) variation.
## Brute Force:
- For each starting position in `s2`, extract the substring of length equal to `s1`'s length.
- Check if that substring is an permutation of `s1` by building a fresh frequency count and comparing.
## Observation:
- The window size is fixed.
- Window size is `s1` string length.
- When the window shifts by one position, only one character leaves and one character enters. 
- Therefore, we can update the window frequency in O(1) time instead of recomputing it.
- If `s2` length is less than `s1`, then we can state that there is no `s1`'s permutation in `s2`.
## Intuition:
If `s2.length() < s1.length()` return false.

Initialize two integer arrays:
- `int[] s1Freq = new int[26]` - to store `s1`'s frequency count.
- `int[] windowFreq = new int[26]` - to track window's frequency count.

Build `s1`'s frequency count once.

Initialize two pointers:
- `int left = 0` - points to the leftmost character of the window.
- `int right = 0` - points to the rightmost character of the window, and used to traverse the string.

Traverse the string until `right < s2.length()`:
- Add incoming character - `windowFreq[s2.charAt(right) - 'a']++`.
- Once window exceeds size `s1`'s length, remove the outgoing character - `windowFreq[s2.charAt(left) - 'a']--`. and move the left pointer forward.
- Compare once the window has reached size `s1`'s length.
	- `right - left + 1 == n && Arrays.equals(s1Freq, windowFreq)` - if yes return `true`.

At last if there is no permutation found return `false`.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n * k) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- The 26-size frequency arrays rely on the guarantee that s1 and s2 contain only lowercase letters. If the problem allowed uppercase, digits, or other characters, the array size (and index formula `charAt(i) - 'a'`) would need to change.
## Code I wrote:
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s2.length() < s1.length()) return false;
  
        int[] s1Freq = new int[26];
        int[] windowFreq = new int[26];
  
        for (char ch : s1.toCharArray()) {
            s1Freq[ch - 'a']++;
        }
  
        int left = 0;
        for (int right = 0; right < s2.length(); right++) {
            windowFreq[s2.charAt(right) - 'a']++;

            if (right - left + 1 > s1.length()) {
                windowFreq[s2.charAt(left) - 'a']--;
                left++;
            }

            if (right - left + 1 == s1.length() && Arrays.equals(s1Freq, windowFreq)) {
                return true;
            }
        }
  
        return false;
    }
}
```