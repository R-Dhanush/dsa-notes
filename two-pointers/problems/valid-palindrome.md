---
pattern: Two pointers
variation: Opposite ends
difficulty: Easy
solvedDate: 2026-07-16
link: https://leetcode.com/problems/valid-palindrome/description/
---
## Problem Summary:
- Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.
- Convert all uppercase letters into lowercase letters and skip all non-alphanumeric characters.
- The string `s` length will from 1 to 200000.
- The string consist only of printable ASCII characters.
## What made me recognize the pattern?
- Need to compare values at two positions.
- String is processed from the outside toward the center.
- We need to check given string is symmetric. Which points towards Opposite ends variation.
## Brute Force:
- Create a new clean string by converting all uppercase letters into lowercase letters and skip all non-alphanumeric characters.
- Create a new string which is reversed version of clean string, then compare the reversed string and clean string.
## Observation:
- A palindrome has matching characters at equal distances from the beginning and the end.
- By comparing characters from both ends while ignoring non-alphanumeric characters, we can determine whether the string is a palindrome in a single pass.
## Intuition:
Initialize two pointers:
- left = 0
- right = string length - 1

Before comparing:
- If the current character at left is not alphanumeric, skip it -> left++.
- If the current character at right is not alphanumeric, skip it -> right--.

Compare characters:
- After both pointers point to valid alphanumeric character.
- Convert both to lowercase.
- Compare them
	- If they are same.
		- left++
		- right--
	- else
		- return false

Continue until the pointers cross.
If no mismatch is found, the string is palindrome.
### Complexity:
|                 | Time                                                                                                    | Space |
| --------------- | ------------------------------------------------------------------------------------------------------- | ----- |
| **Brute Force** | O(n)                                                                                                    | O(n)  |
| **Optimized**   | O(n) - left and right each move at most n times total across the whole run, regardless of loop nesting. | O(1)  |
## What would break this approach?
- Empty string - loop never runs and return true. but in this problem test case they wont give empty string.
- String with only non-alphanumeric characters - both pointers skip non-alphanumeric characters and cross without comparing, return true.
- Single character in string - loop never runs and return true.
### Code I wrote:
```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while(left < right) {
            while(left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
            while(left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;
            
            if(left < right && Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) return false;

            left++;
            right--;
        }
        return true;
    }
}
```