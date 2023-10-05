---
title: Floyd-Warshall Algorithm
updated: 2023-04-11 06:06:40Z
created: 2023-04-11 03:41:25Z
latitude: 55.85587000
longitude: -3.16418900
altitude: 0.0000
---

## Floyd Warshall
- Follows Dynamic Programming approach,  we should reutilize the results, not recalculate them.
- All pairs shortest path for directed and connected graphs
- Find a shortest path between every pair in O(n^3)
```

	n = no of vertices
	A = matrix of dimension n*n
	for k = 1 to n
  	  for i = 1 to n
   	     for j = 1 to n
   	         A^k[i, j] = min (A^k-1[i, j], A^k-1[i, k] + A^k-1[k, j])
	return A

```

## Steps
- Make distance matrix from the image and perform relaxation one-by-one
	- means first take a vertex - 1 as source vertex and perform relaxation so forth...
		- **Use shortcut** 
			- Set the diagonal cells of the distance matrix to 0.
			- Copy corresponding row and column values from the previous matrix.
			- Update iteration row and column cells containing infinity with values from the previous matrix either in the row or column.
			- Calculate remaining cell values manually by adding the nearest or corresponding rows and columns.
			- Repeat the iterative process until all shortest paths have been found.