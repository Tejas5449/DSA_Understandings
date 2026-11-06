# Breadth First Search (BFS) in Java

## 1. Introduction to BFS

Breadth First Search (BFS) is a graph traversal algorithm used to visit vertices level by level.

BFS starts from a source vertex and visits all its immediate neighbors before moving to the next level.

It uses a **Queue** data structure.

### Example

Consider this graph:

```text
          0
        /   \
       1     2
      / \     \
     3   4     5
```

Starting from vertex `0`, BFS visits:

```text
0 → 1 → 2 → 3 → 4 → 5
```

### Why do we use BFS?

- Traverse a graph.
- Find the shortest path in an unweighted graph.
- Find connected components.
- Solve level-based problems.
- Find the minimum number of steps to reach a destination.
- Solve grid-based problems.

---

## 2. Why Does BFS Use a Queue?

A Queue follows the FIFO (First In, First Out) principle.

The vertex discovered first is processed first.

This ensures that BFS visits vertices level by level.

Example:

```text
          0
        /   \
       1     2
      / \     \
     3   4     5
```

Starting from `0`:

- Level 0: `0`
- Level 1: `1, 2`
- Level 2: `3, 4, 5`

BFS processes all vertices at one level before moving to the next.

---

## 3. BFS Algorithm

The basic BFS algorithm follows these steps:

1. Create a Queue.
2. Create a visited array.
3. Mark the starting vertex as visited.
4. Add the starting vertex to the Queue.
5. While the Queue is not empty:
   - Remove the front vertex.
   - Process the vertex.
   - Visit all its unvisited neighbors.
   - Mark each neighbor as visited.
   - Add each neighbor to the Queue.

### Pseudocode

```text
BFS(graph, start):

    create visited array
    create queue

    visited[start] = true
    queue.add(start)

    while queue is not empty:

        current = queue.remove()

        process(current)

        for each neighbor of current:

            if neighbor is not visited:

                visited[neighbor] = true

                queue.add(neighbor)
```

**Important:** Mark a vertex as visited when adding it to the Queue, not when removing it. This prevents the same vertex from being inserted multiple times.

---

## 4. Graph Representation Using Adjacency List

We will use an adjacency list to implement BFS.

Consider:

```text
          0
        /   \
       1     2
      / \     \
     3   4     5
```

Adjacency list:

```text
0 → 1, 2
1 → 0, 3, 4
2 → 0, 5
3 → 1
4 → 1
5 → 2
```

### Java implementation

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        int vertices = 6;

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        addEdge(graph, 0, 1);
        addEdge(graph, 0, 2);
        addEdge(graph, 1, 3);
        addEdge(graph, 1, 4);
        addEdge(graph, 2, 5);

        System.out.println(graph);
    }

    static void addEdge(
            List<List<Integer>> graph,
            int u,
            int v) {

        graph.get(u).add(v);
        graph.get(v).add(u);
    }
}
```

This creates an undirected graph.

---

## 5. BFS Traversal in Java

### Java solution

```java
import java.util.*;

public class Main {

