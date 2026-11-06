# Shortest Path Algorithms in Java

## 1. Introduction to Shortest Path

A **shortest path** is the path between two vertices that has the minimum total cost or distance.

The cost of a path depends on the weights assigned to its edges.

Consider this weighted graph:

```text
       4
   0 ------ 1
   |        |
  2|        |1
   |        |
   2 ------ 3
       3
```

There are two possible paths from vertex `0` to vertex `3`:

**Path 1:**

```text
0 -> 1 -> 3

Cost = 4 + 1 = 5
```

**Path 2:**

```text
0 -> 2 -> 3

Cost = 2 + 3 = 5
```

Both paths have the same minimum cost.

Shortest path algorithms help us find the minimum distance between vertices efficiently.

## 2. Types of Shortest Path Problems

Shortest path problems can be divided into different categories.

| Type | Description |
|---|---|
| Single-source shortest path | Find shortest distances from one source to all other vertices |
| Single-destination shortest path | Find shortest distances from all vertices to one destination |
| Single-pair shortest path | Find the shortest path between two specific vertices |
| All-pairs shortest path | Find shortest distances between every pair of vertices |

Different algorithms are suitable for different graph types.

## 3. Which Algorithm Should You Use?

Choosing the correct algorithm is important.

| Algorithm | Graph Type | Negative Weights | Negative Cycles |
|---|---|---|---|
| BFS | Unweighted graph or equal edge weights | Not applicable | Not applicable |
| Dijkstra | Weighted graph with non-negative edges | No | Not supported |
| Bellman-Ford | Weighted directed graph | Yes | Can detect reachable negative cycles |
| Floyd-Warshall | All-pairs shortest paths | Yes | Can detect negative cycles |

### Quick selection guide

- Unweighted graph: Use BFS.
- Weighted graph with non-negative edges: Use Dijkstra.
- Graph with negative edge weights: Use Bellman-Ford.
- Shortest paths between all pairs of vertices: Use Floyd-Warshall.

**Important:** Dijkstra's algorithm should not be used when negative edge weights are present.

---

# 4. Shortest Path Using BFS

BFS is the simplest shortest path algorithm for an unweighted graph.

It explores vertices level by level.

Because BFS visits vertices in increasing order of the number of edges from the source, the first time a vertex is reached gives its minimum number of edges from the source.

### Example

```text
       0
      / \
     1   2
    /     \
   3       4
            \
             5
```

Starting from vertex `0`:

```text
Distance from 0:

0 -> 0
1 -> 1
2 -> 1
3 -> 2
4 -> 2
5 -> 3
```

### Algorithm

1. Initialize all distances to infinity.
2. Set the source distance to `0`.
3. Add the source to a queue.
4. Remove a vertex from the queue.
5. Visit each unvisited neighbor.
6. Update its distance and add it to the queue.
7. Continue until the queue is empty.

### Java Implementation

```java
import java.util.*;

public class BFSShortestPath {

    static int[] shortestPath(
            List<List<Integer>> graph,
            int vertices,
            int source) {

        int[] distance = new int[vertices];

        Arrays.fill(distance, -1);

        Queue<Integer> queue = new LinkedList<>();

        distance[source] = 0;

        queue.offer(source);

        while (!queue.isEmpty()) {

            int node = queue.poll();

            for (int neighbor : graph.get(node)) {

                if (distance[neighbor] == -1) {

                    distance[neighbor] =
                            distance[node] + 1;

                    queue.offer(neighbor);
                }
            }
        }

        return distance;
    }

    public static void main(String[] args) {

        int vertices = 6;

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        graph.get(0).add(1);
        graph.get(0).add(2);

        graph.get(1).add(0);
        graph.get(1).add(3);

        graph.get(2).add(0);
        graph.get(2).add(4);

        graph.get(3).add(1);

        graph.get(4).add(2);
        graph.get(4).add(5);

        graph.get(5).add(4);

        int[] distance =
                shortestPath(graph, vertices, 0);

        System.out.println(Arrays.toString(distance));
    }
}
```

**Output:**

```text
[0, 1, 1, 2, 2, 3]
```

Here, `-1` represents an unreachable vertex.

### Time and Space Complexity

