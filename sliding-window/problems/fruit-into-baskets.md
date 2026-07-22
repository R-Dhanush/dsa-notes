---
pattern: Sliding window
variation: Variable-size window
difficulty: Medium
createdDate: 2026-07-22
link: https://leetcode.com/problems/fruit-into-baskets/description/
---
## Problem Summary:
- A farm has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the `ith` tree produces.
- Need to collect as much fruit as possible but
	- We have only **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
	- We must pick **exactly one fruit** from **every** tree while moving to the right. The picked fruits must fit in one of the baskets.
- Need to return maximum number of fruits we pick.
## What made me recognize the pattern?
- Problem involves a contiguous subarray.
- We need to find the **longest** window.
- Window size is decided based on a condition, not fixed upfront — points towards [variable-size-window](../concepts/variable-size-window.md) variation.
## Brute Force:
- Check every possible subarray (all start/end pairs).
- For each one, count how many distinct fruit types it contains.
- If distinct types ≤ 2, track the max length among valid subarrays.
## Observation:
- We can count only two types of fruits at a time. if new type appears we need to remove one type of fruits from the basket and add new type fruit to the basket.
- We can expand the window until we found new type of fruit and need shrink the window to add the new fruit type.
## Intuition:
We need a `HashMap` to group the two types of fruits and their counts.
Initialize:
- `left = 0`, `right = 0` -> window boundaries.
- `maxCount` -> to track maximum number of fruits.
Traverse the array with `right`:
- Add the fruit to the basket, `baskets.put(fruits[right], baskets.getOrDefault(fruits[right], 0) + 1)`.
- If basket has more than two types of fruits, then remove the fruits from `left` until basket has two types of fruits and move the `left` pointer forward.
- Keep track `maxCount`.

At last will have max number of fruits of two types. return `maxCount`.
## Complexity:
|                 | Time     | Space                                                                                                                                                                                   |
| --------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Brute Force** | O(n ^ 2) | O(1)                                                                                                                                                                                    |
| **Optimized**   | O(n)     | O(1) - the map size is capped at a small constant (at most 3 entries: 2 valid fruit types + 1 temporarily over the limit before shrinking), regardless of how large the input array is. |
## What would break this approach?
- none
## Code I wrote:
```java
class Solution {
    public int totalFruit(int[] fruits) {
        HashMap<Integer, Integer> baskets = new HashMap<>();
  
        int left = 0;
        int right = 0;
        int maxCount = 0;
  
        while (right < fruits.length) {
            baskets.put(fruits[right], baskets.getOrDefault(fruits[right], 0) + 1);
  
            while (baskets.size() > 2) {
                baskets.put(fruits[left], baskets.get(fruits[left]) - 1);
                if (baskets.get(fruits[left]) == 0) {
                    baskets.remove(fruits[left]);
                }
                left++;
            }
  
            maxCount = Math.max(maxCount, right - left + 1);
            right++;
        }
  
        return maxCount;
    }
}
```