---
pattern: Two pointers
variation: Fixed element + Opposite ends
difficulty: Medium
solvedDate: 2026-07-17
link: https://leetcode.com/problems/3sum/
---
## Problem Summary:
- Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
- Solution set must not contain duplicate triplets.
- Array length range 3 to 3000.
- Values range -10 ^ 5 to 10 ^ 5.
## What made me recognize the pattern?
- Given input is array.
- Given data is sortable.
- Need to find triplet. which points to [fixed-element+opposite-ends](../concepts/fixed-element+opposite-ends.md).
## Brute Force:
- Check every triplet in the array, and return all triplets such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
## Observation:
- We need to sort the array, so that.
	- Smaller numbers on left.
	- Lager numbers on right.
- We can fix one pointer(`i`) and control the sum with remain two pointers(`left`, `right`):
	- `left` pointer points smaller numbers.
	- `right` pointer points lager numbers.
- We need to skip duplicate values of `i`, `left`, `right`.
## Intuition:
First we need to sort the array:
	`Arrays.sort(nums)`

Traverse the array using `i` until it is less than `nums.length - 2`. Because we need at least three value to form a triplet:
- If `i` is greater than 0 and `nums[i] == nums[i - 1]`, then we need to skip the iteration because we should not have the duplicate triplets.
- Here `i` act as the fixed point.
- Now initialize two pointers:
	- `left = i + 1`. Here we start `left` pointer from `i + 1` because `nums[i]` is already a element in triplet. so we need to start from the next element to avoid duplicate. 
	- `right = nums.length - 1`.
- Until `left < right`:
	- Calculate `sum = nums[i] + nums[left] + nums[right]`.
	- Case 1 -> If `sum` is less than 0.
		- `left++`
		- Why? Since the array is sorted, moving left -> right gives a larger value, increasing the sum.
	- Case 2 -> if `sum` is equal to 0.
		- We find out the triplet.
		- Store it in a nested list.
		- Move both the pointers `left++` and `right--`.
		- Until `nums[left] == nums[left - 1]` skip it  `left++`, we need to skip the duplicate values.
		- Until `nums[right] == nums[right + 1]` skip it  `right--`, we need to skip the duplicate values.
	- Case 3 -> if `sum` is greater than 0.
		- `right--`
		- Why? Since the array is sorted, moving right -> left gives a smaller value, decreasing the sum.

At last return the triplet list.
## Complexity:
|                 | Time     | Space                                                                                                                                                                       |
| --------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Brute Force** | O(n ^ 3) | O(k)                                                                                                                                                                        |
| **Optimized**   | O(n ^ 2) | O(log n) + O(k) -> Space = O(log n) from the recursive call stack of `Arrays.sort()` (quicksort halves the array at each level), plus O(k) for storing the output triplets. |
## What would break this approach?
- If the array weren't sorted, this approach breaks entirely. 
## Code I wrote:
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> list = new ArrayList<>();
  
        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
  
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < 0)
                    left++;
                else if (sum == 0) {
                    list.add(new ArrayList<>(List.of(nums[i], nums[left], nums[right])));
  
                    left++;
                    right--;
  
                    while (left < right && nums[left] == nums[left - 1])
                        left++;
                    while (left < right && nums[right] == nums[right + 1])
                        right--;
                } else
                    right--;
            }
        }
        return list;
    }
}
```