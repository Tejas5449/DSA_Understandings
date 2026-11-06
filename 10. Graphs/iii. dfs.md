# Depth First Search (DFS) in Java

## 1. Introduction to DFS

**Depth First Search (DFS)** is a graph traversal algorithm used to visit all reachable vertices of a graph.

DFS explores a path as deeply as possible before backtracking to explore another path.

In simple words:

1. Start from a vertex.
2. Visit an unvisited adjacent vertex.
3. Continue exploring deeper vertices.
4. When no unvisited adjacent vertex remains, backtrack.
5. Repeat until all reachable vertices are visited.

DFS is commonly implemented using:

- Recursion (uses the call stack).
- An explicit stack (iterative DFS).

## 2. Example Graph

Consider the following undirected graph:

```text
       0
      / \
     1   2
    / \   \
   3   4   5
```

Adjacency list:

```text
0 -> 1, 2
1 -> 0, 3, 4
2 -> 0, 5
3 -> 1
4 -> 1
5 -> 2
```

Starting DFS from vertex `0`, one possible traversal order is:

```text
0 -> 1 -> 3 -> 4 -> 2 -> 5
```

The exact order depends on the order in which neighbors are stored and visited.

## 3. How DFS Works

DFS uses a `visited` array to prevent visiting the same vertex multiple times.

For each vertex:

1. Mark it as visited.
2. Process or print the vertex.
3. Visit each unvisited neighbor recursively.
4. Return when all neighbors have been explored.

### Pseudocode

```text
DFS(graph, vertex, visited):

    visited[vertex] = true

    process(vertex)

    for each neighbor of vertex:

        if neighbor is not visited:

            DFS(graph, neighbor, visited)
```

## 4. Graph Representation in Java

We can represent a graph using an adjacency list.

```java
import java.util.*;

public class Graph {

    private int vertices;
    private List<List<Integer>> adj;

    public Graph(int vertices) {

        this.vertices = vertices;

        adj = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            adj.add(new ArrayList<>());
        }
    }

    public void addEdge(int u, int v) {

        adj.get(u).add(v);
        adj.get(v).add(u);
    }
}
```

This implementation represents an undirected graph.

For a directed graph, add only:

```java
adj.get(u).add(v);
```

## 5. Recursive DFS Implementation

This is the most common DFS implementation.

```java
import java.util.*;

public class DFSExample {

    static void dfs(
            List<List<Integer>> graph,
            int node,
            boolean[] visited) {

        // Mark current node as visited
        visited[node] = true;

        // Process current node
        System.out.print(node + " ");

        // Visit all unvisited neighbors
        for (int neighbor : graph.get(node)) {

            if (!visited[neighbor]) {

                dfs(graph, neighbor, visited);
            }
        }
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
        graph.get(1).add(4);

        graph.get(2).add(0);
        graph.get(2).add(5);

        graph.get(3).add(1);
        graph.get(4).add(1);
        graph.get(5).add(2);

        boolean[] visited = new boolean[vertices];

        System.out.println("DFS Traversal:");

        dfs(graph, 0, visited);
    }
}
```

**Output:**

```text
DFS Traversal:
0 1 3 4 2 5
```

## 6. Dry Run of Recursive DFS

Graph:

```text
       0
      / \
     1   2
    / \   \
   3   4   5
```

Start from vertex `0`.

| Step | Current vertex | Action |
|---|---:|---|
| 1 | 0 | Visit 0 |
| 2 | 1 | Visit 1 |
| 3 | 3 | Visit 3 |
| 4 | 3 | No unvisited neighbor, backtrack |
| 5 | 1 | Visit next neighbor 4 |
| 6 | 4 | No unvisited neighbor, backtrack |
| 7 | 1 | All neighbors explored, backtrack |
| 8 | 0 | Visit next neighbor 2 |
| 9 | 2 | Visit next neighbor 5 |
| 10 | 5 | No unvisited neighbor, backtrack |
| 11 | 2 | All neighbors explored, backtrack |

Final traversal:

```text
0 -> 1 -> 3 -> 4 -> 2 -> 5
```

### Understanding the recursion stack

The recursive calls are conceptually stacked like this while exploring vertex `3`:

```text
DFS(0)
  |
  DFS(1)
    |
    DFS(3)
```

When vertex `3` has no unvisited neighbors, its function call returns to `DFS(1)`.

This process is called **backtracking**.

## 7. Iterative DFS Using Stack

DFS can also be implemented without recursion by using an explicit stack.

Java's `ArrayDeque` is a good choice for implementing a stack.

### Java Implementation

