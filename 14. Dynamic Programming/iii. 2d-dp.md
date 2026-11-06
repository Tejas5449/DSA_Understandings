# 2D Dynamic Programming in Java

## 1. What is 2D Dynamic Programming?

**2D Dynamic Programming** is a technique where we use a two-dimensional array to store the results of smaller subproblems.

Unlike 1D DP, which generally uses one index, 2D DP uses two indexes to represent each state.

Example:

```java
int[][] dp = new int[rows][cols];
```

Here:

```text
dp[i][j] = Answer to the subproblem represented by row i and column j
```

2D DP is commonly used when problems involve:

- Grids and matrices.
- Two strings or sequences.
- Two changing variables.
- Comparing elements from two different collections.
- Finding minimum or maximum paths.

### Common 2D DP Problems

| Problem | Description |
|---|---|
| Unique Paths | Count ways to reach a destination |
| Unique Paths II | Count paths with obstacles |
| Minimum Path Sum | Find minimum cost through a grid |
| Longest Common Subsequence | Compare two strings |
| Edit Distance | Find minimum operations to convert strings |
| 0/1 Knapsack | Select items under a weight limit |

In this chapter, we will focus mainly on grid-based 2D DP.

---

## 2. Understanding a 2D DP Array

Consider a matrix with 3 rows and 3 columns.

```java
int[][] dp = new int[3][3];
```

It creates the following structure:

```text
       Column
       0  1  2

Row 0  0  0  0
Row 1  0  0  0
Row 2  0  0  0
```

To access an element:

```java
dp[0][0] = 10;
dp[1][2] = 20;
```

The first index represents the row, and the second index represents the column.

For a grid problem, a DP state might mean:

```text
dp[i][j] = Number of ways to reach cell (i, j)
```

Or:

```text
dp[i][j] = Minimum cost required to reach cell (i, j)
```

The meaning depends on the problem.

---

## 3. How to Solve a 2D DP Problem

Follow these steps:

1. Define the DP state using two indexes.
2. Identify the possible previous states.
3. Write the recurrence relation.
4. Identify the base cases.
5. Decide the order in which states should be calculated.
6. Optimize space if possible.

Let's understand these steps through common interview problems.

---

## 4. Problem 1: Unique Paths

<text id="example1">**Problem:**

You are given a grid with `m` rows and `n` columns.

A robot starts at the top-left cell `(0, 0)` and wants to reach the bottom-right cell `(m - 1, n - 1)`.

The robot can move only:

- Right
- Down

Find the total number of unique paths to reach the destination.

### Example

Input:

```text
m = 3
n = 3
```

Grid:

```text
S  .  .
.  .  .
.  .  E
```

`S` represents the starting cell and `E` represents the destination.

Possible paths include:

```text
Right -> Right -> Down -> Down

Right -> Down -> Right -> Down

Right -> Down -> Down -> Right

Down -> Right -> Right -> Down

Down -> Right -> Down -> Right

Down -> Down -> Right -> Right
```

Output:

```text
6
```

### Step 1: Define the DP State

```text
dp[i][j] = Number of unique paths to reach cell (i, j)
```

### Step 2: Identify Previous States

To reach cell `(i, j)`, the robot can come from:

- Top cell `(i - 1, j)`
- Left cell `(i, j - 1)`

It cannot come from the bottom or right because movement is restricted.

### Step 3: Recurrence Relation

```text
dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
```

### Step 4: Base Cases

There is only one way to reach the starting cell:

```text
dp[0][0] = 1
```

There is also only one way to reach any cell in the first row: keep moving right.

Similarly, there is only one way to reach any cell in the first column: keep moving down.

```text
dp[0][j] = 1
dp[i][0] = 1
```

### Step 5: Java Implementation Using Tabulation

```java
public class UniquePaths {

    public static int uniquePaths(int m, int n) {

        int[][] dp = new int[m][n];

        // Base cases
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }

        for (int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }

        // Calculate remaining states
        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[i][j] = dp[i - 1][j]
                         + dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }

    public static void main(String[] args) {

        System.out.println(uniquePaths(3, 3));
    }
}
```

Output:

```text
6
```

### Dry Run

Input:

```text
m = 3
n = 3
```

Initial DP array after setting base cases:

```text
1  1  1
1  0  0
1  0  0
```

Calculate `dp[1][1]`:

```text
dp[1][1] = dp[0][1] + dp[1][0]
         = 1 + 1
         = 2
```

Calculate `dp[1][2]`:

```text
dp[1][2] = dp[0][2] + dp[1][1]
         = 1 + 2
         = 3
```

Calculate `dp[2][1]`:

```text
dp[2][1] = dp[1][1] + dp[2][0]
         = 2 + 1
         = 3
```

Calculate `dp[2][2]`:

```text
dp[2][2] = dp[1][2] + dp[2][1]
         = 3 + 3
         = 6
```

Final DP array:

```text
1  1  1
1  2  3
1  3  6
```

Answer:

```text
6
```

### Complexity

- Time: O(m × n)
- Auxiliary space: O(m × n)

---

## 5. Unique Paths Using Memoization

We can also solve this problem using recursion and memoization.

### Recursive Thinking

To reach cell `(i, j)`, we can come from:

```text
(i - 1, j)
(i, j - 1)
```

Therefore:

```text
f(i, j) = f(i - 1, j) + f(i, j - 1)
```

Base cases:

- If we reach `(0, 0)`, return `1`.
- If the row or column becomes negative, return `0`.

### Java Implementation

```java
import java.util.Arrays;

public class UniquePathsMemoization {

    public static int solve(
            int i,
            int j,
            int[][] dp) {

        if (i == 0 && j == 0) {
            return 1;
        }

        if (i < 0 || j < 0) {
            return 0;
        }

        if (dp[i][j] != -1) {
            return dp[i][j];
        }

        int up = solve(i - 1, j, dp);

        int left = solve(i, j - 1, dp);

        dp[i][j] = up + left;

        return dp[i][j];
    }

    public static int uniquePaths(int m, int n) {

        int[][] dp = new int[m][n];

        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        return solve(m - 1, n - 1, dp);
    }

    public static void main(String[] args) {

        System.out.println(uniquePaths(3, 3));
    }
}
```

Output:

```text
6
```

### Complexity

- Time: O(m × n)
- Auxiliary space: O(m × n) for the DP array and O(m + n) for the recursion stack.

---

## 6. Unique Paths Using Space Optimization

In the previous solution, we used a two-dimensional DP array.

However, each cell depends only on:

- The cell above it.
- The cell to its left.

We can optimize the space using a one-dimensional array.

### Java Implementation

```java
public class UniquePathsOptimized {

    public static int uniquePaths(int m, int n) {

        int[] dp = new int[n];

        // First row has one way to reach every cell
        for (int j = 0; j < n; j++) {
            dp[j] = 1;
        }

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[j] = dp[j] + dp[j - 1];
            }
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {

        System.out.println(uniquePaths(3, 3));
    }
}
```

Output:

```text
6
```

### Understanding the Optimization

In the one-dimensional array:

```text
dp[j] = Value from the previous row
```

Before updating `dp[j]`:

- `dp[j]` represents the value from the cell above.
- `dp[j - 1]` represents the updated value from the left.

Therefore:

```text
dp[j] = dp[j] + dp[j - 1]
```

Complexity:

- Time: O(m × n)
- Auxiliary space: O(n)

---

## 7. Problem 2: Unique Paths II (With Obstacles)

**Problem:**

You are given a grid containing obstacles.

The robot starts at the top-left cell and wants to reach the bottom-right cell.

It can move only right or down.

A cell containing `1` represents an obstacle. A cell containing `0` represents an open cell.

The robot cannot move through an obstacle.

Find the number of unique paths.

### Example

Input:

```java
int[][] obstacleGrid = {
    {0, 0, 0},
    {0, 1, 0},
    {0, 0, 0}
};
```

Grid:

```text
S  .  .
.  X  .
.  .  E
```

`X` represents an obstacle.

Output:

```text
2
```

### Step 1: Define the DP State

```text
dp[i][j] = Number of valid paths to reach cell (i, j)
```

### Step 2: Identify the Recurrence

If the current cell contains an obstacle:

```text
dp[i][j] = 0
```

Otherwise:

```text
dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
```

### Step 3: Base Case

If the starting cell is blocked, no path exists.

Otherwise:

```text
dp[0][0] = 1
```

### Java Implementation

```java
public class UniquePathsWithObstacles {

    public static int uniquePathsWithObstacles(
            int[][] obstacleGrid) {

        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[][] dp = new int[m][n];

        // Starting cell is blocked
        if (obstacleGrid[0][0] == 1) {
            return 0;
        }

        dp[0][0] = 1;

        for (int i = 0; i < m; i++) {

            for (int j = 0; j < n; j++) {

                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                    continue;
                }

                if (i == 0 && j == 0) {
                    continue;
                }

                int up = 0;
                int left = 0;

                if (i > 0) {
                    up = dp[i - 1][j];
                }

                if (j > 0) {
                    left = dp[i][j - 1];
                }

                dp[i][j] = up + left;
            }
        }

        return dp[m - 1][n - 1];
    }

    public static void main(String[] args) {

        int[][] grid = {
            {0, 0, 0},
            {0, 1, 0},
            {0, 0, 0}
        };

        System.out.println(
                uniquePathsWithObstacles(grid)
        );
    }
}
```

