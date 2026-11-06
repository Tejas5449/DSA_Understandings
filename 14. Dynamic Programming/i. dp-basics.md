# Dynamic Programming Basics in Java

## 1. What is Dynamic Programming?

**Dynamic Programming (DP)** is an algorithmic technique used to solve complex problems by breaking them into smaller overlapping subproblems and storing their results so that the same calculations are not repeated.

The main idea is:

1. Break a problem into smaller subproblems.
2. Solve each subproblem.
3. Store the result.
4. Reuse the stored result whenever the same subproblem appears again.

Dynamic Programming helps improve the performance of many recursive solutions.

### Real-World Example

Imagine you are calculating the Fibonacci number:

```text
Fibonacci(5)
```

To calculate it, you need:

```text
Fibonacci(5) = Fibonacci(4) + Fibonacci(3)
```

But while calculating `Fibonacci(4)`, you also calculate `Fibonacci(3)`.

This means the same subproblem is calculated multiple times.

Dynamic Programming avoids this repeated work by storing previously calculated results.

---

## 2. What is the Fibonacci Sequence?

The Fibonacci sequence is a sequence where each number is the sum of the previous two numbers.

The sequence starts with:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
```

Mathematical definition:

\[
F(n) =
\begin{cases}
0 & \text{if } n = 0 \\
1 & \text{if } n = 1 \\
F(n-1) + F(n-2) & \text{if } n \geq 2
\end{cases}
\]

Examples:

```text
F(0) = 0
F(1) = 1
F(2) = 1
F(3) = 2
F(4) = 3
F(5) = 5
F(6) = 8
```

Fibonacci is a useful example for understanding Dynamic Programming.

---

## 3. Fibonacci Using Normal Recursion

Let's first solve Fibonacci using recursion without Dynamic Programming.

### Java Implementation

```java
public class FibonacciRecursion {

    public static int fibonacci(int n) {

        if (n <= 1) {
            return n;
        }

        return fibonacci(n - 1) + fibonacci(n - 2);
    }

    public static void main(String[] args) {

        int n = 5;

        System.out.println(fibonacci(n));
    }
}
```

Output:

```text
5
```

### How It Works

For `fibonacci(5)`:

```text
fib(5)
   |
   +---- fib(4)
   |       |
   |       +---- fib(3)
   |       |       |
   |       |       +---- fib(2)
   |       |       +---- fib(1)
   |       |
   |       +---- fib(2)
   |
   +---- fib(3)
           |
           +---- fib(2)
           +---- fib(1)
```

Notice that:

- `fib(3)` is calculated more than once.
- `fib(2)` is calculated multiple times.

This repeated calculation is called **overlapping subproblems**.

### Time Complexity

The time complexity is:

\[
O(2^n)
\]

This is because each recursive call can generate two additional calls.

More precisely, the number of calls grows exponentially.

### Space Complexity

\[
O(n)
\]

The maximum recursion depth is proportional to `n`.

**Problem:** Normal recursion repeats calculations, making it inefficient for larger values of `n`.

---

## 4. What Are the Two Main Properties of Dynamic Programming?

A problem is a good candidate for Dynamic Programming when it has these properties.

### 4.1 Overlapping Subproblems

A problem has overlapping subproblems when the same smaller problems are solved repeatedly.

Example:

```text
fib(5)
    |
    +---- fib(4)
    |       |
    |       +---- fib(3)
    |       +---- fib(2)
    |
    +---- fib(3)
            |
            +---- fib(2)
            +---- fib(1)
```

Here, `fib(3)` and `fib(2)` are calculated more than once.

Dynamic Programming stores their results and reuses them.

### 4.2 Optimal Substructure

A problem has optimal substructure when an optimal solution can be constructed from optimal solutions to its smaller subproblems.

For example, in the Fibonacci problem:

```text
fib(n) = fib(n - 1) + fib(n - 2)
```

The answer depends on the answers to smaller Fibonacci problems.

Another example is finding the minimum cost to reach the last step in a staircase. The answer can be constructed from the minimum costs of reaching earlier steps.

**Remember:**

- Overlapping subproblems → Repeated calculations.
- Optimal substructure → A solution can be built using solutions to smaller problems.

---

## 5. Two Main Approaches to Dynamic Programming

There are two commonly used approaches:

1. Memoization (Top-Down)
2. Tabulation (Bottom-Up)

Both approaches store previously calculated results.

---

## 6. Memoization (Top-Down Approach)

Memoization is a Dynamic Programming technique where we use recursion and store the results of previously solved subproblems.

The process:

1. Start with the original problem.
2. Break it into smaller recursive problems.
3. Before calculating a problem, check whether its result is already stored.
4. If stored, return the result.
5. Otherwise, calculate it and save the result.

### Fibonacci Using Memoization

```java
import java.util.Arrays;