- Time Complexity: `O(V + E)`
- Auxiliary Space Complexity: `O(V)`

Where:

- `V` = Number of vertices.
- `E` = Number of edges.

---

# 5. Dijkstra's Algorithm

Dijkstra's algorithm finds the shortest distance from a source vertex to every other vertex in a weighted graph with non-negative edge weights.

It uses a **Priority Queue (Min Heap)** to process the vertex with the smallest known distance first.

### Example

Consider this weighted graph:

```text
        4
    0 ------ 1
    |       /|
   2|     1/ |5
    |    /   |
    2 ------ 3
        3
```

Edges:

```text
0 -> 1 = 4
0 -> 2 = 2
2 -> 1 = 1
1 -> 3 = 5
2 -> 3 = 3
```

Starting vertex: `0`

Possible shortest distances:

| Vertex | Shortest Distance |
|---|---:|
| 0 | 0 |
| 1 | 3 |
| 2 | 2 |
| 3 | 5 |

### How Dijkstra Works

1. Initialize all distances to infinity.
2. Set the source distance to `0`.
3. Insert the source into a min priority queue.
4. Remove the vertex with the smallest distance.
5. Check all its adjacent vertices.
6. Calculate the new possible distance.
7. If the new distance is smaller, update the distance.
8. Insert the updated distance into the priority queue.
9. Continue until the queue is empty.

### Important Concept: Relaxation

Relaxation means checking whether a path through the current vertex provides a shorter distance to a neighbor.

Formula:

```text
newDistance = distance[current] + edgeWeight
```

If:

```text
newDistance < distance[neighbor]
```

Update:

```text
distance[neighbor] = newDistance
```

### Java Implementation

```java
import java.util.*;

public class Dijkstra {

    static class Pair {

        int node;
        int distance;

        Pair(int node, int distance) {
            this.node = node;
            this.distance = distance;
        }
    }

    static int[] dijkstra(
            List<List<Pair>> graph,
            int vertices,
            int source) {

        int[] distance = new int[vertices];

        Arrays.fill(distance, Integer.MAX_VALUE);

        PriorityQueue<Pair> pq =
                new PriorityQueue<>(
                        Comparator.comparingInt(p -> p.distance)
                );

        distance[source] = 0;

        pq.offer(new Pair(source, 0));

        while (!pq.isEmpty()) {

            Pair current = pq.poll();

            int node = current.node;
            int currentDistance = current.distance;

            // Ignore outdated priority queue entries
            if (currentDistance > distance[node]) {
                continue;
            }

            for (Pair neighbor : graph.get(node)) {

                int nextNode = neighbor.node;
                int weight = neighbor.distance;

                int newDistance =
                        currentDistance + weight;

                if (newDistance < distance[nextNode]) {

                    distance[nextNode] = newDistance;

                    pq.offer(
                            new Pair(nextNode, newDistance)
                    );
                }
            }
        }

        return distance;
    }

    public static void main(String[] args) {

        int vertices = 4;

        List<List<Pair>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        // Undirected weighted edges

        graph.get(0).add(new Pair(1, 4));
        graph.get(1).add(new Pair(0, 4));

        graph.get(0).add(new Pair(2, 2));
        graph.get(2).add(new Pair(0, 2));

        graph.get(2).add(new Pair(1, 1));
        graph.get(1).add(new Pair(2, 1));

        graph.get(1).add(new Pair(3, 5));
        graph.get(3).add(new Pair(1, 5));

        graph.get(2).add(new Pair(3, 3));
        graph.get(3).add(new Pair(2, 3));

        int[] result = dijkstra(graph, vertices, 0);

        System.out.println(Arrays.toString(result));
    }
}
```

**Output:**

```text
[0, 3, 2, 5]
```

### Dry Run of Dijkstra

Starting vertex: `0`

Initial distances:

```text
0 = 0
1 = INF
2 = INF
3 = INF
```

**Step 1: Process vertex 0**

Neighbors:

- Vertex 1: distance = `0 + 4 = 4`
- Vertex 2: distance = `0 + 2 = 2`

Updated distances:

```text
0 = 0
1 = 4
2 = 2
3 = INF
```

**Step 2: Process vertex 2**

Vertex `2` has the smallest distance (`2`).

