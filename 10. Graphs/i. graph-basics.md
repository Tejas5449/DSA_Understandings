# Graph Basics in Java

## 1. Introduction to Graph Data Structure

A Graph is a non-linear data structure consisting of a collection of vertices (nodes) and edges (connections between nodes).

Graphs are used to represent relationships and connections between different entities.

### Real-world examples

- Social networks: Users connected as friends.
- Google Maps: Cities connected by roads.
- Computer networks: Computers connected to each other.
- Airline systems: Airports connected through flights.
- Web pages: Pages connected through hyperlinks.
- Recommendation systems: Users connected to products or interests.

### Example of a Graph

```text
        1
       / \
      2   3
      |   |
      4---5
```

In this graph:

- `1`, `2`, `3`, `4`, and `5` are vertices.
- Connections between them are edges.

---

## 2. Important Graph Terminology

### 2.1 Vertex (Node)

A vertex represents an individual element in a graph.

Example:

```text
A, B, C, D
```

These are four vertices.

### 2.2 Edge

An edge represents a connection between two vertices.

Example:

```text
A ---- B
```

The connection between A and B is an edge.

### 2.3 Adjacent Vertices

Two vertices are adjacent if there is an edge connecting them.

Example:

```text
A ---- B ---- C
```

- A and B are adjacent.
- B and C are adjacent.
- A and C are not adjacent.

### 2.4 Degree of a Vertex

The degree of a vertex in an undirected graph is the number of edges connected to it.

Example:

```text
       A
      / \
     B   C
      \ /
       D
```

- Degree of A = 2.
- Degree of B = 2.
- Degree of C = 2.
- Degree of D = 2.

For a directed graph, we distinguish between:
- **Indegree:** Number of incoming edges.
- **Outdegree:** Number of outgoing edges.

### 2.5 Path

A path is a sequence of vertices connected through edges.

Example:

```text
A ---- B ---- C ---- D
```

A to D has the path:

```text
A → B → C → D
```

### 2.6 Cycle

A cycle is a path that starts and ends at the same vertex, following at least one edge.

Example:

```text
A ---- B
|      |
D ---- C
```

One cycle is:

```text
A → B → C → D → A
```

### 2.7 Connected Graph

An undirected graph is connected if every vertex can be reached from every other vertex.

Example:

```text
A ---- B ---- C
       |
       D
```

All vertices are connected through paths.

### 2.8 Disconnected Graph

A graph is disconnected if at least one pair of vertices has no path connecting them.

Example:

```text
A ---- B       C ---- D
```

There are two disconnected components.

### 2.9 Weight

A weight is a numerical value assigned to an edge.

Example:

```text
A ----5---- B
|
3
|
C
```

The weight of the edge between A and B is 5.

Weights can represent:
- Distance.
- Cost.
- Time.
- Network capacity.

---

## 3. Types of Graphs

### 3.1 Undirected Graph

In an undirected graph, edges do not have a direction.

If A is connected to B, B is also connected to A.

Example:

```text
A ----- B
```

This represents:

```text
A ↔ B
```

Real-world example: A two-way friendship relationship.

### 3.2 Directed Graph

In a directed graph, edges have a specific direction.

Example:

```text
A ----> B
```

This means A has an outgoing connection to B.

It does not automatically mean B has a connection back to A.

Real-world example:
- Following someone on social media.
- One-way roads.
- Dependencies between tasks.

### 3.3 Weighted Graph

A weighted graph assigns a weight to each edge.

Example:

```text
A ----10---- B
|            |
5            8
|            |
C ----3----- D
```

This can represent distances between cities.

### 3.4 Unweighted Graph

An unweighted graph does not assign numerical weights to edges.

Example:

```text
A ---- B
|      |
C ---- D
```

Each edge is treated equally.

### 3.5 Cyclic Graph

A graph containing at least one cycle is called a cyclic graph.

Example:

```text
A ---- B
|      |
D ---- C
```

### 3.6 Acyclic Graph

A graph without any cycles is called an acyclic graph.

Example:

```text
A ---- B ---- C
       |
       D
```

A connected, undirected, acyclic graph is called a Tree.

A Directed Acyclic Graph is called a DAG.

DAGs are commonly used in:
- Task scheduling.
- Build systems.
- Dependency management.
- Course prerequisite systems.