public class FibonacciMemoization {

    public static int fibonacci(int n, int[] dp) {

        // Base condition
        if (n <= 1) {
            return n;
        }

        // Check whether result is already calculated
        if (dp[n] != -1) {
            return dp[n];
        }

        // Calculate and store the result
        dp[n] = fibonacci(n - 1, dp)
              + fibonacci(n - 2, dp);

        return dp[n];
    }

    public static void main(String[] args) {

        int n = 5;

        int[] dp = new int[n + 1];

        Arrays.fill(dp, -1);

        int result = fibonacci(n, dp);

        System.out.println(result);
    }
}
```

Output:

```text
5
```

### Understanding the Code

**Step 1: Create a DP array**

```java
int[] dp = new int[n + 1];
```

The array stores previously calculated Fibonacci values.

**Step 2: Initialize with -1**

```java
Arrays.fill(dp, -1);
```

We use `-1` to indicate that a value has not been calculated.

**Step 3: Check whether the result exists**

```java
if (dp[n] != -1) {
    return dp[n];
}
```

If the result is already stored, reuse it.

**Step 4: Calculate and store**

```java
dp[n] = fibonacci(n - 1, dp)
      + fibonacci(n - 2, dp);
```

The result is saved for future use.

### Dry Run

For:

```text
fibonacci(5)
```

The calculations are:

```text
fib(0) = 0
fib(1) = 1
fib(2) = 1
fib(3) = 2
fib(4) = 3
fib(5) = 5
```

The DP array becomes:

```text
Index:  0  1  2  3  4  5
Value:  0  1  1  2  3  5
```

If `fib(3)` is requested again, its stored value is returned instead of recalculating it.

### Complexity

| Complexity | Value |
|---|---|
| Time | O(n) |
| Auxiliary space | O(n) for DP array and recursion stack |

Memoization improves the Fibonacci solution from exponential time to linear time.

---

## 7. Tabulation (Bottom-Up Approach)

Tabulation solves smaller subproblems first and uses their results to calculate larger problems.

Unlike memoization, it usually does not use recursion.

The process:

1. Create a DP array.
2. Initialize the base cases.
3. Use a loop to calculate the remaining values.
4. Return the final result.

### Fibonacci Using Tabulation

```java
public class FibonacciTabulation {

    public static int fibonacci(int n) {

        if (n <= 1) {
            return n;
        }

        int[] dp = new int[n + 1];

        // Base cases
        dp[0] = 0;
        dp[1] = 1;

        // Calculate remaining values
        for (int i = 2; i <= n; i++) {

            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }

    public static void main(String[] args) {

        int n = 5;

        System.out.println(fibonacci(n));
    }
}
```

Output:

```text
5
```

### Dry Run

Input:

```text
n = 5
```

Initial DP array:

```text
[0, 1, 0, 0, 0, 0]
```

Iteration 1:

```text
i = 2

dp[2] = dp[1] + dp[0]
      = 1 + 0
      = 1

DP = [0, 1, 1, 0, 0, 0]
```

Iteration 2:

```text
i = 3

dp[3] = dp[2] + dp[1]
      = 1 + 1
      = 2

DP = [0, 1, 1, 2, 0, 0]
```

Iteration 3:

```text
i = 4

dp[4] = dp[3] + dp[2]
      = 2 + 1
      = 3

DP = [0, 1, 1, 2, 3, 0]
```

Iteration 4:

```text
i = 5

dp[5] = dp[4] + dp[3]
      = 3 + 2
      = 5

DP = [0, 1, 1, 2, 3, 5]
```

Final answer:

```text
5
```

### Complexity

| Complexity | Value |
|---|---|
| Time | O(n) |
| Auxiliary space | O(n) for DP array |

Tabulation avoids the recursive call stack.

---

## 8. Memoization vs Tabulation

| Feature | Memoization | Tabulation |
|---|---|---|
| Approach | Top-Down | Bottom-Up |
| Technique | Recursion | Iteration |
| Calculation | On demand | Builds states in order |
| DP storage | Usually required | Usually required |
| Recursion stack | Yes | No |
| Time for Fibonacci | O(n) | O(n) |
| DP array space | O(n) | O(n) |
| Additional stack space | O(n) | O(1) |

### When Should You Use Memoization?

Use memoization when:

- The recursive solution is easy to understand.
- Only a portion of possible states may be needed.
- You want to convert an existing recursive solution into DP.

### When Should You Use Tabulation?

Use tabulation when:

- You know the order in which states should be calculated.
- You want to avoid recursion stack usage.
- You want an iterative solution.

---

## 9. Space Optimization in Dynamic Programming

Sometimes we do not need to store every previously calculated value.

For Fibonacci, each number only depends on the previous two numbers.

Instead of using a complete DP array, we can use two variables.

### Optimized Fibonacci Solution

```java
public class FibonacciOptimized {