Neighbors:

- Vertex 1: distance = `2 + 1 = 3`
- Vertex 3: distance = `2 + 3 = 5`

Distance to vertex `1` improves from `4` to `3`.

Updated distances:

```text
0 = 0
1 = 3
2 = 2
3 = 5
```

**Step 3: Process vertex 1**

Possible distance to vertex `3`:

```text
3 + 5 = 8
```

Current distance is `5`, so no update is needed.

**Step 4: Process vertex 3**

No shorter distance is found.

Final distances:

```text
[0, 3, 2, 5]
```

### Why Do We Check Outdated Entries?

In Java's `PriorityQueue`, updating an existing element's priority is not directly supported.

Instead, we insert a new entry when a shorter distance is found.

Older entries can remain in the queue.

This condition skips them:

```java
if (currentDistance > distance[node]) {
    continue;
}
```

### Dijkstra Complexity

Using an adjacency list and binary heap:

- Time Complexity: `O((V + E) log V)`
- Auxiliary Space Complexity: `O(V + E)` including the priority queue and distance data in the worst case.

The graph representation itself requires `O(V + E)` space.

### Important Dijkstra Rules

1. Edge weights must be non-negative.
2. Use a min priority queue.
3. Always relax edges when a shorter distance is found.
4. Ignore outdated queue entries.
5. Use a sufficiently large distance value.
6. Be careful about integer overflow when adding distances.

For graphs with potentially large weights, use `long[]` instead of `int[]`.

---

# 6. Bellman-Ford Algorithm

Bellman-Ford is another shortest path algorithm.

Unlike Dijkstra, Bellman-Ford supports negative edge weights.

It can also detect negative-weight cycles reachable from the source.

### What Is a Negative Edge Weight?

A negative edge has a weight less than zero.

Example:

```text
0 -----> 1
    5

1 -----> 2
   -3

0 -----> 2
    4
```

The path `0 -> 1 -> 2` has total cost:

```text
5 + (-3) = 2
```

This is shorter than the direct path from `0` to `2`, which costs `4`.

### What Is a Negative Cycle?

A negative cycle is a cycle whose total edge weight is negative.

Example:

```text
0 -> 1 -> 2 -> 0

Weights:

0 -> 1 = 2
1 -> 2 = -5
2 -> 0 = 1

Total = 2 - 5 + 1 = -2
```

By repeatedly following this cycle, the path cost can keep decreasing.

Therefore, a finite shortest path does not exist for vertices reachable through that negative cycle.

### Main Idea of Bellman-Ford

Bellman-Ford relaxes every edge repeatedly.

For a graph with `V` vertices, it performs at most:

```text
V - 1 iterations
```

Why `V - 1`?

A shortest simple path can contain at most `V - 1` edges because it cannot repeat a vertex.

After `V - 1` iterations, all shortest distances should be finalized if no reachable negative cycle exists.

### Edge Representation in Java

```java
static class Edge {

    int source;
    int destination;
    int weight;

    Edge(int source, int destination, int weight) {

        this.source = source;
        this.destination = destination;
        this.weight = weight;
    }
}
```

### Java Implementation

```java
import java.util.*;

public class BellmanFord {

    static class Edge {

        int source;
        int destination;
        int weight;

        Edge(int source, int destination, int weight) {

            this.source = source;
            this.destination = destination;
            this.weight = weight;
        }
    }

    static void bellmanFord(
            int vertices,
            List<Edge> edges,
            int source) {

        int[] distance = new int[vertices];

        Arrays.fill(distance, Integer.MAX_VALUE);

        distance[source] = 0;

        // Relax all edges V - 1 times
        for (int i = 1; i < vertices; i++) {

            boolean updated = false;

            for (Edge edge : edges) {

                if (distance[edge.source] != Integer.MAX_VALUE) {

                    int newDistance =
                            distance[edge.source] + edge.weight;

                    if (newDistance < distance[edge.destination]) {

                        distance[edge.destination] = newDistance;

                        updated = true;
                    }
                }
            }

            // Stop early if no distance changed
            if (!updated) {
                break;
            }
        }

        // Check for a reachable negative cycle
        for (Edge edge : edges) {

            if (distance[edge.source] != Integer.MAX_VALUE &&
                distance[edge.source] + edge.weight
                        < distance[edge.destination]) {

                System.out.println(
                        "Negative weight cycle detected"
                );

                return;
            }
        }

        System.out.println(
                "Shortest distances: " +
                Arrays.toString(distance)
        );
    }

    public static void main(String[] args) {

        int vertices = 5;

        List<Edge> edges = new ArrayList<>();

        edges.add(new Edge(0, 1, 6));
        edges.add(new Edge(0, 2, 7));

        edges.add(new Edge(1, 2, 8));
        edges.add(new Edge(1, 3, 5));
        edges.add(new Edge(1, 4, -4));

        edges.add(new Edge(2, 3, -3));
        edges.add(new Edge(2, 4, 9));

        edges.add(new Edge(3, 1, -2));

        edges.add(new Edge(4, 0, 2));
        edges.add(new Edge(4, 3, 7));

        bellmanFord(vertices, edges, 0);
    }
}
```

