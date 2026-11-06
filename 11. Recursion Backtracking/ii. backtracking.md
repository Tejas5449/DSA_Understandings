# Backtracking in Java

## 1. What Is Backtracking?

**Backtracking is an algorithmic technique that solves problems by exploring possible choices and undoing a choice when it cannot lead to a valid solution.**

It is commonly used when a problem requires us to explore multiple possible combinations or arrangements.

The basic idea is:

1. Make a choice.
2. Explore the result of that choice.
3. If the choice does not lead to a solution, undo it.
4. Try another choice.
5. Continue until all required solutions are found or a valid solution is reached.

### Real-Life Example: Finding a Path in a Maze

Imagine you are inside a maze.

- You choose a direction.
- You continue moving.
- If you reach a dead end, you return to the previous position.
- You try another direction.

This is exactly how backtracking works.

<AsyncImage query="simple black and white maze puzzle start finish path dead ends top view" aspectRatio="16:9" width="100%" />

## 2. Relationship Between Recursion and Backtracking

Recursion and backtracking are related, but they are not the same.

| Recursion | Backtracking |
|---|---|
| A method calls itself | Uses recursion to explore choices |
| Solves smaller versions of a problem | Explores possible solutions |
| Does not always undo choices | Usually reverses changes after exploring |
| Used in tree traversal, factorial, etc. | Used in subsets, permutations, puzzles, etc. |

**Important:** Backtracking is a problem-solving technique that commonly uses recursion. It is not a replacement for recursion.

## 3. Basic Backtracking Structure

A typical backtracking method looks like this:

```java
void backtrack(parameters) {

    // Base case: solution found
    if (solutionFound) {
        // Store or process solution
        return;
    }

    // Explore possible choices
    for (each choice) {

        // Make a choice
        applyChoice();

        // Explore further
        backtrack(updatedParameters);

        // Undo the choice
        undoChoice();
    }
}
```

The most important operation is:

```java
undoChoice();
```

This restores the state so that another choice can be explored independently.

## 4. Example: Generate All Subsets

A subset is a collection of elements selected from an array.

Consider:

```text
Input = [1, 2, 3]
```

All possible subsets are:

```text
[]
[1]
[2]
[3]
[1, 2]
[1, 3]
[2, 3]
[1, 2, 3]
```

For an array of size `n`, the total number of subsets is:

\[
2^n
\]

### Approach: Include or Exclude

For every element, we have two choices:

1. Include the element.
2. Exclude the element.

### Recursion Tree

For input `[1, 2]`:

```text
                    []
                  /    \
             Include 1  Exclude 1
                [1]       []
               /  \      /  \
          Include Exclude Include Exclude
             2       2      2       2
           [1,2]    [1]    [2]     []
```

### Java Implementation

```java
import java.util.*;

public class Subsets {

    static void generateSubsets(
            int[] arr,
            int index,
            List<Integer> current,
            List<List<Integer>> result) {

        // Base case
        if (index == arr.length) {

            result.add(new ArrayList<>(current));

            return;
        }

        // Choice 1: Include current element
        current.add(arr[index]);

        generateSubsets(
                arr,
                index + 1,
                current,
                result
        );

        // Backtrack: Remove the element
        current.remove(current.size() - 1);

        // Choice 2: Exclude current element
        generateSubsets(
                arr,
                index + 1,
                current,
                result
        );
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 3};

        List<List<Integer>> result = new ArrayList<>();

        generateSubsets(
                arr,
                0,
                new ArrayList<>(),
                result
        );

        System.out.println(result);
    }
}
```

**Possible output:**

```text
[[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3], []]
```

The order depends on the order in which choices are explored.

### Important: Why Do We Use `new ArrayList<>(current)`?

We must store a copy of the current subset.

```java
result.add(new ArrayList<>(current));
```

If we instead write:

```java
result.add(current);
```

Every result entry refers to the same mutable list. Later modifications can change previously stored results.

### Complexity

- Time: `O(n × 2^n)` including copying each subset.
- Auxiliary recursion and current-list space: `O(n)`.
- Result storage: `O(n × 2^n)`.

## 5. Example: Generate All Permutations

A permutation is an arrangement of elements in a particular order.

For input:

```text
[1, 2, 3]
```

All permutations are:

```text
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 1, 2]
[3, 2, 1]
```

For `n` distinct elements, the number of permutations is:

