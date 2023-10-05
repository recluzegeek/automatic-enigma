---
title: Minimum Cost Spanning Tree
updated: 2023-04-11 03:37:21Z
created: 2023-04-11 03:23:51Z
latitude: 55.85587000
longitude: -3.16418900
altitude: 0.0000
---

- Subgraph of graph having same number of vertices and edges are 1 less than vertices.
### Minimum Cost Spanning Tree
- **Prim's Algorithim**
	- Initially select the smallest one (weight) or select any random vertex and then find the **connected smallest one vertex.**
	- Selected vertex should not form any cycle and edge must be 1 less than the vertexes
	- Time Complexity is O(n^2) as it has to select the minimum edge from set of edges and there can be at max n^2 edges in case of connected graph(mesh).
	
 - **Kruskal's Algorithim**
	- Select the **smallest weighted vertex and continue finding smallest one from set of vertices** 
	- Selected vertex should not form any cycle and edge must be 1 less than the vertices.
	- **It can find minimum cost spanning tree for each component of the non-connected graph.**
	- Time Complexity is O(n^2) as it has to select the minimum edge from set of edges and there can be at max n^2 edges in case of connected graph(mesh).
	- Time can be reduced to **nlogn** by implementing kruskal using **min-heap** as it gives minimum value from the tree and we don't have to search for minimum value from the set of edge.