**Output:**

```text
Shortest distances: [0, 2, 7, 4, -2]
```

### Bellman-Ford Complexity

| Metric | Complexity |
|---|---|
| Time | `O(V × E)` |
| Auxiliary Space | `O(V)` |

The edge list requires an additional `O(E)` space.

### When Should You Use Bellman-Ford?

Use Bellman-Ford when:

- The graph contains negative edge weights.
- You need to detect negative-weight cycles reachable from a source.
- The graph is relatively small enough for its `O(V × E)` time complexity.

---

# 7. Floyd-Warshall Algorithm

Floyd-Warshall finds the shortest distance between **every pair of vertices**.

Unlike Dijkstra and Bellman-Ford, which are single-source algorithms, Floyd-Warshall is an all-pairs shortest path algorithm.

It uses Dynamic Programming.

### Main Idea

For every pair of vertices `(i, j)`, check whether traveling through an intermediate vertex `k` produces a shorter path.

Formula:

```text
dist[i][j] = min(
    dist[i][j],
    dist[i][k] + dist[k][j]
)
```

Here:

- `i` = Starting vertex.
- `j` = Destination vertex.
- `k` = Intermediate vertex.

### Example

Initial distance matrix:

```text
     0   1   2
0    0   5  10
1   INF  0   3
2   INF INF  0
```

There is a path from `0` to `2` directly with cost `10`.

But the path through vertex `1` costs:

```text
0 -> 1 -> 2

5 + 3 = 8
```

Therefore, the shortest distance from `0` to `2` becomes `8`.

### Java Implementation

```java
import java.util.*;

public class FloydWarshall {

    static final int INF = 1_000_000_000;

    static void floydWarshall(int[][] distance, int vertices) {

        // Consider each vertex as an intermediate vertex
        for (int k = 0; k < vertices; k++) {

            for (int i = 0; i < vertices; i++) {

                for (int j = 0; j < vertices; j++) {

                    if (distance[i][k] != INF &&
                        distance[k][j] != INF) {

                        distance[i][j] = Math.min(
                                distance[i][j],
                                distance[i][k] + distance[k][j]
                        );
                    }
                }
            }
        }

        // Print shortest distance matrix
        for (int i = 0; i < vertices; i++) {

            for (int j = 0; j < vertices; j++) {

                if (distance[i][j] == INF) {
                    System.out.print("INF ");
                } else {
                    System.out.print(distance[i][j] + " ");
                }
            }

            System.out.println();
        }
    }

    public static void main(String[] args) {

        int[][] distance = {
            {0, 5, 10},
            {INF, 0, 3},
            {INF, INF, 0}
        };

        floydWarshall(distance, 3);
    }
}
```

**Output:**

```text
0 5 8
INF 0 3
INF INF 0
```

### Floyd-Warshall Complexity

| Metric | Complexity |
|---|---|
| Time | `O(V³)` |
| Auxiliary Space | `O(1)` excluding the input matrix |
| Input matrix space | `O(V²)` |

### Negative Cycle Detection

After running Floyd-Warshall, check the diagonal of the distance matrix.

If:

```text
distance[i][i] < 0
```

then a negative cycle exists.

This is because the shortest path from a vertex back to itself should normally be zero, unless a negative cycle is reachable.