\[
n!
\]

### Approach

1. Choose an unused element.
2. Add it to the current permutation.
3. Mark it as used.
4. Recursively explore the remaining elements.
5. Remove the element and mark it unused.

### Java Implementation

```java
import java.util.*;

public class Permutations {

    static void generatePermutations(
            int[] arr,
            boolean[] used,
            List<Integer> current,
            List<List<Integer>> result) {

        // Base case
        if (current.size() == arr.length) {

            result.add(new ArrayList<>(current));

            return;
        }

        for (int i = 0; i < arr.length; i++) {

            if (used[i]) {
                continue;
            }

            // Make a choice
            used[i] = true;
            current.add(arr[i]);

            // Explore
            generatePermutations(
                    arr,
                    used,
                    current,
                    result
            );

            // Undo the choice
            current.remove(current.size() - 1);
            used[i] = false;
        }
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 3};

        boolean[] used = new boolean[arr.length];

        List<List<Integer>> result = new ArrayList<>();

        generatePermutations(
                arr,
                used,
                new ArrayList<>(),
                result
        );

        System.out.println(result);
    }
}
```

**Output:**

```text
[[1, 2, 3], [1, 3, 2], [2, 1, 3],
 [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

### Complexity

- Time: `O(n × n!)` including copying each permutation.
- Auxiliary space: `O(n)` for the recursion path and used array.
- Result storage: `O(n × n!)`.

## 6. Example: Combination Sum

In the Combination Sum problem, we are given an array of positive integers and a target value.

We need to find all combinations whose sum equals the target.

A candidate can be selected more than once.

Example:

```text
Input:
candidates = [2, 3, 6, 7]
target = 7
```

Possible combinations:

```text
[2, 2, 3]
[7]
```

### Approach

At each step:

1. Select a candidate.
2. Subtract it from the remaining target.
3. Explore the next choices.
4. Backtrack when the remaining target becomes invalid.

We pass the current index again after selecting a candidate because the same candidate can be used repeatedly.

### Java Implementation

```java
import java.util.*;

public class CombinationSum {

    static void findCombinations(
            int[] candidates,
            int target,
            int index,
            List<Integer> current,
            List<List<Integer>> result) {

        // Valid combination found
        if (target == 0) {

            result.add(new ArrayList<>(current));

            return;
        }

        // Invalid combination
        if (target < 0) {
            return;
        }

        for (int i = index; i < candidates.length; i++) {

            // Make a choice
            current.add(candidates[i]);

            // Same candidate can be selected again
            findCombinations(
                    candidates,
                    target - candidates[i],
                    i,
                    current,
                    result
            );

            // Backtrack
            current.remove(current.size() - 1);
        }
    }

    public static void main(String[] args) {

        int[] candidates = {2, 3, 6, 7};

        int target = 7;

        List<List<Integer>> result = new ArrayList<>();

        findCombinations(
                candidates,
                target,
                0,
                new ArrayList<>(),
                result
        );

        System.out.println(result);
    }
}
```

**Output:**

```text
[[2, 2, 3], [7]]
```

### Important Difference: Combination Sum vs Permutations

| Combination Sum | Permutations |
|---|---|
| Finds combinations | Finds arrangements |
| Order does not matter | Order matters |
| Same candidate may be reused in Combination Sum I | Each array index is used once in the example |
| Uses an index to avoid duplicate orderings | Uses a `used` array |

### Complexity

The search tree can grow exponentially. Its exact size depends on the candidates and target. Do not assume a single `O(2^n)` bound for this problem because candidates may be reused.

## 7. Example: Combination Sum II

This variation allows each candidate to be used only once.

The input can contain duplicates, but the output must not contain duplicate combinations.

Example:

```text
Input:
candidates = [10, 1, 2, 7, 6, 1, 5]
target = 8
```

Output:

```text
[
  [1, 1, 6],
  [1, 2, 5],
  [1, 7],
  [2, 6]
]
```

### Important Concepts

- Sort the array.
- Move to the next index after choosing an element.
- Skip duplicate values at the same recursion depth.

### Java Implementation

```java
import java.util.*;

public class CombinationSumII {

