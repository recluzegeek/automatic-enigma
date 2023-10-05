---
title: Count Sort
updated: 2023-04-12 10:42:32Z
created: 2023-04-12 10:10:10Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---

## Count Sort
- Index based sorting - fastest sorting, consume large memory
## Assumptions and Requirements
- Array size should be equal to the largest value in the array
	- Lets assume, we have this array {10, 5, 7, 5, 9, 3, 20, 22, 17, 12, 19, 21}
		- largest element is 22 -> array size will be 22 + 1 = 23 so that I've index 22 available
		- All array indices initialized with some empty values e.g. 0, -1 etc...
## Process
- Scan through all the elements in the array one by one.
- Whatever the value is received, go to that index in the count array and Increment the value.
- Repeat the process till the whole array has been traversed.
- Now start moving elements from the count array to actual array
	- If index value in count array is greater than 0, then	copy the index number value to original array times the number of occurences

## Summary
- In the count array, we got the number of occurences of each number present in the given array
- Applicable to small range of number as its huge space consuming algorithm
- Takes O(n) for sorting and space complexity