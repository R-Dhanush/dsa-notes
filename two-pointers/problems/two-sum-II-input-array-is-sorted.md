---
pattern: Two pointers
variation: Opposite ends
difficulty: Medium
solvedDate: 2026-07-16
link: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/
---
## Problem Summary:
- Array of integers `numbers` that is already **_sorted in non-decreasing order_**, need to find two numbers such that they add up to a specific `target` number.
- Return indices of the two numbers by incremented by one.
- There will be exactly one solution.
- Should not use same element.
- Array length will be from 2 to 3×10⁴.
- Values range -1000 to 1000.
- Target range -1000 to 1000.
## What made me recognize the pattern?
- Array is sorted.
- Need a pair of numbers.
- Using pointers from opposite ends we can control sum.
## Brute Force:
- Check every pair in the array, and return two numbers such that they add up to the given target number.
## Observation:
The array is already sorted in ascending order.
- Smaller numbers on left.
- Lager numbers on right.

Instead of checking every pair, we can use two pointers.
## Intuition:
Initialize two pointers:
- left = 0
- right = array length - 1

Calculate:
  `sum = numbers[left] + numbers[right]`

Case 1 ->  sum < target:
- Move left pointer
- Why? Since the array is sorted, moving left → right gives a larger value, increasing the sum.

Case 2 -> sum == target:
- We found the required pair.
- Return the indices (`left + 1`, `right + 1`).

Case 3: sum > target:
- Move right pointer
- Why? Since the array is sorted, moving right → left gives a smaller value, decreasing the sum.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- If the array weren't sorted, this approach breaks entirely. but problem constraints guarantee sorted input.
## Code I wrote:
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum < target)
                left++;
            else if (sum == target)
                return new int[] { left + 1, right + 1 };
            else
                right--;
        }
        return new int[] { -1, -1 };
    }
}
```