    static void findCombinations(
            int[] arr,
            int target,
            int start,
            List<Integer> current,
            List<List<Integer>> result) {

        if (target == 0) {

            result.add(new ArrayList<>(current));

            return;
        }

        for (int i = start; i < arr.length; i++) {

            // Skip duplicates at the same recursion depth
            if (i > start && arr[i] == arr[i - 1]) {
                continue;
            }

            // Array is sorted, so no larger value can work
            if (arr[i] > target) {
                break;
            }

            current.add(arr[i]);

            // Move to the next index: each element is used once
            findCombinations(
                    arr,
                    target - arr[i],
                    i + 1,
                    current,
                    result
            );

            current.remove(current.size() - 1);
        }
    }

    public static void main(String[] args) {

        int[] arr = {10, 1, 2, 7, 6, 1, 5};

        Arrays.sort(arr);

        List<List<Integer>> result = new ArrayList<>();

        findCombinations(
                arr,
                8,
                0,
                new ArrayList<>(),
                result
        );

        System.out.println(result);
    }
}
```

**Output:**

```text
[[1, 1, 6], [1, 2, 5], [1, 7], [2, 6]]
```

## 8. Example: Generate Parentheses

We need to generate all combinations of well-formed parentheses for `n` pairs.

Example:

```text
Input: n = 3

Output:
((()))
(()())
(())()
()(())
()()()
```

### Rules

- We can add an opening bracket if the number of opening brackets is less than `n`.
- We can add a closing bracket if the number of closing brackets is less than the number of opening brackets.

### Java Implementation

```java
import java.util.*;

public class GenerateParentheses {

    static void generate(
            int n,
            int open,
            int close,
            String current,
            List<String> result) {

        // Complete valid combination
        if (current.length() == 2 * n) {

            result.add(current);

            return;
        }

        // Add opening bracket
        if (open < n) {

            generate(
                    n,
                    open + 1,
                    close,
                    current + "(",
                    result
            );
        }

        // Add closing bracket
        if (close < open) {

            generate(
                    n,
                    open,
                    close + 1,
                    current + ")",
                    result
            );
        }
    }