    static void bfs(
            List<List<Integer>> graph,
            int start,
            int vertices) {

        boolean[] visited = new boolean[vertices];

        Queue<Integer> queue = new LinkedList<>();

        visited[start] = true;

        queue.offer(start);

        while (!queue.isEmpty()) {

            int current = queue.poll();

            System.out.print(current + " ");

            for (int neighbor : graph.get(current)) {

                if (!visited[neighbor]) {

                    visited[neighbor] = true;

                    queue.offer(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {

        int vertices = 6;

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        addEdge(graph, 0, 1);
        addEdge(graph, 0, 2);
        addEdge(graph, 1, 3);
        addEdge(graph, 1, 4);
        addEdge(graph, 2, 5);

        System.out.print("BFS Traversal: ");

        bfs(graph, 0, vertices);
    }

    static void addEdge(
            List<List<Integer>> graph,
            int u,
            int v) {

        graph.get(u).add(v);
        graph.get(v).add(u);
    }
}
```

Output:

```text
BFS Traversal: 0 1 2 3 4 5
```

---

## 6. BFS Dry Run

Consider this graph:

```text
          0
        /   \
       1     2
      / \     \
     3   4     5
```

Starting vertex = `0`.

Initially:

```text
Visited = [false, false, false, false, false, false]
Queue = []
```

### Step-by-step execution

| Step | Processed vertex | Queue after processing | Visited vertices |
|---|---:|---|---|
| 1 | 0 | [1, 2] | 0, 1, 2 |
| 2 | 1 | [2, 3, 4] | 0, 1, 2, 3, 4 |
| 3 | 2 | [3, 4, 5] | 0, 1, 2, 3, 4, 5 |
| 4 | 3 | [4, 5] | All |
| 5 | 4 | [5] | All |
| 6 | 5 | [] | All |

Final BFS traversal:

```text
0 1 2 3 4 5
```

### Understanding the Queue

Initially:

```text
[0]
```

Remove `0`, add its neighbors:

```text
[1, 2]
```

Remove `1`, add its unvisited neighbors:

```text
[2, 3, 4]
```

Remove `2`, add its unvisited neighbor:

```text
[3, 4, 5]
```

Continue until the Queue becomes empty.

---

## 7. BFS for Disconnected Graphs

A graph may contain multiple disconnected components.

Example:

```text
       0 ---- 1 ---- 2

       3 ---- 4

       5 ---- 6
```

If we start BFS from vertex `0`, only vertices `0`, `1`, and `2` will be visited.

To traverse the entire graph, we must start BFS from every unvisited vertex.

### Java implementation

```java
import java.util.*;

public class Main {

    static void bfsDisconnected(
            List<List<Integer>> graph,
            int vertices) {

        boolean[] visited = new boolean[vertices];

        for (int i = 0; i < vertices; i++) {

            if (!visited[i]) {

                bfs(graph, i, visited);
            }
        }
    }

    static void bfs(
            List<List<Integer>> graph,
            int start,
            boolean[] visited) {

        Queue<Integer> queue = new LinkedList<>();

        visited[start] = true;

        queue.offer(start);

        while (!queue.isEmpty()) {

            int current = queue.poll();

            System.out.print(current + " ");

            for (int neighbor : graph.get(current)) {

                if (!visited[neighbor]) {

                    visited[neighbor] = true;

                    queue.offer(neighbor);
                }
            }
        }
    }
}
```

Possible output:

```text
0 1 2 3 4 5 6
```

The exact order depends on the adjacency-list ordering.

### Important concept

The outer loop ensures that every connected component is visited.

**Time Complexity:** O(V + E).

**Space Complexity:** O(V) for the visited array and Queue.

---

## 8. BFS to Find the Shortest Path in an Unweighted Graph

One of the most important applications of BFS is finding the shortest path in an unweighted graph.

### Problem

Given an unweighted graph, find the minimum number of edges required to reach a destination from a source.

Example:

```text
          0
        /   \
       1     2
      /       \
     3         5
      \       /
        \   /
          4
```

Find the shortest distance from `0` to `4`.

Possible paths:

```text
0 → 1 → 3 → 4

0 → 2 → 5 → 4
```

Both paths have 3 edges.

Output:

```text
3
```

### Approach

Maintain a distance array.

- Initialize all distances to `-1`.
- Set the source distance to `0`.
- Use BFS to explore neighbors.
- When visiting an unvisited neighbor, set its distance to the current distance plus one.

### Java solution

```java
import java.util.*;

public class Main {

    static int shortestDistance(
            List<List<Integer>> graph,
            int source,
            int destination,
            int vertices) {

        int[] distance = new int[vertices];

        Arrays.fill(distance, -1);

        Queue<Integer> queue = new LinkedList<>();

        distance[source] = 0;

        queue.offer(source);

        while (!queue.isEmpty()) {

            int current = queue.poll();

            if (current == destination) {
                return distance[current];
            }

            for (int neighbor : graph.get(current)) {

                if (distance[neighbor] == -1) {

                    distance[neighbor] =
                            distance[current] + 1;

                    queue.offer(neighbor);
                }
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int vertices = 6;

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        addEdge(graph, 0, 1);
        addEdge(graph, 0, 2);
        addEdge(graph, 1, 3);
        addEdge(graph, 2, 4);
        addEdge(graph, 3, 5);
        addEdge(graph, 4, 5);

        System.out.println(
            shortestDistance(graph, 0, 5, vertices)
        );
    }

    static void addEdge(
            List<List<Integer>> graph,
            int u,
            int v) {

        graph.get(u).add(v);
        graph.get(v).add(u);
    }
}
```

Output:

```text
3
```

### Why does BFS find the shortest path?

BFS visits vertices in increasing order of distance from the source.

It processes:
- Distance 0 first.
- Then distance 1.
- Then distance 2.
- Then distance 3.

Therefore, the first time we reach a destination, we have found a path with the minimum number of edges.

**Important:** This shortest-path guarantee applies to unweighted graphs or graphs where every edge has the same weight. Ordinary BFS does not guarantee shortest weighted paths when edge weights differ.

---

## 9. BFS to Print the Shortest Path

Sometimes we need the actual path, not just the distance.

For this, maintain a `parent` array.

Whenever a vertex is discovered, store the vertex from which it was reached.

### Java solution

```java
import java.util.*;

public class Main {

    static List<Integer> shortestPath(
            List<List<Integer>> graph,
            int source,
            int destination,
            int vertices) {

        int[] parent = new int[vertices];

        Arrays.fill(parent, -1);

        boolean[] visited = new boolean[vertices];

        Queue<Integer> queue = new LinkedList<>();

        visited[source] = true;

        queue.offer(source);

        while (!queue.isEmpty()) {

            int current = queue.poll();

            if (current == destination) {
                break;
            }

            for (int neighbor : graph.get(current)) {

                if (!visited[neighbor]) {

                    visited[neighbor] = true;

                    parent[neighbor] = current;

                    queue.offer(neighbor);
                }
            }
        }

        List<Integer> path = new ArrayList<>();

        if (!visited[destination]) {
            return path;
        }

        int current = destination;

        while (current != -1) {

            path.add(current);

            current = parent[current];
        }

        Collections.reverse(path);

        return path;
    }

    public static void main(String[] args) {

        int vertices = 6;

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        addEdge(graph, 0, 1);
        addEdge(graph, 0, 2);
        addEdge(graph, 1, 3);
        addEdge(graph, 2, 4);
        addEdge(graph, 3, 5);
        addEdge(graph, 4, 5);

        System.out.println(
            shortestPath(graph, 0, 5, vertices)
        );
    }

    static void addEdge(
            List<List<Integer>> graph,
            int u,
            int v) {

        graph.get(u).add(v);
        graph.get(v).add(u);
    }
}
```

Possible output:

```text
[0, 1, 3, 5]
```

Another equally short path may be returned depending on the order of neighbors.

### Complexity

- Time Complexity: O(V + E).
- Space Complexity: O(V).

---

## 10. BFS in a Grid (2D Matrix)

BFS is also commonly used in grid-based problems.

A grid can be treated as a graph:
- Each cell represents a vertex.
- Valid movements between neighboring cells represent edges.

### Problem

Given a grid containing `0` and `1`:
- `0` represents an open cell.
- `1` represents a blocked cell.

Find the minimum number of moves from the top-left cell to the bottom-right cell.

You can move up, down, left, or right.

Input:

```text
[
  [0, 0, 0],
  [1, 1, 0],
  [0, 0, 0]
]
```

One shortest path is:

```text
(0,0) → (0,1) → (0,2) → (1,2) → (2,2)
```

Output:

```text
4
```

The distance is measured in moves between cells.

### Java solution

```java
import java.util.*;

public class Main {

    static int shortestPath(int[][] grid) {

        int rows = grid.length;
        int cols = grid[0].length;

        if (grid[0][0] == 1 ||
                grid[rows - 1][cols - 1] == 1) {

            return -1;
        }

        int[][] distance = new int[rows][cols];

        for (int[] row : distance) {
            Arrays.fill(row, -1);
        }

        int[][] directions = {
            {-1, 0},  // Up
            {1, 0},   // Down
            {0, -1},  // Left
            {0, 1}    // Right
        };

        Queue<int[]> queue = new LinkedList<>();

        queue.offer(new int[]{0, 0});

        distance[0][0] = 0;

        while (!queue.isEmpty()) {

            int[] current = queue.poll();

            int row = current[0];
            int col = current[1];

            if (row == rows - 1 &&
                    col == cols - 1) {

                return distance[row][col];
            }

            for (int[] direction : directions) {

                int newRow = row + direction[0];

                int newCol = col + direction[1];

                if (newRow >= 0 &&
                        newRow < rows &&
                        newCol >= 0 &&
                        newCol < cols &&
                        grid[newRow][newCol] == 0 &&
                        distance[newRow][newCol] == -1) {

                    distance[newRow][newCol] =
                            distance[row][col] + 1;

                    queue.offer(new int[]{
                        newRow,
                        newCol
                    });
                }
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[][] grid = {
            {0, 0, 0},
            {1, 1, 0},
            {0, 0, 0}
        };

        System.out.println(shortestPath(grid));
    }
}
```

Output:

```text
4
```

### Important concepts

- A cell is visited only once.
- The `directions` array defines the allowed movements.
- Boundary checks prevent accessing invalid cells.
- The distance array stores the minimum number of moves.

**Time Complexity:** O(R × C).

**Space Complexity:** O(R × C).

Where:
- R = Number of rows.
- C = Number of columns.

---

## 11. Number of Connected Components Using BFS

### Problem

Given an undirected graph, find the number of connected components.

Example:

```text
0 ---- 1 ---- 2

3 ---- 4

5
```

There are three connected components:

```text
Component 1: 0, 1, 2
Component 2: 3, 4
Component 3: 5
```

Output:

```text
3
```

### Java solution

```java
import java.util.*;

public class Main {

    static int countComponents(
            List<List<Integer>> graph,
            int vertices) {

        boolean[] visited = new boolean[vertices];

        int components = 0;

        for (int i = 0; i < vertices; i++) {

            if (!visited[i]) {

                components++;

                bfs(graph, i, visited);
            }
        }

        return components;
    }

    static void bfs(
            List<List<Integer>> graph,
            int start,
            boolean[] visited) {

        Queue<Integer> queue = new LinkedList<>();

        queue.offer(start);

        visited[start] = true;

        while (!queue.isEmpty()) {

            int current = queue.poll();

            for (int neighbor : graph.get(current)) {

                if (!visited[neighbor]) {

                    visited[neighbor] = true;

                    queue.offer(neighbor);
                }
            }
        }
    }
}
```

**Time Complexity:** O(V + E).

**Space Complexity:** O(V).

---

## 12. Important BFS Applications

| Application | Description |
|---|---|
| Shortest path | Finds minimum-edge paths in unweighted graphs |
| Connected components | Finds disconnected groups |
| Grid traversal | Explores cells level by level |
| Level order traversal | Visits tree nodes by level |
| Social networks | Finds people within a certain connection distance |
| Word Ladder | Finds the minimum number of transformations |
| Network broadcasting | Spreads information level by level |
| Minimum moves | Finds minimum steps in puzzles and games |

---

## 13. BFS Time and Space Complexity

Let:
- V = Number of vertices.
- E = Number of edges.

| Operation | Complexity |
|---|---:|
| Initialize visited array | O(V) |
| Insert a vertex into Queue | O(1) |
| Remove a vertex from Queue | O(1) |
| Process all adjacency lists | O(V + E) |
| Total BFS | O(V + E) |
| Visited array | O(V) |
| Queue | O(V) worst case |

### Overall complexity

**Time Complexity:**

\[
O(V + E)
\]

**Space Complexity:**

\[
O(V)
\]

This analysis assumes an adjacency-list representation.

With an adjacency matrix, scanning all possible neighbors takes O(V) per vertex, giving O(V²) time.

---

## 14. BFS vs DFS

| Feature | BFS | DFS |
|---|---|---|
| Full form | Breadth First Search | Depth First Search |
| Approach | Level by level | Depth first |
| Data structure | Queue | Stack or recursion |
| Shortest path in unweighted graph | Yes | Not guaranteed |
| Traversal time | O(V + E) | O(V + E) |
| Auxiliary space | O(V) | O(V) worst case |
| Common use | Minimum steps and distances | Cycle detection and path exploration |

---

## 15. Common Mistakes

1. Forgetting to create a visited array.
2. Marking a vertex visited only after removing it from the Queue.
3. Using Stack instead of Queue for BFS.
4. Forgetting to handle disconnected components.
5. Forgetting to check whether the Queue is empty.
6. Assuming BFS finds shortest paths in arbitrary weighted graphs.
7. Not initializing distances to `-1` or another appropriate unvisited value.
8. Forgetting boundary conditions in grid problems.
9. Adding an undirected edge in only one direction.
10. Forgetting that traversal order can depend on neighbor ordering.

---

## 16. Common Interview Questions

**Q1. What is BFS?**

BFS is a graph traversal algorithm that visits vertices level by level, starting from a source vertex.

**Q2. Which data structure is used in BFS?**

Queue.

**Q3. Why does BFS use a Queue?**

Because FIFO processing ensures that vertices are explored in increasing order of distance from the source in an unweighted graph.

**Q4. What is the time complexity of BFS?**

O(V + E) using an adjacency list.

**Q5. Can BFS find the shortest path?**

Yes, BFS finds the shortest path in an unweighted graph, measured by the number of edges.

**Q6. Does BFS work for disconnected graphs?**

A single BFS visits only the component containing its starting vertex. Run BFS from every unvisited vertex to traverse all components.

**Q7. When should a vertex be marked as visited?**

When it is added to the Queue.

**Q8. What is the difference between BFS and DFS?**

BFS explores level by level using a Queue. DFS explores deeply using recursion or a Stack.

**Q9. Can BFS be used in a 2D matrix?**

Yes. Each cell can be treated as a vertex, and valid movements represent edges.

**Q10. Can BFS find shortest paths in weighted graphs?**

Ordinary BFS is not suitable for arbitrary different edge weights. Use an appropriate shortest-path algorithm such as Dijkstra's algorithm for non-negative weights.

**Q11. What is the space complexity of BFS?**

O(V) for the visited array and Queue.

**Q12. What is a connected component?**

A maximal group of vertices in an undirected graph where every vertex is reachable from every other vertex in that group.

---

## 17. Practice Problems

### Easy

1. Find if Path Exists in Graph.
2. Number of Provinces.
3. Find the Number of Connected Components.
4. Flood Fill.
5. Find if Graph Is Bipartite.

### Medium

1. Number of Islands.
2. Rotting Oranges.
3. 01 Matrix.
4. Open the Lock.
5. Shortest Path in Binary Matrix.
6. Word Ladder.
7. Minimum Genetic Mutation.
8. Nearest Exit from Entrance in Maze.

### Hard

1. Word Ladder II.
2. Shortest Path in a Grid with Obstacles Elimination.
3. Minimum Moves to Move a Box to Their Target Location.
4. Shortest Path Visiting All Nodes.

---

## 18. Quick Revision Cheat Sheet

### BFS template

```java
Queue<Integer> queue = new LinkedList<>();

boolean[] visited = new boolean[vertices];

visited[start] = true;

queue.offer(start);

while (!queue.isEmpty()) {

    int current = queue.poll();

    System.out.println(current);

    for (int neighbor : graph.get(current)) {

        if (!visited[neighbor]) {

            visited[neighbor] = true;

            queue.offer(neighbor);
        }
    }
}
```

### Important points

- BFS uses Queue.
- BFS explores level by level.
- Mark vertices visited when enqueuing them.
- BFS finds shortest paths in unweighted graphs.
- Run BFS from every unvisited vertex for disconnected graphs.
- BFS time complexity is O(V + E).
- BFS auxiliary space complexity is O(V).

---

## 19. Chapter Completion Checklist

- [ ] Understand Breadth First Search.
- [ ] Understand why BFS uses Queue.
- [ ] Implement BFS using adjacency list.
- [ ] Understand the visited array.
- [ ] Perform a BFS dry run.
- [ ] Implement BFS for disconnected graphs.
- [ ] Find shortest distance in an unweighted graph.
- [ ] Print the shortest path using a parent array.
- [ ] Implement BFS in a 2D grid.
- [ ] Count connected components.
- [ ] Understand BFS time and space complexity.
- [ ] Solve at least five BFS practice problems.

---

## Next Topic

**10-graphs/dfs.md**

In the next chapter, we will learn:

- What is Depth First Search?
- Why DFS uses Stack and recursion.
- Recursive DFS implementation in Java.
- Iterative DFS implementation using Stack.
- DFS for disconnected graphs.
- Cycle detection introduction.
- Connected components using DFS.
- Grid traversal using DFS.
- Interview questions and practice problems.