    public static int fibonacci(int n) {

        if (n <= 1) {
            return n;
        }

        int prev2 = 0;
        int prev1 = 1;

        for (int i = 2; i <= n; i++) {

            int current = prev1 + prev2;

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }

    public static void main(String[] args) {

        int n = 5;

        System.out.println(fibonacci(n));
    }
}
```

Output:

```text
5
```

### How It Works

We maintain:

- `prev2`: Fibonacci value from two positions earlier.
- `prev1`: Fibonacci value from the previous position.
- `current`: Newly calculated Fibonacci value.

Example:

```text
Initial:
prev2 = 0
prev1 = 1

i = 2:
current = 0 + 1 = 1

i = 3:
current = 1 + 1 = 2

i = 4:
current = 1 + 2 = 3

i = 5:
current = 2 + 3 = 5
```

### Complexity

| Complexity | Value |
|---|---|
| Time | O(n) |
| Auxiliary space | O(1) |

This is called **Space Optimization**.

**Important:** This implementation uses Java `int`, which overflows for sufficiently large Fibonacci values. Use `long` for a larger range or `BigInteger` when arbitrary precision is required.

---

## 10. Comparison of All Fibonacci Approaches

| Approach | Time Complexity | Auxiliary Space |
|---|---|---|
| Normal Recursion | O(2ⁿ) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | O(1) |

For ordinary Fibonacci calculations, the space-optimized approach is the most memory-efficient of these implementations.

---

## 11. How to Identify a Dynamic Programming Problem

Not every problem needs Dynamic Programming.

Look for these common signs:

### 1. The Problem Asks for Minimum or Maximum

Examples:

- Minimum cost to reach the destination.
- Maximum profit.
- Minimum number of coins.
- Maximum sum of non-adjacent elements.

### 2. The Problem Asks for the Number of Ways

Examples:

- Number of ways to climb stairs.
- Number of ways to reach a destination.
- Number of ways to make a target amount.

### 3. The Problem Involves Choices

Examples:

- Take an element or skip it.
- Buy or sell a stock.
- Include or exclude an item.
- Move left or move right.

### 4. The Same Subproblems Appear Repeatedly

If a recursive solution calculates the same states repeatedly, DP may help.

### 5. The Problem Has Optimal Substructure

If the final answer can be built from answers to smaller subproblems, DP may be applicable.

**Interview tip:** A minimum, maximum, or counting problem is not automatically a DP problem. First identify the states and recurrence, then verify whether subproblems overlap and whether the smaller solutions correctly combine into the larger solution.

---

## 12. A Step-by-Step Framework for Solving DP Problems

Use this framework when approaching a new DP problem.

### Step 1: Understand the Problem

Identify:

- What is the input?
- What is the expected output?
- Are we finding a minimum, maximum, or number of ways?
- What choices can we make?

### Step 2: Define the DP State

A DP state represents a subproblem.

For Fibonacci:

```text
dp[i] = Fibonacci number at index i
```

For the Climbing Stairs problem:

```text
dp[i] = Number of ways to reach stair i
```

### Step 3: Find the Recurrence Relation

A recurrence relation describes how the current state depends on smaller states.

For Fibonacci:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

For Climbing Stairs:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

### Step 4: Identify Base Cases

Base cases are the smallest problems whose answers are already known.

For Fibonacci:

```text
dp[0] = 0
dp[1] = 1
```

### Step 5: Decide the Approach

Choose:

- Memoization (Top-Down)
- Tabulation (Bottom-Up)

### Step 6: Optimize Space If Possible

Check whether the current state needs the entire DP array or only a few previous states.

### Step 7: Analyze Complexity

Calculate:

- Time complexity.
- DP storage space.
- Recursion stack space, if applicable.

---

## 13. First Classic DP Problem: Climbing Stairs

Problem:

You are climbing a staircase with `n` steps.

You can climb either:

- 1 step at a time.
- 2 steps at a time.

Find the number of distinct ways to reach the top.

### Example 1

Input:

```text
n = 2
```

Possible ways:

```text
1 + 1
2
```

Output:

```text
2
```

### Example 2

Input:

```text
n = 3
```

Possible ways:

```text
1 + 1 + 1
1 + 2
2 + 1
```

Output:

```text
3
```

### Step 1: Define the DP State

```text
dp[i] = Number of ways to reach step i
```

### Step 2: Find the Recurrence

To reach step `i`, we can:

- Come from step `i - 1` by taking one step.
- Come from step `i - 2` by taking two steps.

Therefore:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

### Step 3: Base Cases

```text
dp[0] = 1
dp[1] = 1
```

`dp[0] = 1` represents one way to stay at the starting position: take no steps.

### Step 4: Java Implementation Using Tabulation

```java
public class ClimbingStairs {

