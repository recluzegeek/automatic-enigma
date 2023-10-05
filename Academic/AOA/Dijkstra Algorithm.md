---
title: Dijkstra Algorithm
updated: 2023-04-11 09:46:50Z
created: 2023-04-11 09:41:24Z
---

## Dijkstra Algorithm
- Single source shortest path for positive weighted acyclic graphs
### Steps
- **Idea is to select minimum weight vertex whenever possible from the set of vertices that are reachable.**
- Select start node and marks its distance as 0 and remaining nodes distances initially set to infinity.
- Select the minimum weight vertex and perform relaxation,
	-
	 ```bash
			if (d[u] + c(u,v) < d[v])
					d[v] = d[u] + c(u,v)
	 ```