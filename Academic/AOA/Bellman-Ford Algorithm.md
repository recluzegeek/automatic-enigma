---
title: Bellman-Ford Algorithm
updated: 2023-04-11 06:19:32Z
created: 2023-04-11 04:42:55Z
latitude: 55.85587000
longitude: -3.16418900
altitude: 0.0000
---

## Bellman-Ford Algorithm
- Follows dynamic programming - you try all possible solutions and pick up the best one.
- Single source shortest path and can handle negative edges not negative cycles.
- Go on relaxing edges for n-1 times where n is the num of vertices.
	- Longest distance from the source vertex to destination vertex, can be at max | v |-1, covers all the possible paths.
	- Can be used to detect negative edge cycles by running it for n-times instead of n-1 times.
## Steps
- Make edges list and go on relaxing them for | v | - 1 times.
- Relaxing all edges | E | -> n^2
	- for how many times: 
		- | v | - 1 times -> n
			- so O(n^2 * n) -> O(n^3)
			- Minimum time O(n^2) and max O(n^3)