**Important:** Floyd-Warshall supports negative edges, but shortest paths are not well-defined for vertex pairs that can travel through a negative cycle.

---

# 8. Reconstructing the Shortest Path

Sometimes knowing the minimum distance is not enough.

We also need the actual sequence of vertices in the shortest path.

For example:

```text
Shortest distance = 5

Shortest path = 0 -> 2 -> 3
```

We can store a `parent` array while running Dijkstra.

Whenever a shorter path to a neighbor is found:

```java
parent[neighbor] = currentNode;
```

### Java Implementation: Dijkstra with Path Reconstruction

```java
import java.util.*;

public class DijkstraPath {

    static class Pair {

        int node;
        long distance;

        Pair(int node, long distance) {
            this.node = node;
            this.distance = distance;
        }
    }

    static void shortestPath(
            List<List<Pair>> graph,
            int vertices,
            int source,
            int destination) {

        long[] distance = new long[vertices];

        int[] parent = new int[vertices];

        Arrays.fill(distance, Long.MAX_VALUE);
        Arrays.fill(parent, -1);

        PriorityQueue<Pair> pq =
                new PriorityQueue<>(
                        Comparator.comparingLong(p -> p.distance)
                );

        distance[source] = 0;

        pq.offer(new Pair(source, 0));

        while (!pq.isEmpty()) {

            Pair current = pq.poll();

            int node = current.node;

            if (current.distance > distance[node]) {
                continue;
            }

            for (Pair neighbor : graph.get(node)) {

                int nextNode = neighbor.node;

                long newDistance =
                        distance[node] + neighbor.distance;

                if (newDistance < distance[nextNode]) {

                    distance[nextNode] = newDistance;

                    parent[nextNode] = node;

                    pq.offer(
                            new Pair(nextNode, newDistance)
                    );
                }
            }
        }

        if (distance[destination] == Long.MAX_VALUE) {

            System.out.println("No path exists");

            return;
        }

        List<Integer> path = new ArrayList<>();

        int current = destination;

        while (current != -1) {

            path.add(current);

            current = parent[current];
        }

        Collections.reverse(path);

        System.out.println(
                "Shortest Distance: " + distance[destination]
        );

        System.out.println(
                "Shortest Path: " + path
        );
    }
}
```

### How Parent Tracking Works

Suppose the shortest path is:

```text
0 -> 2 -> 3
```

The parent array contains:

```text
parent[0] = -1
parent[1] = ...
parent[2] = 0
parent[3] = 2
```

Starting from destination `3`:

```text
3 -> 2 -> 0
```

Reverse the list:

```text
0 -> 2 -> 3
```

This gives the shortest path.

---

# 9. Comparison of Shortest Path Algorithms

| Algorithm | Main Data Structure | Time Complexity | Best Use |
|---|---|---|---|
| BFS | Queue | `O(V + E)` | Unweighted graphs |
| Dijkstra | Min Priority Queue | `O((V + E) log V)` | Non-negative weights |
| Bellman-Ford | Edge list | `O(V × E)` | Negative edge weights |
| Floyd-Warshall | Distance matrix | `O(V³)` | All-pairs shortest paths |

### Important Differences

**BFS vs Dijkstra**

- BFS considers the number of edges.
- Dijkstra considers the total weight of edges.
- BFS is suitable for unweighted graphs.
- Dijkstra is suitable for non-negative weighted graphs.

**Dijkstra vs Bellman-Ford**

- Dijkstra is generally faster.
- Bellman-Ford supports negative weights.
- Bellman-Ford can detect reachable negative cycles.
- Dijkstra does not correctly handle negative edge weights.

**Bellman-Ford vs Floyd-Warshall**

- Bellman-Ford solves single-source shortest paths.
- Floyd-Warshall solves all-pairs shortest paths.
- Bellman-Ford uses an edge list.
- Floyd-Warshall uses a distance matrix.

---

# 10. Common Shortest Path Mistakes

1. **Using Dijkstra with negative edge weights**

   Dijkstra requires non-negative edge weights.

2. **Using BFS on a weighted graph**

   BFS does not account for different edge weights. It only minimizes the number of edges.

3. **Forgetting to initialize distances**

   Initialize all distances to infinity and the source to zero.

