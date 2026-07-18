# Opposite Ends
- Pointers start at both ends, move toward the center.
- There are two situations where this pattern is useful:
	1. Just comparing values:
		- No sorting needed.
		- Simply compare the two ends and decide whether they satisfy the condition.
	2. Eliminating possibilities safely:
		- We can prove that some pairs can never produce the answer.
		- Therefore, it is safe to move one of the pointers.
		- The proof usually comes from:
			- Sorted order. 
			- A mathematical property of the problem.
## Problems
1. [valid-palindrome](../problems/valid-palindrome.md) - Just comparing values.
2. [two-sum-II-input-array-is-sorted](../problems/two-sum-II-input-array-is-sorted.md) - safe move comes from sorted array.
3. [container-with-most-water](../problems/container-with-most-water.md) - safe move comes from math fact.