    public static void main(String[] args) {

        int n = 3;

        List<String> result = new ArrayList<>();

        generate(n, 0, 0, "", result);

        System.out.println(result);
    }
}
```

**Output:**

```text
[((())), (()()), (())(), ()(()), ()()()]
```

### Why Is `close < open` Important?

A closing bracket cannot be added unless an opening bracket is available to match it.

This condition prevents invalid combinations such as:

```text
)(
```

This is an example of **pruning**: we avoid exploring choices that cannot produce a valid answer.

## 9. Example: Rat in a Maze

In this problem, a rat needs to travel from the starting cell to the destination cell.

- `1` represents an open cell.
- `0` represents a blocked cell.
- The rat can move in four directions.
- A cell cannot be visited more than once in the same path.

Example:

```text
1 0 0 0
1 1 0 1
0 1 0 0
1 1 1 1
```

Start: top-left cell.

Destination: bottom-right cell.

### Approach

1. Start at `(0, 0)`.
2. Check whether the current cell is valid.
3. Mark it as visited in the current path.
4. Try moving in four directions.
5. If the destination is reached, store the path.
6. Backtrack by removing the current cell from the path.

### Java Implementation

```java
import java.util.*;

public class RatInMaze {

    static int n;

    static int[][] directions = {
        {1, 0},   // Down
        {0, -1},  // Left
        {0, 1},   // Right
        {-1, 0}   // Up
    };

    static String[] directionNames = {
        "D", "L", "R", "U"
    };

    static void solve(
            int[][] maze,
            int row,
            int col,
            boolean[][] visited,
            String path,
            List<String> result) {

        // Destination reached
        if (row == n - 1 && col == n - 1) {

            result.add(path);

            return;
        }

        for (int i = 0; i < directions.length; i++) {

            int newRow = row + directions[i][0];
            int newCol = col + directions[i][1];

            // Check boundaries and valid cells
            if (newRow >= 0 &&
                newCol >= 0 &&
                newRow < n &&
                newCol < n &&
                maze[newRow][newCol] == 1 &&
                !visited[newRow][newCol]) {

                // Make a choice
                visited[newRow][newCol] = true;

                // Explore
                solve(
                        maze,
                        newRow,
                        newCol,
                        visited,
                        path + directionNames[i],
                        result
                );

                // Backtrack
                visited[newRow][newCol] = false;
            }
        }
    }

    static List<String> findPaths(int[][] maze) {

        n = maze.length;

        List<String> result = new ArrayList<>();

        if (n == 0 || maze[0][0] == 0 ||
            maze[n - 1][n - 1] == 0) {

            return result;
        }

        boolean[][] visited = new boolean[n][n];

        visited[0][0] = true;

        solve(
                maze,
                0,
                0,
                visited,
                "",
                result
        );

        Collections.sort(result);

        return result;
    }

    public static void main(String[] args) {

        int[][] maze = {
            {1, 0, 0, 0},
            {1, 1, 0, 1},
            {0, 1, 0, 0},
            {1, 1, 1, 1}
        };

        System.out.println(findPaths(maze));
    }
}
```

**Output:**

```text
[DDRDRR]
```

The returned strings represent directions:

- `D` = Down
- `L` = Left
- `R` = Right
- `U` = Up

This example uses a visited matrix for each current path, not a global permanent visited state. This allows a cell to be explored again through a different path.

## 10. Example: N-Queens Problem

The N-Queens problem asks us to place `N` queens on an `N × N` chessboard so that no two queens attack each other.

A queen can attack:

- Horizontally.
- Vertically.
- Diagonally.

Example for `N = 4`:

```text
. Q . .
. . . Q
Q . . .
. . Q .
```

### Approach

We place one queen in each row.

For every row:

1. Try each column.
2. Check whether the position is safe.
3. Place the queen if it is safe.
4. Move to the next row.
5. If no valid placement works, remove the previous queen and try another column.

### Java Implementation

```java
import java.util.*;

public class NQueens {

    static boolean isSafe(
            char[][] board,
            int row,
            int col,
            int n) {

        // Check same column above
        for (int i = 0; i < row; i++) {

            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check upper-left diagonal
        for (int i = row - 1, j = col - 1;
             i >= 0 && j >= 0;
             i--, j--) {

            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Check upper-right diagonal
        for (int i = row - 1, j = col + 1;
             i >= 0 && j < n;
             i--, j++) {

            if (board[i][j] == 'Q') {
                return false;
            }
        }

        return true;
    }

    static void solve(
            char[][] board,
            int row,
            int n,
            List<List<String>> result) {

        // All queens placed
        if (row == n) {

            List<String> solution = new ArrayList<>();

            for (int i = 0; i < n; i++) {
                solution.add(new String(board[i]));
            }

            result.add(solution);

            return;
        }

        for (int col = 0; col < n; col++) {

            if (isSafe(board, row, col, n)) {

                // Make a choice
                board[row][col] = 'Q';

                // Explore next row
                solve(board, row + 1, n, result);

                // Backtrack
                board[row][col] = '.';
            }
        }
    }

    static List<List<String>> solveNQueens(int n) {

        char[][] board = new char[n][n];

        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }

        List<List<String>> result = new ArrayList<>();

        solve(board, 0, n, result);

        return result;
    }

    public static void main(String[] args) {

        System.out.println(solveNQueens(4));
    }
}
```

There are two valid solutions for `N = 4`.

### Complexity

The worst-case search is exponential. A common upper-bound description is `O(N!)` candidate placements, with additional work for safety checks and copying solutions.

The board and recursion state use `O(N²)` space, excluding the returned solutions.

## 11. Pruning in Backtracking

**Pruning means stopping a search path early when it cannot lead to a valid solution.**

Without pruning, the algorithm may explore many unnecessary possibilities.

### Example: Combination Sum

```java
if (target < 0) {
    return;
}
```

If the remaining target becomes negative and all candidates are positive, there is no reason to continue exploring that path.

### Example: Generate Parentheses

```java
if (close > open) {
    return;
}
```

A sequence with more closing brackets than opening brackets can never become valid.

A better approach is to avoid making that invalid choice in the first place.

### Benefits of Pruning

- Reduces unnecessary recursive calls.
- Makes search algorithms faster.
- Avoids exploring invalid states.
- Improves practical performance.

Pruning does not always improve the theoretical worst-case complexity, but it can significantly reduce the actual search space.

## 12. Backtracking vs Brute Force

| Feature | Brute Force | Backtracking |
|---|---|---|
| Approach | Checks possible candidates | Explores choices recursively |
| Invalid paths | May evaluate many complete candidates | Can stop invalid paths early |
| Optimization | Often limited | Uses pruning |
| Common use | Simple exhaustive search | Combinations, permutations, puzzles |

Backtracking is still a form of search. It becomes more efficient when we can identify and reject invalid partial solutions early.

## 13. Common Backtracking Mistakes

1. **Forgetting to undo a choice**

   Always restore the state after recursive exploration when the state is shared and mutable.

2. **Not copying the current solution**

   Use:

   ```java
   result.add(new ArrayList<>(current));
   ```

3. **Incorrect base case**

   This may cause missing or invalid solutions.

4. **Not handling duplicate elements**

   Sorting and skipping duplicates at the same recursion depth can prevent duplicate combinations.

5. **Incorrect index management**

   Combination problems often use a starting index to avoid generating the same combination in different orders.

6. **Using the wrong reuse rule**

   - Reuse allowed: recurse with the same index.
   - Reuse not allowed: recurse with the next index.

7. **Not pruning invalid paths**

   Check whether a partial solution can still become valid.

8. **Using a global visited array incorrectly**

   In path-enumeration problems, a cell may need to become available again after backtracking.

## 14. Important Backtracking Interview Questions

### Beginner

1. What is backtracking?
2. What is the difference between recursion and backtracking?
3. Why do we undo a choice?
4. What is pruning?
5. What is a recursion tree?
6. How do you generate all subsets of an array?

### Intermediate

1. Generate all permutations of an array.
2. Solve Combination Sum.
3. Generate all valid parentheses.
4. Solve the Rat in a Maze problem.
5. Find all combinations that sum to a target.
6. How do you avoid duplicate subsets?
7. What is the difference between Combination Sum I and II?

### Advanced

1. Explain the N-Queens problem.
2. How do you solve Sudoku using backtracking?
3. How do you optimize backtracking with pruning?
4. What is the difference between backtracking and Dynamic Programming?
5. How do you estimate the complexity of a backtracking algorithm?
6. How can bit manipulation optimize N-Queens?

## 15. Practice Problems

### Easy

1. [Subsets](https://leetcode.com/problems/subsets/)
2. [Binary Watch](https://leetcode.com/problems/binary-watch/)

### Medium

1. [Permutations](https://leetcode.com/problems/permutations/)
2. [Combinations](https://leetcode.com/problems/combinations/)
3. [Combination Sum](https://leetcode.com/problems/combination-sum/)
4. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
5. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
6. [Word Search](https://leetcode.com/problems/word-search/)
7. [Subsets II](https://leetcode.com/problems/subsets-ii/)

### Hard

1. [N-Queens](https://leetcode.com/problems/n-queens/)
2. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
3. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## 16. Quick Revision

| Concept | Meaning |
|---|---|
| Backtracking | Explore choices and undo them |
| Choice | Select an available option |
| Explore | Make recursive calls |
| Undo | Restore the previous state |
| Pruning | Stop exploring invalid paths |
| Subsets | Include or exclude elements |
| Permutations | Arrange elements in different orders |
| Combinations | Select elements without considering order |
| N-Queens | Place queens without attacks |
| Maze solving | Explore valid paths |

### Common Patterns

**Subsets:**

```java
current.add(arr[index]);

backtrack(index + 1);

current.remove(current.size() - 1);

backtrack(index + 1);
```

**Permutations:**

```java
used[i] = true;
current.add(arr[i]);

backtrack();

current.remove(current.size() - 1);
used[i] = false;
```

**Combination Sum with reuse:**

```java
current.add(arr[i]);

backtrack(i);

current.remove(current.size() - 1);
```

**Combination Sum without reuse:**

```java
current.add(arr[i]);

backtrack(i + 1);

current.remove(current.size() - 1);
```

## 17. Learning Checklist

- [ ] Understand the backtracking approach.
- [ ] Understand choice, exploration, and undo.
- [ ] Understand recursion trees.
- [ ] Generate all subsets.
- [ ] Generate all permutations.
- [ ] Solve Combination Sum.
- [ ] Solve Combination Sum II.
- [ ] Generate valid parentheses.
- [ ] Solve Rat in a Maze.
- [ ] Understand N-Queens.
- [ ] Understand pruning.
- [ ] Practice at least five backtracking problems.

---

## Conclusion

Backtracking is one of the most important techniques for solving problems that require exploring multiple possibilities.

Remember this pattern:

**Choose → Explore → Undo**

Once you understand this pattern, you can solve subsets, permutations, combinations, maze problems, N-Queens, Sudoku, and many other interview problems.