### 3.7 Complete Graph

A simple undirected graph is complete if every pair of distinct vertices has an edge between them.

For three vertices:

```text
       A
      / \
     B---C
```

Every vertex is connected to every other vertex.

For an undirected complete graph with `V` vertices:

\[
E = \frac{V(V-1)}{2}
\]

### 3.8 Bipartite Graph

A graph is bipartite if its vertices can be divided into two sets such that every edge connects vertices from different sets.

Example:

```text
Set A             Set B

  A1  -----------  B1
   |               |
  A2  -----------  B2
```

Applications:
- Job assignment.
- Matching students to courses.
- Matching users to products.

---

## 4. Graph Representation

A graph can be represented in different ways.

The two most common representations are:

1. Adjacency Matrix.
2. Adjacency List.

Let's consider this graph:

```text
       0
      / \
     1---2
      \
       3
```

Edges:

```text
0 - 1
0 - 2
1 - 2
1 - 3
```

Number of vertices = 4.

Number of edges = 4.

---

## 5. Adjacency Matrix

An Adjacency Matrix is a two-dimensional array used to represent connections between vertices.

If an edge exists between two vertices, we store `1`; otherwise, we store `0`.

### Example

Graph:

```text
       0
      / \
     1---2
      \
       3
```

Adjacency Matrix:

```text
    0  1  2  3

0   0  1  1  0
1   1  0  1  1
2   1  1  0  0
3   0  1  0  0
```

### Java implementation

```java
public class Main {

    public static void main(String[] args) {

        int vertices = 4;

        int[][] graph = new int[vertices][vertices];

        // Add edges
        graph[0][1] = 1;
        graph[1][0] = 1;

        graph[0][2] = 1;
        graph[2][0] = 1;

        graph[1][2] = 1;
        graph[2][1] = 1;

        graph[1][3] = 1;
        graph[3][1] = 1;

        // Print adjacency matrix
        for (int i = 0; i < vertices; i++) {

            for (int j = 0; j < vertices; j++) {

                System.out.print(graph[i][j] + " ");
            }

            System.out.println();
        }
    }
}
```

### Adding an edge in an undirected graph

```java
graph[u][v] = 1;
graph[v][u] = 1;
```

Both directions must be updated because the graph is undirected.

### Adding an edge in a directed graph

```java
graph[u][v] = 1;
```

Only the outgoing direction is updated.

### Weighted graph using adjacency matrix

Instead of storing `1`, store the edge weight.

```java
int[][] graph = new int[4][4];

graph[0][1] = 10;
graph[1][0] = 10;
```

Use a separate convention or structure to distinguish a missing edge from a valid edge with weight zero.

### Complexity

| Operation | Complexity |
|---|---:|
| Space | O(V²) |
| Check whether an edge exists | O(1) |
| Add an edge | O(1) |
| Remove an edge | O(1) |
| Find all neighbors | O(V) |

### Advantages

- Easy to implement.
- Checking an edge is very fast.
- Useful for dense graphs.

### Disadvantages

- Requires O(V²) memory.
- Not memory-efficient for sparse graphs.

---

## 6. Adjacency List

An Adjacency List stores the neighbors of every vertex.

For example:

```text
       0
      / \
     1---2
      \
       3
```

Adjacency List:

```text
0 → 1, 2
1 → 0, 2, 3
2 → 0, 1
3 → 1
```

### Java implementation using ArrayList

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        int vertices = 4;

        List<List<Integer>> graph = new ArrayList<>();

        // Create an empty list for every vertex
        for (int i = 0; i < vertices; i++) {

            graph.add(new ArrayList<>());
        }

        // Add undirected edges
        addEdge(graph, 0, 1);
        addEdge(graph, 0, 2);
        addEdge(graph, 1, 2);
        addEdge(graph, 1, 3);

        // Print adjacency list
        for (int i = 0; i < vertices; i++) {

            System.out.println(
                i + " -> " + graph.get(i)
            );
        }
    }

    static void addEdge(
            List<List<Integer>> graph, int u, int v) {

        graph.get(u).add(v);

        graph.get(v).add(u);
    }
}
```

Output:

```text
0 -> [1, 2]
1 -> [0, 2, 3]
2 -> [0, 1]
3 -> [1]
```

### Adding an edge in an undirected graph

```java
graph.get(u).add(v);
graph.get(v).add(u);
```

### Adding an edge in a directed graph

```java
graph.get(u).add(v);
```

### Weighted graph using adjacency list

We can use a separate class to store the neighbor and edge weight.

```java
import java.util.*;