    public static int climbStairs(int n) {

        if (n <= 1) {
            return 1;
        }

        int[] dp = new int[n + 1];

        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {

            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }

    public static void main(String[] args) {

        int n = 5;

        System.out.println(climbStairs(n));
    }
}
```

Output:

```text
8
```

### Dry Run

Input:

```text
n = 5
```

| Step | Calculation | Number of Ways |
|---|---|---:|
| 0 | Base case | 1 |
| 1 | Base case | 1 |
| 2 | dp[1] + dp[0] | 2 |
| 3 | dp[2] + dp[1] | 3 |
| 4 | dp[3] + dp[2] | 5 |
| 5 | dp[4] + dp[3] | 8 |

Final DP array:

```text
[1, 1, 2, 3, 5, 8]
```

Answer:

```text
8
```

### Complexity

- Time: O(n)
- Auxiliary space: O(n)

### Space-Optimized Java Implementation

```java
public class ClimbingStairsOptimized {

    public static int climbStairs(int n) {

        if (n <= 1) {
            return 1;
        }

        int prev2 = 1;
        int prev1 = 1;

        for (int i = 2; i <= n; i++) {

            int current = prev1 + prev2;

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }

    public static void main(String[] args) {

        System.out.println(climbStairs(5));
    }
}
```

Complexity:

- Time: O(n)
- Auxiliary space: O(1)

**Note:** These `int` implementations are suitable for small inputs. For large values, the number of ways can exceed the `int` range.

---

## 14. Common Dynamic Programming Patterns

As you solve more problems, you will start recognizing common DP patterns.

| Pattern | Example Problems |
|---|---|
| 1D DP | Fibonacci, Climbing Stairs |
| 2D DP | Unique Paths, Grid Problems |
| Knapsack DP | 0/1 Knapsack, Subset Sum |
| Subsequence DP | Longest Increasing Subsequence |
| String DP | Longest Common Subsequence |
| Interval DP | Matrix Chain Multiplication |
| DP on Trees | Diameter and Tree DP problems |
| DP with Bitmasking | Traveling Salesperson Problem |

We will learn these patterns in detail in the upcoming chapters.

---

## 15. Common Mistakes in Dynamic Programming

### Mistake 1: Not Defining the DP State Clearly

Before writing code, understand what `dp[i]` represents.

For example:

```text
dp[i] = Minimum cost to reach index i
```

This is more useful than simply creating an array named `dp`.

### Mistake 2: Forgetting Base Cases

Incorrect base cases can make the entire solution wrong.

Always identify the smallest valid subproblems first.

### Mistake 3: Using Incorrect Recurrence Relations

Make sure every possible valid choice is considered.

For Climbing Stairs:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

### Mistake 4: Incorrect Loop Boundaries

For an array of size `n + 1`, valid indexes range from `0` to `n`.

Check whether your loop should use `< n` or `<= n`.

### Mistake 5: Confusing Memoization and Tabulation

Remember:

- Memoization uses recursion and caching.
- Tabulation uses iteration and builds results from smaller states.

### Mistake 6: Ignoring Space Optimization

If a state depends only on the previous two states, you may not need an entire DP array.

### Mistake 7: Integer Overflow

The number of ways or calculated values may become larger than the Java `int` range.

Use `long` or `BigInteger` when appropriate, depending on the problem constraints.

---

## 16. Frequently Asked Interview Questions

### Q1. What is Dynamic Programming?

Dynamic Programming is a technique that solves problems by storing and reusing results of overlapping subproblems.

### Q2. What are the two main properties of DP?

1. Overlapping subproblems.
2. Optimal substructure.

### Q3. What is Memoization?

Memoization is a top-down DP technique that uses recursion and stores calculated results.

### Q4. What is Tabulation?

Tabulation is a bottom-up DP technique that calculates smaller subproblems first using iteration.

### Q5. What is the difference between Recursion and Dynamic Programming?

Normal recursion may repeatedly calculate the same subproblems.

Dynamic Programming stores results and reuses them to avoid repeated calculations.

### Q6. What is the difference between Memoization and Tabulation?

Memoization uses recursion and calculates states as needed.

Tabulation uses iteration and builds states in a predefined order.

### Q7. What is Space Optimization?

Space Optimization reduces memory usage by storing only the previous states required to calculate the current state.

### Q8. What is a DP State?

A DP state represents a smaller subproblem whose result is stored for reuse.

### Q9. What is a Recurrence Relation?

A recurrence relation describes how the result of a current state depends on smaller states.

### Q10. Can every recursive problem be solved using DP?

No. DP is especially useful when the problem has overlapping subproblems and suitable optimal substructure. Some recursive problems do not benefit from storing previous results.

### Q11. Why is Fibonacci recursion inefficient?

Because it recalculates the same Fibonacci values multiple times, resulting in exponential time complexity.

### Q12. What is the time complexity of Fibonacci using Memoization?

O(n), because each Fibonacci state is calculated at most once.

---

## 17. Practice Problems

### Beginner

1. Fibonacci Number.
2. Climbing Stairs.
3. Min Cost Climbing Stairs.
4. Tribonacci Number.
5. Count ways to reach the nth step.
6. Maximum sum of non-adjacent elements.

### Intermediate

1. House Robber.
2. House Robber II.
3. Maximum Product Subarray.
4. Decode Ways.
5. Unique Paths.
6. Minimum Path Sum.

### Recommended LeetCode Problems

| Problem | Difficulty |
|---|---|
| [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | Easy |
| [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Easy |
| [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | Easy |
| [N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/) | Easy |
| [House Robber](https://leetcode.com/problems/house-robber/) | Medium |
| [House Robber II](https://leetcode.com/problems/house-robber-ii/) | Medium |
| [Unique Paths](https://leetcode.com/problems/unique-paths/) | Medium |
| [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | Medium |

---

## 18. Quick Revision

- Dynamic Programming stores and reuses results of smaller subproblems.
- It is useful when overlapping subproblems and optimal substructure are present.
- Memoization is the Top-Down approach.
- Tabulation is the Bottom-Up approach.
- A DP state represents a smaller subproblem.
- A recurrence relation describes how states depend on one another.
- Base cases provide the starting values.
- Space optimization reduces unnecessary storage.
- Fibonacci recursion takes O(2ⁿ) time.
- Fibonacci with Memoization takes O(n) time.
- Fibonacci with Tabulation takes O(n) time.
- Space-optimized Fibonacci takes O(n) time and O(1) auxiliary space.

---

## 19. Self-Assessment Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain Dynamic Programming.
- [ ] Explain overlapping subproblems.
- [ ] Explain optimal substructure.
- [ ] Identify a DP state.
- [ ] Write a recurrence relation.
- [ ] Identify base cases.
- [ ] Implement Fibonacci using recursion.
- [ ] Implement Fibonacci using memoization.
- [ ] Implement Fibonacci using tabulation.
- [ ] Optimize Fibonacci space complexity.
- [ ] Solve Climbing Stairs using DP.
- [ ] Explain time and space complexity.
- [ ] Solve at least three beginner DP problems.

---

## 20. What's Next?

Next, we will learn **1D Dynamic Programming**.

File: `14-dynamic-programming/1d-dp.md`

We will cover:

- What is 1D DP?
- DP state and transition.
- Climbing Stairs.
- Min Cost Climbing Stairs.
- Frog Jump.
- House Robber.
- Maximum sum of non-adjacent elements.
- Space optimization.
- Java implementations and dry runs.