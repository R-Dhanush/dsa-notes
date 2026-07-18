---
pattern: Two pointers
variation: Two fixed element + Opposite ends
difficulty: Medium
solvedDate: 2026-07-18
link: https://leetcode.com/problems/4sum/description/
---
## Problem Summary:
- Given an array `nums` , need to find all the **unique** quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that they add up to a specific `target` number.
- `a`, `b`, `c`, and `d` are **distinct**.
- Array length range from 1 to 200.
- Values range from -10 ^ 9 to 10 ^ 9.
- Target range from -10 ^ 9 to 10 ^ 9.
## What made me recognize the pattern?
- Given input is array.
- Given data is sortable.
- Need to find quadruplets. which points to [two-fixed-element+opposite-ends](../concepts/two-fixed-element+opposite-ends.md).
## Brute Force:
- Check every quadruplets in the array, and return all unique quadruplets such that they add up to a specific `target` number.
## Observation:
- Need to sort the array:
	- To know which side to move the pointers.
	- To skip the duplicates.
- We can fix two pointers (`i`, `j`) and control the sum with remaining two pointers (`left`, `right`).
- Max value is 10 ^ 9, where the sum equals to 4 * 10 ^ 9. so we need to use `long` for sum. to overcome overflow error.
- `long sum = nums[i] + nums[j] + nums[left] + nums[right]` - here the right hand side first calculate. but in right hand side all are `int` so Java adds them together as plain `int` arithmetic. The overflow already happened before the conversion. so we need to force at least one operand to be `long` _before_ the addition happens.
## Intuition:
First we need to sort the array:
	`Arrays.sort(nums)`


First for loop traverse the array using `i` until it is less than `nums.length - 3`. Because we need three more values including `i` to form a quadruplet:
- If `i` is greater than 0 and `nums[i] == nums[i - 1]`, then we need to skip the iteration because we should not have the duplicate values.
- Here `i` act as the first fixed point.
- Nested for loop traverse the array using `j = i + 1` (Here we start `j` pointer from `i + 1` because `nums[i]` is already a element in quadruplet. so we need to start from the next element to avoid duplicate.) until it is less than `nums.length - 2`. Because we need two more values including `i` and `j` to form a quadruplet.
	- Now initialize two pointers:
		- `left = j + 1`. Here we start `left` pointer from `j + 1` because `nums[j]` is already a element in quadruplet. so we need to start from the next element to avoid duplicate. 
		- `right = nums.length - 1`.
	- Until `left < right`:
		- Calculate `sum = (long) nums[i] + nums[j] + nums[left] + nums[right]`.
		- Case 1 -> If `sum` is less than `target`.
			- `left++`
			- Why? Since the array is sorted, moving left -> right gives a larger value, increasing the sum.
		- Case 2 -> if `sum` is equal to `target`.
			- We find out the quadruplet.
			- Store it in a nested list.
			- Move both the pointers `left++` and `right--`.
			- Until `nums[left] == nums[left - 1]` skip it  `left++`, we need to skip the duplicate values.
			- Until `nums[right] == nums[right + 1]` skip it  `right--`, we need to skip the duplicate values.
		- Case 3 -> if `sum` is greater than `target`.
			- `right--`
			- Why? Since the array is sorted, moving right -> left gives a smaller value, decreasing the sum.

At last return the quadruplet list.
## Complexity:
|                 | Time     | Space                                                                                                                                                                          |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Brute Force** | O(n ^ 4) | O(k)                                                                                                                                                                           |
| **Optimized**   | O(n ^ 3) | O(log n) + O(k) -> Space = O(log n) from the recursive call stack of `Arrays.sort()` (quicksort halves the array at each level), plus O(k) for storing the output quadruplets. |
## What would break this approach?
- If the array weren't sorted, this approach breaks entirely. 
## Code I wrote:
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
  
        List<List<Integer>> list = new ArrayList<>();
  
        for (int i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
  
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
  
                int left = j + 1;
                int right = nums.length - 1;
  
                while (left < right) {
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
  
                    if (sum < target) {
                        left++;
                    } else if (sum == target) {
                        list.add(new ArrayList<>(List.of(nums[i], nums[j], nums[left], nums[right])));
  
                        left++;
                        right--;
  
                        while (left < right && nums[left] == nums[left - 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right + 1]) {
                            right--;
                        }
                    } else {
                        right--;
                    }
                }
            }
        }
        return list;
    }
}
```