class Edge {

    int destination;
    int weight;

    Edge(int destination, int weight) {
        this.destination = destination;
        this.weight = weight;
    }
}

public class Main {

    public static void main(String[] args) {

        int vertices = 3;

        List<List<Edge>> graph = new ArrayList<>();

        for (int i = 0; i < vertices; i++) {
            graph.add(new ArrayList<>());
        }

        // Undirected weighted edge
        graph.get(0).add(new Edge(1, 10));
        graph.get(1).add(new Edge(0, 10));

        graph.get(0).add(new Edge(2, 5));
        graph.get(2).add(new Edge(0, 5));

        for (int i = 0; i < vertices; i++) {

            System.out.print(i + " -> ");

            for (Edge edge : graph.get(i)) {

                System.out.print(
                    "(" + edge.destination
                    + ", " + edge.weight + ") "
                );
            }

            System.out.println();
        }
    }
}
```

Output:

```text
0 -> (1, 10) (2, 5)
1 -> (0, 10)
2 -> (0, 5)
```

### Complexity

| Operation | Complexity |
|---|---:|
| Space | O(V + E) |
| Add an edge | O(1) |
| Check whether an edge exists | O(degree of vertex) |
| Find all neighbors | O(degree of vertex) |
| Remove an edge | O(degree of vertex) |

### Advantages

- Memory-efficient for sparse graphs.
- Easy to iterate over neighbors.
- Commonly used in graph algorithms.

### Disadvantages

- Checking whether a particular edge exists may require scanning the neighbor list.
- Slightly more complex than an adjacency matrix.

---

## 7. Adjacency Matrix vs Adjacency List

| Feature | Adjacency Matrix | Adjacency List |
|---|---|---|
| Structure | 2D array | List of neighbor lists |
| Space | O(V²) | O(V + E) |
| Check edge | O(1) | O(degree) |
| Add edge | O(1) | O(1) |
| Find neighbors | O(V) | O(degree) |
| Best suited for | Dense graphs | Sparse graphs |
| Memory usage | Higher for sparse graphs | Usually lower for sparse graphs |

### Which one should you use?

Use Adjacency Matrix when:
- The graph has many edges.
- You need frequent edge-existence checks.
- The number of vertices is relatively small.

Use Adjacency List when:
- The graph has relatively few edges.
- Memory efficiency is important.
- You frequently traverse neighboring vertices.

**Interview tip:** Most graph coding problems use Adjacency Lists.

---

## 8. Graph Representation Using HashMap

Sometimes graph vertices are strings instead of integers.

Example:

```text
Mumbai ---- Pune
   |
Nashik
```

We can use a HashMap to represent the graph.

### Java implementation

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Map<String, List<String>> graph = new HashMap<>();

        graph.put("Mumbai", new ArrayList<>());
        graph.put("Pune", new ArrayList<>());
        graph.put("Nashik", new ArrayList<>());

        addEdge(graph, "Mumbai", "Pune");
        addEdge(graph, "Mumbai", "Nashik");

        System.out.println(graph);
    }

    static void addEdge(
            Map<String, List<String>> graph,
            String u,
            String v) {

        graph.get(u).add(v);

        graph.get(v).add(u);
    }
}
```

Output order may vary:

```text
{
  Mumbai=[Pune, Nashik],
  Pune=[Mumbai],
  Nashik=[Mumbai]
}
```

This approach is useful when graph vertices have names rather than numeric IDs.

---

## 9. Basic Graph Operations

### 9.1 Add a Vertex

For an adjacency list:

```java
graph.add(new ArrayList<>());
```

This assumes vertices are represented by consecutive integer indices.

For a named graph:

```java
graph.put("Delhi", new ArrayList<>());
```

### 9.2 Add an Edge

For an undirected graph:

```java
graph.get(u).add(v);
graph.get(v).add(u);
```

For a directed graph:

```java
graph.get(u).add(v);
```

### 9.3 Remove an Edge

For an undirected graph:

```java
graph.get(u).remove(Integer.valueOf(v));
graph.get(v).remove(Integer.valueOf(u));
```