4. **Not checking outdated priority queue entries**

   This can lead to unnecessary processing in Dijkstra.

5. **Integer overflow**

   Adding a weight to an infinity value can overflow. Check reachability first and consider using `long`.

6. **Forgetting disconnected vertices**

   Their distance from the source remains infinity or a chosen unreachable marker.

7. **Incorrect negative cycle detection**

   Bellman-Ford detects negative cycles reachable from the selected source, not necessarily every negative cycle in the graph.

8. **Incorrect Floyd-Warshall initialization**

   Set diagonal distances to zero and unreachable pairs to infinity.

9. **Forgetting directed versus undirected edges**

   For an undirected graph, add both directions. For a directed graph, add only the outgoing edge.

---

# 11. Important Interview Questions

### Beginner

1. What is a shortest path in a graph?
2. What is the difference between BFS and Dijkstra?
3. Why does BFS find the shortest path in an unweighted graph?
4. What is edge relaxation?
5. What is a weighted graph?
6. What is a negative edge weight?

### Intermediate

1. Explain Dijkstra's algorithm with an example.
2. Why does Dijkstra fail with negative edge weights?
3. Explain Bellman-Ford.
4. How does Bellman-Ford detect a negative cycle?
5. What is the difference between single-source and all-pairs shortest paths?
6. How do you reconstruct a shortest path using a parent array?
7. What is the time complexity of Dijkstra using a priority queue?

### Advanced

1. Explain Floyd-Warshall using Dynamic Programming.
2. How can Floyd-Warshall detect negative cycles?
3. When would you choose Bellman-Ford over Dijkstra?
4. How do you find the shortest path in a grid?
5. What is the difference between a shortest path and a shortest simple path?
6. How can shortest path algorithms be adapted for large graphs?

---

# 12. Practice Problems

Try solving these problems in Java.

### Easy

1. [Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph/)
2. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)

### Medium

1. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
2. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)
3. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
4. [Path With Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)
5. [Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

### Advanced

1. [Shortest Path to Get All Keys](https://leetcode.com/problems/shortest-path-to-get-all-keys/)
2. [Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

---

# 13. Quick Revision

| Algorithm | Remember |
|---|---|
| BFS | Shortest path in unweighted graphs |
| Dijkstra | Non-negative weighted graphs |
| Bellman-Ford | Supports negative weights |
| Floyd-Warshall | All-pairs shortest paths |
| Relaxation | Update a distance if a shorter path is found |
| Priority Queue | Used by Dijkstra |
| Negative cycle | A cycle with a negative total weight |
| Parent array | Reconstructs the shortest path |

### Complexity Cheat Sheet

```text
BFS:
Time  = O(V + E)
Space = O(V)

Dijkstra (Binary Heap):
Time  = O((V + E) log V)
Space = O(V + E) including the queue and graph

Bellman-Ford:
Time  = O(V * E)
Space = O(V) auxiliary, plus O(E) for the edge list

Floyd-Warshall:
Time  = O(V³)
Space = O(V²) for the distance matrix
```

---

# 14. Learning Checklist

- [ ] Understand shortest path problems.
- [ ] Implement BFS shortest path in Java.
- [ ] Understand edge relaxation.
- [ ] Implement Dijkstra using `PriorityQueue`.
- [ ] Understand Dijkstra's dry run.
- [ ] Understand why Dijkstra does not support negative weights.
- [ ] Implement Bellman-Ford.
- [ ] Understand negative cycle detection.
- [ ] Implement Floyd-Warshall.
- [ ] Reconstruct a shortest path using a parent array.
- [ ] Understand the complexity of each algorithm.
- [ ] Solve at least three shortest path problems on LeetCode.

---

## Conclusion

Shortest path algorithms are essential for solving graph problems involving distance, cost, and connectivity.

Remember these four rules:

1. **BFS** — Unweighted graph.
2. **Dijkstra** — Weighted graph with non-negative edges.
3. **Bellman-Ford** — Weighted graph with negative edges.
4. **Floyd-Warshall** — Shortest paths between all pairs of vertices.

Master these algorithms because they are frequently asked in coding interviews and are used in routing, navigation, network optimization, and many real-world applications.