```java
import java.util.*;

public class IterativeDFS {

    static void dfs(
            List<List<Integer>> graph,
            int start,
            int vertices) {

        boolean[] visited = new boolean[vertices];

        Deque<Integer> stack = new ArrayDeque<>();

        stack.push(start);

        while (!stack.isEmpty()) {

            int node = stack.pop();

            if (visited[node]) {
                continue;
            }

            visited[node] = true;

            System.out.print(node + " ");

            // Push neighbors in reverse order
            List<Integer> neighbors = graph.get(node);

            for (int i = neighbors.size() - 1; i >= 0; i--) {

                int neighbor = neighbors.get(i);

                if (!visited[neighbor]) {
                    stack.push(neighbor);
                }
            }
        }
    }
}
```

### Why use a stack?

A stack follows LIFO:

**Last In, First Out**

The most recently pushed vertex is processed first, allowing DFS to explore deeper paths.

Pushing neighbors in reverse order can help produce an order similar to recursive DFS when adjacency lists use a consistent order.

However, the iterative traversal order can differ if neighbors are pushed in another order.

### Stack Operations

| Operation | Description |
|---|---|
| `push()` | Add an element to the top |
| `pop()` | Remove the top element |
| `peek()` | View the top element |
| `isEmpty()` | Check whether the stack is empty |

## 8. DFS for a Disconnected Graph

A graph may contain multiple disconnected components.

If we start DFS from vertex `0`, we cannot visit vertices that are not connected to vertex `0`.

To visit every vertex, loop through all vertices and start DFS whenever an unvisited vertex is found.

### Example

```text
Component 1: 0 --- 1 --- 2

Component 2: 3 --- 4

Component 3: 5
```

### Java Implementation

```java
static void dfs(
        List<List<Integer>> graph,
        int node,
        boolean[] visited) {

    visited[node] = true;

    System.out.print(node + " ");

    for (int neighbor : graph.get(node)) {

        if (!visited[neighbor]) {
            dfs(graph, neighbor, visited);
        }
    }
}

static void dfsAllComponents(
        List<List<Integer>> graph,
        int vertices) {

    boolean[] visited = new boolean[vertices];

    for (int i = 0; i < vertices; i++) {

        if (!visited[i]) {

            System.out.print("Component: ");

            dfs(graph, i, visited);

            System.out.println();
        }
    }
}
```

**Output for the example:**

```text
Component: 0 1 2
Component: 3 4
Component: 5
```

## 9. Count Connected Components Using DFS

A connected component is a group of vertices where every vertex is reachable from every other vertex in that group.

We can count connected components by starting DFS from each unvisited vertex.

Every new DFS start represents one new component.

### Java Implementation

```java
static void dfs(
        List<List<Integer>> graph,
        int node,
        boolean[] visited) {

    visited[node] = true;

    for (int neighbor : graph.get(node)) {

        if (!visited[neighbor]) {
            dfs(graph, neighbor, visited);
        }
    }
}

static int countComponents(
        List<List<Integer>> graph,
        int vertices) {

    boolean[] visited = new boolean[vertices];

    int count = 0;

    for (int i = 0; i < vertices; i++) {

        if (!visited[i]) {

            count++;

            dfs(graph, i, visited);
        }
    }

    return count;
}
```

**Time Complexity:** `O(V + E)`

## 10. Find Whether a Path Exists Between Two Vertices

DFS can determine whether a path exists between two vertices.

For example:

```text
0 --- 1 --- 2

3 --- 4
```

A path exists between `0` and `2`, but not between `0` and `4`.

### Java Implementation

```java
static boolean hasPath(
        List<List<Integer>> graph,
        int source,
        int destination,
        boolean[] visited) {

    if (source == destination) {
        return true;
    }

    visited[source] = true;

    for (int neighbor : graph.get(source)) {

        if (!visited[neighbor]) {

            if (hasPath(
                    graph,
                    neighbor,
                    destination,
                    visited)) {

                return true;
            }
        }
    }

    return false;
}
```

Usage:

```java
boolean[] visited = new boolean[vertices];

boolean result = hasPath(graph, 0, 4, visited);

System.out.println(result);
```

**Output:**

```text
false
```

This checks whether a path exists. It does not guarantee that the path found is the shortest path.

## 11. Cycle Detection in an Undirected Graph

A cycle exists when we can start from a vertex, follow edges, and return to the same vertex without repeating an edge.

Example:

```text
0 --- 1
|     |
|     |
3 --- 2
```

This graph contains a cycle.

### Important concept: Parent vertex

In an undirected graph, every edge appears in both adjacency lists.

For example:

```text
0 -> 1
1 -> 0
```

When DFS moves from `0` to `1`, it sees `0` again as a neighbor.

This is not a cycle. It is simply the edge we used to reach vertex `1`.

Therefore, we track the parent vertex.

A visited neighbor that is not the current vertex's parent indicates a cycle.

### Java Implementation