For a directed graph:

```java
graph.get(u).remove(Integer.valueOf(v));
```

Using `Integer.valueOf(v)` ensures that the value is removed rather than treating `v` as a list index.

### 9.4 Check Whether an Edge Exists

```java
boolean exists = graph.get(u).contains(v);
```

This takes O(degree of u) time for an adjacency list backed by ArrayList.

---

## 10. Important Graph Properties and Formulas

Let:
- `V` = Number of vertices.
- `E` = Number of edges.

### Undirected Graph

The sum of degrees of all vertices equals twice the number of edges.

\[
\sum degree(v) = 2E
\]

This is called the Handshaking Lemma.

### Directed Graph

The sum of indegrees equals the sum of outdegrees.

\[
\sum indegree(v) = \sum outdegree(v) = E
\]

### Maximum edges in a simple undirected graph

For a graph without self-loops or multiple edges:

\[
E_{max} = \frac{V(V-1)}{2}
\]

### Maximum edges in a simple directed graph

Without self-loops:

\[
E_{max} = V(V-1)
\]

---

## 11. Graph vs Tree

A Tree is a special type of Graph.

| Feature | Graph | Tree |
|---|---|---|
| Structure | General network | Hierarchical or connected acyclic structure |
| Cycles | May contain cycles | No cycles |
| Connectivity | May be disconnected | Connected |
| Edges | Depends on graph type | Exactly V - 1 |
| Path between two vertices | May have multiple paths | Exactly one simple path between any two vertices |
| Direction | Directed or undirected | Usually discussed as undirected unless rooted/directed |

**Important:** A connected, undirected graph with no cycles is a Tree.

---

## 12. What Are BFS and DFS?

Two important algorithms are used to traverse graphs:

1. Breadth First Search (BFS).
2. Depth First Search (DFS).

### BFS

BFS visits neighboring vertices first, then moves to the next level.

It uses a Queue.

Example:

```text
       0
      / \
     1   2
      \ /
       3
```

One possible BFS order starting from `0`:

```text
0 1 2 3
```

### DFS

DFS explores one path as deeply as possible before backtracking.

It uses recursion or a Stack.

One possible DFS order starting from `0`:

```text
0 1 3 2
```

Traversal order can vary depending on the order in which neighbors are stored.

These algorithms will be covered in separate chapters.

---

## 13. Complete Java Example: Graph Using Adjacency List

```java
import java.util.*;

public class Main {

    static class Graph {

        int vertices;

        List<List<Integer>> adjacencyList;

        Graph(int vertices) {

            this.vertices = vertices;

            adjacencyList = new ArrayList<>();

            for (int i = 0; i < vertices; i++) {

                adjacencyList.add(new ArrayList<>());
            }
        }

        // Add undirected edge
        void addEdge(int u, int v) {

            adjacencyList.get(u).add(v);

            adjacencyList.get(v).add(u);
        }

        // Print graph
        void printGraph() {

            for (int i = 0; i < vertices; i++) {

                System.out.println(
                    i + " -> " + adjacencyList.get(i)
                );
            }
        }

        // Check edge
        boolean hasEdge(int u, int v) {

            return adjacencyList.get(u).contains(v);
        }

        // Remove undirected edge
        void removeEdge(int u, int v) {

            adjacencyList.get(u).remove(Integer.valueOf(v));

            adjacencyList.get(v).remove(Integer.valueOf(u));
        }
    }

    public static void main(String[] args) {

        Graph graph = new Graph(5);

        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 2);
        graph.addEdge(1, 3);
        graph.addEdge(3, 4);

        System.out.println("Graph:");

        graph.printGraph();

        System.out.println(
            "Edge 1-3 exists: " + graph.hasEdge(1, 3)
        );

        graph.removeEdge(1, 3);

        System.out.println("After removing edge 1-3:");

        graph.printGraph();
    }
}
```

---

## 14. Common Mistakes

1. Adding an undirected edge in only one direction.
2. Adding both directions for a directed graph when only one is required.
3. Confusing vertices with edges.
4. Using an adjacency matrix for a very large sparse graph without considering memory.
5. Forgetting to initialize the neighbor list for every vertex.
6. Assuming all graphs are connected.
7. Assuming every graph is acyclic.
8. Confusing a Tree with a general Graph.
9. Forgetting to maintain edge weights when implementing weighted graphs.
10. Assuming BFS or DFS always produces one fixed traversal order regardless of neighbor ordering.

