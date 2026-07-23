# Prefix Sum
- Instead of recomputing a range's value every time a new query comes in, precompute cumulative values once beforehand, and answer range questions in O(1) via subtraction/combination.
- Use this when: 
	- You need to answer the SAME KIND of range question repeatedly on a FIXED array (e.g. "sum from i to j," asked many times). 
	- OR you need to count/find how many ranges satisfy a condition (e.g. "how many subarrays sum to K") - this variant pairs prefix sum with a HashMap, since it's a search/count problem, not a direct lookup.
## Problems
1. [range-sum-query-immutable](../problems/range-sum-query-immutable.md)
2. [running-sum-of-1d-array](../problems/running-sum-of-1d-array.md)