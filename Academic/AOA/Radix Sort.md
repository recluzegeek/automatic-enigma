---
title: Radix Sort
updated: 2023-04-12 11:17:54Z
created: 2023-04-12 10:50:42Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---

## Radix Sort
- Similar to bucket/bin sort.
- Sort element based on digits
	- Sorting decimal integers -> 10 bins
	- Sorting octal integers -> 2 bins

## Assumptions and Requirments
- Follows FIFO principle while taking out elements from the bucket array
## Process
- Array to sort {237, 232, 90, 89, 235, 123}
	- in first iteration, we check the ones digit -> ten digit -> hundered -> thousand
		- by (arr[n] / 1) % 10 -> one's digit
		- by (arr[n] / 10) % 10 -> ten's digit
		- by (arr[n] / 100) % 10 -> hundered's digit
- After each iteration, take the element from the bucket array using FIFO principle
 ## Summary
 - Complexity depends upon the number of digits of the largest number
	 - 1000 -> 3 passes of radix
	 - 1000 -> 4 passes of radix
		 - Time has increased in comparison with count sort but limited number of bins
 - O(dn) -> O(n)
	 - Space complexity is limited and is of O(n)