---

## 15. Common Interview Questions

**Q1. What is a Graph?**

A Graph is a non-linear data structure consisting of vertices and edges that represent connections.

**Q2. What is a vertex?**

A vertex is an individual node in a graph.

**Q3. What is an edge?**

An edge represents a connection between two vertices.

**Q4. What is the difference between directed and undirected graphs?**

A directed graph has edges with direction. An undirected graph has edges without direction.

**Q5. What is a weighted graph?**

A graph in which edges have numerical weights such as distance or cost.

**Q6. What is a cycle?**

A path that starts and ends at the same vertex.

**Q7. What is a connected graph?**

An undirected graph in which every pair of vertices has a path connecting them.

**Q8. What is the difference between adjacency matrix and adjacency list?**

An adjacency matrix uses O(V²) space, while an adjacency list uses O(V + E) space.

**Q9. Which graph representation is more memory-efficient for sparse graphs?**

Adjacency List.

**Q10. What is the Handshaking Lemma?**

The sum of degrees of all vertices in an undirected graph is equal to twice the number of edges.

**Q11. What is a DAG?**

A Directed Acyclic Graph is a directed graph without cycles.

**Q12. What is a bipartite graph?**

A graph whose vertices can be divided into two sets so that every edge connects vertices from different sets.

**Q13. What is the difference between BFS and DFS?**

BFS visits level by level using a Queue. DFS explores deeply using recursion or a Stack.

**Q14. Can a graph have multiple connected components?**

Yes. A disconnected graph consists of two or more connected components.

---

## 16. Practice Problems

### Easy

1. Find if Path Exists in Graph.
2. Find the Number of Provinces.
3. Find Center of Star Graph.
4. Find the Town Judge.
5. Find if Graph Is Bipartite.

### Medium

1. Clone Graph.
2. Number of Islands.
3. Max Area of Island.
4. Keys and Rooms.
5. All Paths From Source to Target.
6. Course Schedule.
7. Find if Path Exists in Graph.

### Hard

1. Critical Connections in a Network.
2. Word Ladder.
3. Shortest Path in a Grid with Obstacles Elimination.
4. Alien Dictionary.
5. Minimum Cost to Connect All Points.

---

## 17. Quick Revision Cheat Sheet

| Concept | Meaning |
|---|---|
| Vertex | Node in a graph |
| Edge | Connection between vertices |
| Degree | Number of connected edges in an undirected graph |
| Indegree | Number of incoming directed edges |
| Outdegree | Number of outgoing directed edges |
| Path | Sequence of connected vertices |
| Cycle | Path returning to its starting vertex |
| Connected graph | Every pair of vertices has a path |
| Disconnected graph | Contains multiple connected components |
| Weighted graph | Edges have weights |
| DAG | Directed Acyclic Graph |
| BFS | Breadth First Search |
| DFS | Depth First Search |

### Important formulas

- Undirected graph: Sum of degrees = `2E`.
- Directed graph: Sum of indegrees = Sum of outdegrees = `E`.
- Maximum undirected edges = `V * (V - 1) / 2`.
- Maximum directed edges without self-loops = `V * (V - 1)`.
- Tree edges = `V - 1`.

---

## 18. Chapter Completion Checklist

- [ ] Understand Graph data structure.
- [ ] Understand vertices and edges.
- [ ] Learn directed and undirected graphs.
- [ ] Learn weighted and unweighted graphs.
- [ ] Understand cyclic and acyclic graphs.
- [ ] Understand connected and disconnected graphs.
- [ ] Implement adjacency matrix in Java.
- [ ] Implement adjacency list in Java.
- [ ] Implement weighted graph representation.
- [ ] Understand graph operations.
- [ ] Understand BFS and DFS basics.
- [ ] Solve at least five graph problems.

---

## Next Topic

**10-graphs/bfs.md**

In the next chapter, we will learn:

- What is Breadth First Search?
- Why BFS uses a Queue.
- BFS traversal in Java.
- BFS for disconnected graphs.
- Shortest path in an unweighted graph.
- Level-based graph traversal.
- Dry runs and complexity analysis.
- Interview questions and practice problems.