```java
static boolean hasCycle(
        List<List<Integer>> graph,
        int node,
        int parent,
        boolean[] visited) {

    visited[node] = true;

    for (int neighbor : graph.get(node)) {

        if (!visited[neighbor]) {

            if (hasCycle(
                    graph,
                    neighbor,
                    node,
                    visited)) {

                return true;
            }

        } else if (neighbor != parent) {

            return true;
        }
    }

    return false;
}
```

To check all components:

```java
static boolean containsCycle(
        List<List<Integer>> graph,
        int vertices) {

    boolean[] visited = new boolean[vertices];

    for (int i = 0; i < vertices; i++) {

        if (!visited[i]) {

            if (hasCycle(graph, i, -1, visited)) {
                return true;
            }
        }
    }

    return false;
}
```

**Note:** This implementation assumes a standard undirected graph without parallel edges. Self-loops and parallel edges need explicit consideration depending on the graph representation.

## 12. DFS in a 2D Grid

DFS is not limited to graphs represented using adjacency lists.

It is also frequently used in matrix and grid problems.

Example:

```text
1 1 0 0
1 0 0 1
0 0 1 1
1 0 0 0
```

We can move in four directions:

```text
       UP
        |
LEFT -- X -- RIGHT
        |
      DOWN
```

### Number of Islands

A common problem asks us to count connected groups of land cells.

- `1` represents land.
- `0` represents water.
- Adjacent land cells belong to the same island.
- Only horizontal and vertical connections count in this version.

### Java Implementation

```java
public class NumberOfIslands {

    static int rows;
    static int cols;

    static int[][] directions = {
        {-1, 0},  // Up
        {1, 0},   // Down
        {0, -1},  // Left
        {0, 1}    // Right
    };

    static void dfs(char[][] grid, int row, int col) {

        // Boundary check
        if (row < 0 || col < 0 ||
            row >= rows || col >= cols) {
            return;
        }

        // Stop if water or already visited
        if (grid[row][col] == '0') {
            return;
        }

        // Mark land as visited
        grid[row][col] = '0';

        // Explore four directions
        for (int[] direction : directions) {

            int newRow = row + direction[0];
            int newCol = col + direction[1];

            dfs(grid, newRow, newCol);
        }
    }

    static int numIslands(char[][] grid) {

        rows = grid.length;
        cols = grid[0].length;

        int islands = 0;

        for (int i = 0; i < rows; i++) {

            for (int j = 0; j < cols; j++) {

                if (grid[i][j] == '1') {

                    islands++;

                    dfs(grid, i, j);
                }
            }
        }

        return islands;
    }
}
```

**Time Complexity:** `O(rows × cols)`

**Space Complexity:** `O(rows × cols)` in the worst case due to recursive calls.

**Important:** This implementation modifies the original grid by changing visited land cells from `'1'` to `'0'`. If the original grid must be preserved, use a separate `visited` matrix.

## 13. DFS for Directed Graphs

In a directed graph, edges have a direction.

Example:

```text
0 -> 1 -> 2
|
v
3
```

If there is an edge from `0` to `1`, it does not automatically mean that an edge exists from `1` to `0`.

For directed graphs:

- Add only the outgoing edge to the adjacency list.
- DFS follows outgoing edges.
- Cycle detection requires different logic from the undirected parent-based approach.

### Directed Cycle Detection Using Three States

We use three states:

| State | Meaning |
|---|---|
| `0` | Not visited |
| `1` | Currently being explored |
| `2` | Fully explored |

If DFS reaches a vertex with state `1`, it has found a back edge and therefore a cycle.

### Java Implementation

```java
static boolean hasDirectedCycle(
        List<List<Integer>> graph,
        int node,
        int[] state) {

    state[node] = 1;

    for (int neighbor : graph.get(node)) {

        if (state[neighbor] == 1) {
            return true;
        }

        if (state[neighbor] == 0) {

            if (hasDirectedCycle(
                    graph,
                    neighbor,
                    state)) {

                return true;
            }
        }
    }

    state[node] = 2;

    return false;
}

static boolean containsDirectedCycle(
        List<List<Integer>> graph,
        int vertices) {

    int[] state = new int[vertices];

    for (int i = 0; i < vertices; i++) {

        if (state[i] == 0) {

            if (hasDirectedCycle(graph, i, state)) {
                return true;
            }
        }
    }

    return false;
}
```

This is also a useful foundation for understanding topological sorting.

## 14. DFS vs BFS

| Feature | DFS | BFS |
|---|---|---|
| Full form | Depth First Search | Breadth First Search |
| Data structure | Stack | Queue |
| Approach | Goes deep first | Explores level by level |
| Implementation | Recursion or stack | Queue |
| Shortest path | Not guaranteed | Guaranteed for unweighted graphs |
| Memory | Can use call stack | Can store a wide frontier |
| Common use | Cycle detection, backtracking | Shortest path, level traversal |

