# LEETCODE-Graphs-1334
Let's walk through the `findTheCity` method of the `Solution` class step by step with a dry run example.

### Problem Overview:
The problem is to find the city with the smallest number of reachable cities within a given distance threshold. If there are multiple such cities, return the city with the greatest number.

### Example Inputs:
- `n`: number of cities
- `edges`: array of edges where each edge is defined as `[u, v, w]` indicating a road between city `u` and city `v` with distance `w`
- `distanceThreshold`: maximum distance allowed to consider a city reachable

### Dry Run Example:
Let’s consider a sample input:
- `n = 4`
- `edges = [[0, 1, 3], [1, 2, 1], [2, 3, 1], [0, 3, 10]]`
- `distanceThreshold = 4`

### Execution Steps:

1. **Initialize `dist` Matrix with Floyd-Warshall:**

    Initially, the distance matrix `dist` is set to `distanceThreshold + 1` for all pairs except the diagonal (self-distances), which are 0.
    ```plaintext
    dist = [
      [0, 5, 5, 5],
      [5, 0, 5, 5],
      [5, 5, 0, 5],
      [5, 5, 5, 0]
    ]
    ```

2. **Set the distances according to the edges:**

    Update the distances based on the edges provided.
    ```plaintext
    After processing edge [0, 1, 3]:
    dist = [
      [0, 3, 5, 5],
      [3, 0, 5, 5],
      [5, 5, 0, 5],
      [5, 5, 5, 0]
    ]
    After processing edge [1, 2, 1]:
    dist = [
      [0, 3, 5, 5],
      [3, 0, 1, 5],
      [5, 1, 0, 5],
      [5, 5, 5, 0]
    ]
    After processing edge [2, 3, 1]:
    dist = [
      [0, 3, 5, 5],
      [3, 0, 1, 5],
      [5, 1, 0, 1],
      [5, 5, 1, 0]
    ]
    After processing edge [0, 3, 10]:
    dist = [
      [0, 3, 5, 10],
      [3, 0, 1, 5],
      [5, 1, 0, 1],
      [10, 5, 1, 0]
    ]
    ```

3. **Apply Floyd-Warshall algorithm:**

    Update the distance matrix `dist` to find the shortest paths between all pairs of nodes.
    ```plaintext
    After applying Floyd-Warshall:
    dist = [
      [0, 3, 4, 5],
      [3, 0, 1, 2],
      [4, 1, 0, 1],
      [5, 2, 1, 0]
    ]
    ```

4. **Count reachable cities within the distance threshold:**

    Iterate through each city and count the number of cities reachable within the distance threshold.
    ```plaintext
    For city 0:
    Reachable cities count = 2 (city 0 and city 1)
    
    For city 1:
    Reachable cities count = 3 (city 0, city 1, and city 2)
    
    For city 2:
    Reachable cities count = 3 (city 1, city 2, and city 3)
    
    For city 3:
    Reachable cities count = 3 (city 1, city 2, and city 3)
    ```

5. **Determine the city with the smallest number of reachable cities:**

    The minimum number of reachable cities is 2, corresponding to city 0. If there were a tie, the city with the greater index would be chosen.

### Final Result:
The city with the smallest number of reachable cities within the distance threshold is city `0`.

### Code Explanation:

```java
class Solution {
  public int findTheCity(int n, int[][] edges, int distanceThreshold) {
    int ans = -1;
    int minCitiesCount = n;
    int[][] dist = floydWarshall(n, edges, distanceThreshold);

    for (int i = 0; i < n; ++i) {
      int citiesCount = 0;
      for (int j = 0; j < n; ++j)
        if (dist[i][j] <= distanceThreshold)
          ++citiesCount;
      if (citiesCount <= minCitiesCount) {
        ans = i;
        minCitiesCount = citiesCount;
      }
    }

    return ans;
  }

  private int[][] floydWarshall(int n, int[][] edges, int distanceThreshold) {
    int[][] dist = new int[n][n];
    Arrays.stream(dist).forEach(A -> Arrays.fill(A, distanceThreshold + 1));

    for (int i = 0; i < n; ++i)
      dist[i][i] = 0;

    for (int[] edge : edges) {
      final int u = edge[0];
      final int v = edge[1];
      final int w = edge[2];
      dist[u][v] = w;
      dist[v][u] = w;
    }

    for (int k = 0; k < n; ++k)
      for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
          dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);

    return dist;
  }
}
```

This code correctly implements the solution to find the city with the smallest number of reachable cities within a given distance threshold using the Floyd-Warshall algorithm to compute shortest paths.
