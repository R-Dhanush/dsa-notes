---
pattern: Two pointers
variation: Opposite ends
difficulty: Medium
solvedDate: 2026-07-16
link: https://leetcode.com/problems/container-with-most-water/
---
## Problem Summary:
- Given an array of heights representing vertical lines, find two lines that, together with the x-axis, form a container holding the most water. Return the max area.
- Array length will be from 2 to 10 ^ 5.
- Values range 0 to 10 ^  4
## What made me recognize the pattern?
- Input is an array.
- Need a pair of heights.
- Need to reduce a nested-loop brute force to a single pass.
- Need to find optimal pair from unsorted array based on value based decision. which point towards opposite ends.
## Brute Force:
- Check every pair (i, j), calculate area = min(height[i], height[j]) * (j - i), track the max.
- Return max area.
## Observation:
- A container is formed using any two walls.
- The amount of water stored depends on:
  - **Width** = distance between the two walls.
  - **Height** = the shorter of the two walls.
- Since water cannot rise above the shorter wall,  `Container Area = min(leftHeight, rightHeight) × width`
## Intuition:
Initialize two pointers:
- left = 0
- right = array length - 1

For every pair of walls:
1. Calculate the width.
2. Find the smaller height.
3. Calculate the container area.
4. Update `maxArea` if the current area is larger.

After calculating the area, move the pointer pointing to the **shorter wall**, Why?.
- The current area is limited by the shorter wall.
- Moving the taller wall only decreases the width while the limiting height stays the same or becomes even smaller.
- The only chance to get a larger area is by finding a taller wall than the current shorter one.
## Complexity:
|                 | Time     | Space |
| --------------- | -------- | ----- |
| **Brute Force** | O(n ^ 2) | O(1)  |
| **Optimized**   | O(n)     | O(1)  |
## What would break this approach?
- All heights equal - still works correctly since area = height * width, no special case needed.
## Code I wrote:
```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;
        while (left < right) {
            int containerArea = Math.min(height[left], height[right]) * (right - left);
            maxArea = Math.max(maxArea, containerArea);

            if (height[left] < height[right])
                left++;
            else
                right--;
        }
        return maxArea;
    }
}
```