### When should you use DFS?

Use DFS when:

- You need to explore all possible paths.
- You need to detect cycles.
- You need to count connected components.
- You are solving backtracking problems.
- You need to explore connected regions in a grid.

### When should you use BFS?

Use BFS when:

- You need the shortest path in an unweighted graph.
- You need level-order traversal.
- You need the minimum number of steps.
- You need to explore vertices by distance from a source.

## 15. Time and Space Complexity

For an adjacency-list graph:

| Operation | Complexity |
|---|---|
| Visit a vertex | `O(1)` |
| Explore all vertices | `O(V)` |
| Explore all edges | `O(E)` |
| Complete DFS traversal | `O(V + E)` |

### Time Complexity

```text
O(V + E)
```

Where:

- `V` = Number of vertices.
- `E` = Number of edges.

Every vertex is visited at most once, and every adjacency-list edge is examined at most once.

### Space Complexity

```text
O(V)
```

The visited array requires `O(V)` space.

The recursion stack can also require up to `O(V)` space in the worst case.

For iterative DFS, the explicit stack can also hold up to `O(V)` vertices.

If the graph itself is included, its adjacency-list representation requires `O(V + E)` space.

## 16. Common DFS Mistakes

1. **Not marking a vertex as visited**

   This can cause infinite recursion in cyclic graphs.

2. **Marking a vertex too late**

   Mark it as visited immediately when entering the DFS function.

3. **Not handling disconnected graphs**

   Starting DFS from one vertex only visits its connected component.

4. **Confusing DFS with shortest path**

   DFS does not guarantee the shortest path in an unweighted graph.

5. **Ignoring parent tracking in undirected cycle detection**

   The edge back to the parent is not itself a cycle.

6. **Using undirected cycle detection for directed graphs**

   Directed graphs require a different cycle detection approach.

7. **Stack overflow in deep recursive DFS**

   Very deep graphs may exceed Java's call stack capacity. Consider iterative DFS for such cases.

8. **Forgetting grid boundary checks**

   Grid DFS must verify row and column limits before accessing a cell.

## 17. Important DFS Interview Questions

### Beginner Level

1. What is Depth First Search?
2. How does DFS work?
3. Which data structure is used in DFS?
4. What is the difference between recursive and iterative DFS?
5. What is a visited array?
6. What is the time complexity of DFS?
7. Can DFS work on disconnected graphs?

### Intermediate Level

1. How do you count connected components using DFS?
2. How do you detect a cycle in an undirected graph?
3. How do you detect a cycle in a directed graph?
4. How do you find whether a path exists between two vertices?
5. How do you count islands in a 2D grid?
6. What is the difference between DFS and BFS?
7. Why does DFS not always find the shortest path?

### Advanced Level

1. What is topological sorting?
2. How is DFS used in detecting strongly connected components?
3. What is a back edge in DFS?
4. What is a DFS tree?
5. How is DFS used in maze-solving problems?
6. What are discovery and finishing times?
7. What is the difference between a tree edge and a back edge?

## 18. Practice Problems

Try solving these problems using DFS.

### Easy

1. [Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph/)
2. [Flood Fill](https://leetcode.com/problems/flood-fill/)
3. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### Medium

1. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
2. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
3. [Clone Graph](https://leetcode.com/problems/clone-graph/)
4. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
5. [Course Schedule](https://leetcode.com/problems/course-schedule/)

### Hard

1. [Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/)
2. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## 19. DFS Quick Revision

| Concept | Remember |
|---|---|
| DFS | Explore deeply before backtracking |
| Recursive DFS | Uses the call stack |
| Iterative DFS | Uses an explicit stack |
| Visited array | Prevents repeated visits |
| Disconnected graph | Start DFS from every unvisited vertex |
| Connected components | Count separate DFS starts |
| Undirected cycle | Track parent vertex |
| Directed cycle | Track current recursion path |
| Grid DFS | Check boundaries and directions |
| Time complexity | `O(V + E)` |
| Auxiliary space | `O(V)` |

## 20. Learning Checklist

- [ ] Understand DFS traversal.
- [ ] Implement recursive DFS in Java.
- [ ] Implement iterative DFS using `ArrayDeque`.
- [ ] Perform a DFS dry run manually.
- [ ] Traverse a disconnected graph.
- [ ] Count connected components.
- [ ] Find whether a path exists.
- [ ] Detect cycles in an undirected graph.
- [ ] Detect cycles in a directed graph.
- [ ] Solve Number of Islands using DFS.
- [ ] Practice at least three DFS problems on LeetCode.

---

## Conclusion

DFS is one of the most important graph traversal algorithms.

The key idea is simple:

**Go as deep as possible, then backtrack.**

Once you understand DFS, you can solve many graph problems involving connected components, cycle detection, path exploration, grids, and backtracking.