Output:

```text
2
```

### Dry Run

Input:

```text
0  0  0
0  1  0
0  0  0
```

Initial state:

```text
1  0  0
0  0  0
0  0  0
```

After calculating the first row and first column:

```text
1  1  1
1  0  0
1  0  0
```

The obstacle at `(1, 1)` makes its path count zero.

Continue calculating:

```text
1  1  1
1  0  1
1  1  2
```

Final answer:

```text
2
```

### Complexity

- Time: O(m × n)
- Auxiliary space: O(m × n)

---

## 8. Unique Paths II Using Space Optimization

We can optimize the solution using a one-dimensional DP array.

### Java Implementation

```java
public class UniquePathsObstacleOptimized {

    public static int uniquePathsWithObstacles(
            int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[] dp = new int[n];

        if (grid[0][0] == 1) {
            return 0;
        }

        dp[0] = 1;

        for (int i = 0; i < m; i++) {

            for (int j = 0; j < n; j++) {

                if (grid[i][j] == 1) {

                    dp[j] = 0;

                } else if (j > 0) {

                    dp[j] = dp[j] + dp[j - 1];
                }
            }
        }

        return dp[n - 1];
    }
}
```

### Understanding the Code

```java
if (grid[i][j] == 1) {
    dp[j] = 0;
}
```

If a cell contains an obstacle, there are no valid ways to reach it.

```java
else if (j > 0) {
    dp[j] = dp[j] + dp[j - 1];
}
```

Otherwise, add the paths from above and left.

Complexity:

- Time: O(m × n)
- Auxiliary space: O(n)

---

## 9. Problem 3: Minimum Path Sum

**Problem:**

You are given a grid containing non-negative integers.

Start at the top-left cell and reach the bottom-right cell.

You can move only right or down.

Find the minimum possible sum of values along the path, including the starting and destination cells.

### Example

Input:

```java
int[][] grid = {
    {1, 3, 1},
    {1, 5, 1},
    {4, 2, 1}
};
```

Grid:

```text
1  3  1
1  5  1
4  2  1
```

One minimum-cost path is:

```text
1 -> 1 -> 5 -> 1 -> 1
```

Total:

```text
1 + 1 + 5 + 1 + 1 = 9
```

Output:

```text
9
```

### Step 1: Define the DP State

```text
dp[i][j] = Minimum path sum required to reach cell (i, j)
```

### Step 2: Identify Previous States

To reach `(i, j)`, we can come from:

- Above: `(i - 1, j)`
- Left: `(i, j - 1)`

### Step 3: Recurrence Relation

```text
dp[i][j] = grid[i][j]
         + min(dp[i - 1][j], dp[i][j - 1])
```

### Step 4: Base Cases

Starting cell:

```text
dp[0][0] = grid[0][0]
```

First row:

```text
dp[0][j] = grid[0][j] + dp[0][j - 1]
```

First column:

```text
dp[i][0] = grid[i][0] + dp[i - 1][0]
```

### Java Implementation

```java
public class MinimumPathSum {

    public static int minPathSum(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[][] dp = new int[m][n];

        dp[0][0] = grid[0][0];

        // First column
        for (int i = 1; i < m; i++) {

            dp[i][0] = grid[i][0] + dp[i - 1][0];
        }

        // First row
        for (int j = 1; j < n; j++) {

            dp[0][j] = grid[0][j] + dp[0][j - 1];
        }

        // Remaining cells
        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[i][j] = grid[i][j]
                        + Math.min(
                                dp[i - 1][j],
                                dp[i][j - 1]
                        );
            }
        }

        return dp[m - 1][n - 1];
    }

    public static void main(String[] args) {

        int[][] grid = {
            {1, 3, 1},
            {1, 5, 1},
            {4, 2, 1}
        };

        System.out.println(minPathSum(grid));
    }
}
```

Output:

```text
9
```

### Dry Run

Input:

```text
1  3  1
1  5  1
4  2  1
```

First row:

```text
1  4  5
```

First column:

```text
1
2
6
```

Calculate the remaining cells.

At `(1, 1)`:

```text
dp[1][1] = 5 + min(4, 2)
         = 7
```

At `(1, 2)`:

```text
dp[1][2] = 1 + min(5, 7)
         = 6
```

At `(2, 1)`:

```text
dp[2][1] = 2 + min(7, 6)
         = 8
```

At `(2, 2)`:

```text
dp[2][2] = 1 + min(6, 8)
         = 7
```

Final DP table:

```text
1  4  5
2  7  6
6  8  7
```

The minimum path sum is:

```text
7
```

The correct minimum path is:

```text
1 -> 1 -> 5 -> 1 -> 1
```

Total:

```text
9
```

The DP table above needs to be checked carefully: the correct first-column values are `1, 2, 6`, and the correct first-row values are `1, 4, 5`. At `(1, 1)`, the minimum is `min(4, 2) = 2`, so `dp[1][1] = 7`. At `(1, 2)`, `dp[1][2] = 1 + min(5, 7) = 6`. At `(2, 1)`, `dp[2][1] = 2 + min(7, 6) = 8`. At `(2, 2)`, `dp[2][2] = 1 + min(6, 8) = 7`.

Therefore, the correct minimum path sum for this grid is **7**, not 9. One minimum path is:

```text
1 -> 3 -> 1 -> 1 -> 1
```

Total:

```text
1 + 3 + 1 + 1 + 1 = 7
```

### Complexity

- Time: O(m × n)
- Auxiliary space: O(m × n)

---

## 10. Minimum Path Sum Using Space Optimization

We only need the previous row and the current row values.

A single array is enough.

### Java Implementation

```java
public class MinimumPathSumOptimized {

    public static int minPathSum(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[] dp = new int[n];

        for (int i = 0; i < m; i++) {

            for (int j = 0; j < n; j++) {

                if (i == 0 && j == 0) {

                    dp[j] = grid[i][j];

                } else {

                    int up = Integer.MAX_VALUE;
                    int left = Integer.MAX_VALUE;

                    if (i > 0) {
                        up = dp[j];
                    }

                    if (j > 0) {
                        left = dp[j - 1];
                    }

                    dp[j] = grid[i][j]
                            + Math.min(up, left);
                }
            }
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {

        int[][] grid = {
            {1, 3, 1},
            {1, 5, 1},
            {4, 2, 1}
        };

        System.out.println(minPathSum(grid));
    }
}
```

Output:

```text
7
```

### Complexity

- Time: O(m × n)
- Auxiliary space: O(n)

---

## 11. Recursion vs Memoization vs Tabulation

Let's compare the approaches for grid-based DP.

| Approach | Description | Typical Complexity |
|---|---|---|
| Recursion | Explores possible paths directly | Often exponential |
| Memoization | Stores recursive subproblem results | O(m × n) time |
| Tabulation | Calculates states iteratively | O(m × n) time |
| Space Optimization | Stores only necessary previous states | O(n) space |

### Which Approach Should You Use?

Use recursion first to understand the choices and recurrence.

Then use memoization to avoid repeated calculations.

Use tabulation to remove recursion.

Finally, optimize space if the recurrence only needs a previous row or a fixed number of states.

---

## 12. How to Identify 2D DP Problems

Look for these common signs:

### 1. The Problem Involves a Grid

Examples:

- Count paths through a matrix.
- Find the minimum path sum.
- Find the maximum reward in a grid.

### 2. The Problem Has Two Changing Variables

Examples:

- Two string indexes.
- Current row and column.
- Current item and remaining capacity.

### 3. The Problem Involves Two Sequences

Examples:

- Longest Common Subsequence.
- Edit Distance.
- Longest Common Substring.

### 4. The Problem Requires Multiple Choices

Examples:

- Move right or down.
- Pick or skip an item.
- Match or skip characters.

These are clues, not proof that a problem needs DP. Identify the state and recurrence before choosing the approach.

---

## 13. Common Mistakes in 2D DP

### Mistake 1: Incorrect DP State

Before coding, define exactly what the state represents.

For example:

```text
dp[i][j] = Minimum cost to reach cell (i, j)
```

### Mistake 2: Incorrect Base Cases

Always handle:

- Starting cell.
- First row.
- First column.
- Blocked starting cell, when obstacles exist.

### Mistake 3: Accessing Invalid Indexes

Incorrect:

```java
int up = dp[i - 1][j];
```

This causes an error when `i == 0`.

Use boundary checks.

### Mistake 4: Incorrect Loop Order

For right-and-down grid problems, calculate cells from top-left to bottom-right because each state depends on the top and left cells.

### Mistake 5: Forgetting Obstacles

If a cell is blocked:

```java
dp[i][j] = 0;
```

For path-counting problems, no valid path reaches that cell.

### Mistake 6: Confusing Minimum and Maximum Transitions

For counting paths:

```text
dp[i][j] = up + left
```

For minimum path sum:

```text
dp[i][j] = grid[i][j] + min(up, left)
```

The transition depends on the problem.

### Mistake 7: Integer Overflow

The number of paths can grow very quickly.

Use `long` when the problem constraints require it.

---

## 14. Frequently Asked Interview Questions

### Q1. What is 2D Dynamic Programming?

It is a DP technique that uses two-dimensional states, usually represented by a two-dimensional array.

### Q2. What is the difference between 1D DP and 2D DP?

1D DP uses one index to represent a state.

2D DP uses two indexes to represent a state.

### Q3. What is the recurrence relation for Unique Paths?

```text
dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
```

### Q4. Why do we initialize the first row and first column in Unique Paths?

Because there is exactly one way to reach each cell in the first row or first column when there are no obstacles.

### Q5. How do obstacles affect Unique Paths?

An obstacle makes the number of ways to reach that cell zero.

### Q6. What is the recurrence relation for Minimum Path Sum?

```text
dp[i][j] = grid[i][j]
         + min(dp[i - 1][j], dp[i][j - 1])
```

### Q7. Can 2D DP be optimized to 1D space?

Yes, when the current row depends only on the previous row and the current row's previous column.

### Q8. What is the time complexity of Unique Paths?

O(m × n).

### Q9. What is the space complexity of a normal 2D DP solution?

O(m × n), excluding any recursion stack.

### Q10. What is the space complexity after optimization?

For common grid problems, O(n), where `n` is the number of columns.

### Q11. Why do we use `Integer.MAX_VALUE` in Minimum Path Sum?

It represents an unavailable direction so that the minimum operation selects the valid direction. Care must be taken not to add a cost to `Integer.MAX_VALUE`, because that can overflow.

### Q12. Can we use recursion for grid DP?

Yes. Recursion can explore the possible previous states, and memoization can store the results to avoid repeated calculations.

---

## 15. Practice Problems

### Beginner

1. Unique Paths.
2. Unique Paths II.
3. Minimum Path Sum.
4. Count paths in a grid.
5. Find the maximum path sum in a grid.

### Intermediate

1. Triangle.
2. Minimum Falling Path Sum.
3. Maximal Square.
4. Dungeon Game.
5. Cherry Pickup.
6. Longest Common Subsequence.
7. Edit Distance.

### Recommended LeetCode Problems

| Problem | Difficulty |
|---|---|
| [Unique Paths](https://leetcode.com/problems/unique-paths/) | Medium |
| [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/) | Medium |
| [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | Medium |
| [Triangle](https://leetcode.com/problems/triangle/) | Medium |
| [Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/) | Medium |
| [Maximal Square](https://leetcode.com/problems/maximal-square/) | Medium |
| [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | Medium |
| [Edit Distance](https://leetcode.com/problems/edit-distance/) | Medium |

---

## 16. Quick Revision

- 2D DP uses two indexes to represent a state.
- A common declaration is `int[][] dp = new int[m][n]`.
- Define the meaning of `dp[i][j]` before writing code.
- Unique Paths counts ways from the top and left.
- Unique Paths II handles blocked cells.
- Minimum Path Sum finds the minimum accumulated cost.
- Memoization uses recursion and caching.
- Tabulation calculates states iteratively.
- Grid DP can often be optimized to O(n) auxiliary space.
- Check row and column boundaries carefully.
- Watch for integer overflow in path-counting problems.

---

## 17. Self-Assessment Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain 2D Dynamic Programming.
- [ ] Create and initialize a 2D DP array.
- [ ] Define a two-dimensional DP state.
- [ ] Write recurrence relations.
- [ ] Solve Unique Paths.
- [ ] Solve Unique Paths II.
- [ ] Solve Minimum Path Sum.
- [ ] Implement memoization.
- [ ] Implement tabulation.
- [ ] Optimize grid DP space to O(n).
- [ ] Explain time and space complexity.
- [ ] Solve at least three 2D DP problems independently.

---

## 18. What's Next?

You have completed the Dynamic Programming topics currently included in the repository structure:

- `dp-basics.md`
- `1d-dp.md`
- `2d-dp.md`

The next step is to revise the complete DSA roadmap and solve problems from the `problems/` directory.

Suggested next practice areas:

1. Arrays and Strings.
2. HashMap and HashSet.
3. Linked Lists.
4. Stack and Queue.
5. Trees and Binary Search Trees.
6. Heap and Priority Queue.
7. Graphs.
8. Sorting and Searching.
9. Dynamic Programming.

Start solving problems independently and document each solution with its approach, Java code, dry run, and complexity analysis.