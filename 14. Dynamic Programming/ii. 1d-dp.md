# 1D Dynamic Programming in Java

## 1. What is 1D Dynamic Programming?

**1D Dynamic Programming** is a DP approach where we use a one-dimensional array (or a small number of variables) to store the results of subproblems.

Each state is usually represented by a single index.

For example:

```java
int[] dp = new int[n + 1];
```

Here:

```text
dp[i] = Answer to the subproblem at index i
```

The answer for the current state is calculated using one or more previously calculated states.

### Common Examples

- Fibonacci Number
- Climbing Stairs
- Min Cost Climbing Stairs
- Frog Jump
- House Robber
- Maximum Sum of Non-Adjacent Elements

---

## 2. How to Solve a 1D DP Problem

Follow these five steps.

1. Define the DP state.
2. Identify the base cases.
3. Find the recurrence relation.
4. Decide the order of calculation.
5. Optimize space if possible.

Let's understand these steps through problems.

---

## 3. Problem 1: Climbing Stairs

**Problem:**

You are climbing a staircase with `n` steps.

You can climb either 1 step or 2 steps at a time.

Find the number of distinct ways to reach the top.

### Example

Input:

```text
n = 4
```

Possible ways:

```text
1 + 1 + 1 + 1
1 + 1 + 2
1 + 2 + 1
2 + 1 + 1
2 + 2
```

Output:

```text
5
```

### Step 1: Define the DP State

```text
dp[i] = Number of ways to reach step i
```

### Step 2: Identify Base Cases

```text
dp[0] = 1
dp[1] = 1
```

There is one way to remain at the starting position: take no steps.

### Step 3: Find the Recurrence Relation

To reach step `i`, we can:

- Come from step `i - 1` by taking one step.
- Come from step `i - 2` by taking two steps.

Therefore:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

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

        System.out.println(climbStairs(4));
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
n = 4
```

| Step | Calculation | Result |
|---|---|---:|
| 0 | Base case | 1 |
| 1 | Base case | 1 |
| 2 | dp[1] + dp[0] | 2 |
| 3 | dp[2] + dp[1] | 3 |
| 4 | dp[3] + dp[2] | 5 |

Final DP array:

```text
[1, 1, 2, 3, 5]
```

### Space-Optimized Java Solution

We only need the previous two values.

```java
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
```

Complexity:

- Time: O(n)
- Auxiliary space: O(1)

---

## 4. Problem 2: Min Cost Climbing Stairs

**Problem:**

You are given an integer array `cost`, where `cost[i]` represents the cost of stepping on stair `i`.

After paying the cost of a stair, you can climb either one or two steps.

You can start from index `0` or index `1`.

Find the minimum cost to reach the top, which is one position beyond the last stair.

### Example

Input:

```java
int[] cost = {10, 15, 20};
```

Possible ways:

**Option 1:**

```text
Start at index 0
Pay 10
Move two steps to the top
Total cost = 10
```

**Option 2:**

```text
Start at index 1
Pay 15
Move two steps to the top
Total cost = 15
```

Minimum cost:

```text
10
```

### Step 1: Define the DP State

```text
dp[i] = Minimum cost required to reach stair i
```

For the top position, the answer is `dp[n]`.

### Step 2: Base Cases

We can start at index `0` or index `1`.

```text
dp[0] = cost[0]
dp[1] = cost[1]
```

### Step 3: Recurrence Relation

To reach stair `i`, we can come from:

- Stair `i - 1`
- Stair `i - 2`

Therefore:

```text
dp[i] = cost[i] + min(dp[i - 1], dp[i - 2])
```

The top position has no additional stair cost, so:

```text
answer = min(dp[n - 1], dp[n - 2])
```

### Java Implementation

```java
public class MinCostClimbingStairs {

    public static int minCostClimbingStairs(int[] cost) {

        int n = cost.length;

        if (n == 2) {
            return Math.min(cost[0], cost[1]);
        }

        int[] dp = new int[n];

        dp[0] = cost[0];
        dp[1] = cost[1];

        for (int i = 2; i < n; i++) {

            dp[i] = cost[i]
                    + Math.min(dp[i - 1], dp[i - 2]);
        }

        return Math.min(dp[n - 1], dp[n - 2]);
    }

    public static void main(String[] args) {

        int[] cost = {10, 15, 20};

        System.out.println(
                minCostClimbingStairs(cost)
        );
    }
}
```

Output:

```text
15
```

**Why is the output 15?**

The minimum cost to reach the top is 15: start at stair index `1`, pay 15, and take two steps to the top.

### Dry Run

Input:

```text
cost = [10, 15, 20]
```

| Index | Cost | Calculation | DP Value |
|---|---:|---|---:|
| 0 | 10 | Base case | 10 |
| 1 | 15 | Base case | 15 |
| 2 | 20 | 20 + min(10, 15) | 30 |

Final result:

```text
min(dp[2], dp[1])
= min(30, 15)
= 15
```

### Space-Optimized Solution

```java
public static int minCostClimbingStairs(int[] cost) {

    int n = cost.length;

    int prev2 = cost[0];
    int prev1 = cost[1];

    for (int i = 2; i < n; i++) {

        int current = cost[i]
                + Math.min(prev1, prev2);

        prev2 = prev1;
        prev1 = current;
    }

    return Math.min(prev1, prev2);
}
```

Complexity:

- Time: O(n)
- Auxiliary space: O(1)

The implementation assumes the problem provides at least two stairs.

---

## 5. Problem 3: Frog Jump

**Problem:**

A frog is standing at the first stone of an array of heights.

It wants to reach the last stone.

The frog can jump:

- One stone forward.
- Two stones forward.

The energy used for a jump from stone `i` to stone `j` is:

```text
abs(height[i] - height[j])
```

Find the minimum energy required to reach the last stone.

### Example

Input:

```java
int[] heights = {10, 20, 30, 10};
```

Possible paths:

**Path 1:**

```text
10 -> 20 -> 30 -> 10

Energy = |10 - 20| + |20 - 30| + |30 - 10|
       = 10 + 10 + 20
       = 40
```

**Path 2:**

```text
10 -> 30 -> 10

Energy = |10 - 30| + |30 - 10|
       = 20 + 20
       = 40
```

Minimum energy:

```text
40
```

### Step 1: Define the DP State

```text
dp[i] = Minimum energy required to reach stone i
```

### Step 2: Base Case

```text
dp[0] = 0
```

The frog is already at the first stone, so no energy is required.

### Step 3: Recurrence Relation

The frog can reach stone `i` from:

- Stone `i - 1`
- Stone `i - 2`

Cost from the previous stone:

```text
oneStep = dp[i - 1]
        + abs(heights[i] - heights[i - 1])
```

Cost from two stones earlier:

```text
twoSteps = dp[i - 2]
         + abs(heights[i] - heights[i - 2])
```

Choose the minimum:

```text
dp[i] = min(oneStep, twoSteps)
```

### Java Implementation

```java
public class FrogJump {

    public static int frogJump(int[] heights) {

        int n = heights.length;

        if (n <= 1) {
            return 0;
        }

        int[] dp = new int[n];

        dp[0] = 0;

        for (int i = 1; i < n; i++) {

            int oneStep = dp[i - 1]
                    + Math.abs(
                            heights[i] - heights[i - 1]
                    );

            int twoSteps = Integer.MAX_VALUE;

            if (i > 1) {

                twoSteps = dp[i - 2]
                        + Math.abs(
                                heights[i] - heights[i - 2]
                        );
            }

            dp[i] = Math.min(oneStep, twoSteps);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {

        int[] heights = {10, 20, 30, 10};

        System.out.println(frogJump(heights));
    }
}
```

Output:

```text
40
```

### Dry Run

Input:

```text
heights = [10, 20, 30, 10]
```

| Index | Height | One-Step Cost | Two-Step Cost | DP Value |
|---|---:|---:|---:|---:|
| 0 | 10 | 0 | Not applicable | 0 |
| 1 | 20 | 10 | Not applicable | 10 |
| 2 | 30 | 20 | 20 | 20 |
| 3 | 10 | 40 | 30 | 30 |

Correct calculation at index 3:

- From index 2: `20 + |10 - 30| = 40`
- From index 1: `10 + |10 - 20| = 20`

Therefore, the minimum is `20`.

The correct final DP array is:

```text
[0, 10, 20, 20]
```

Output:

```text
20
```

### Space-Optimized Solution

Only the previous two DP values are required.

```java
public static int frogJumpOptimized(int[] heights) {

    int n = heights.length;

    if (n <= 1) {
        return 0;
    }

    int prev2 = 0;
    int prev1 = Math.abs(heights[1] - heights[0]);

    for (int i = 2; i < n; i++) {

        int oneStep = prev1
                + Math.abs(heights[i] - heights[i - 1]);

        int twoSteps = prev2
                + Math.abs(heights[i] - heights[i - 2]);

        int current = Math.min(oneStep, twoSteps);

        prev2 = prev1;
        prev1 = current;
    }

    return prev1;
}
```

Complexity:

- Time: O(n)
- Auxiliary space: O(1)

---

## 6. Problem 4: Frog Jump with K Distances

This is an extension of the Frog Jump problem.

**Problem:**

A frog can jump from its current stone to any of the next `k` stones.

The energy used for a jump from stone `i` to stone `j` is:

```text
abs(height[i] - height[j])
```

Find the minimum energy required to reach the last stone.

### Example

Input:

```java
int[] heights = {10, 30, 40, 50, 20};

int k = 3;
```

The frog can jump up to three stones forward.

### DP State

```text
dp[i] = Minimum energy required to reach stone i
```

### Base Case

```text
dp[0] = 0
```

### Recurrence Relation

For every stone `i`, try all valid previous jump positions.

```text
dp[i] = min(
    dp[i - j] + abs(heights[i] - heights[i - j])
)
```

Where:

```text
1 <= j <= k
```

And:

```text
i - j >= 0
```

### Java Implementation

```java
import java.util.Arrays;

public class FrogJumpK {

    public static int frogJump(int[] heights, int k) {

        int n = heights.length;

        if (n <= 1) {
            return 0;
        }

        int[] dp = new int[n];

        Arrays.fill(dp, Integer.MAX_VALUE);

        dp[0] = 0;

        for (int i = 1; i < n; i++) {

            for (int j = 1; j <= k && i - j >= 0; j++) {

                int jumpCost = dp[i - j]
                        + Math.abs(
                                heights[i] - heights[i - j]
                        );

                dp[i] = Math.min(dp[i], jumpCost);
            }
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {

        int[] heights = {10, 30, 40, 50, 20};

        int k = 3;

        System.out.println(frogJump(heights, k));
    }
}
```

Output:

```text
30
```

### Understanding the Nested Loop

```java
for (int j = 1; j <= k && i - j >= 0; j++)
```

This checks all possible previous stones from which the frog can jump to the current stone.

For example, if:

```text
i = 4
k = 3
```

The frog can come from:

```text
Index 3
Index 2
Index 1
```

The algorithm calculates the energy for each valid jump and stores the minimum.

### Complexity

- Time: O(n × k)
- Auxiliary space: O(n)

---

## 7. Problem 5: House Robber

**Problem:**

A robber wants to steal money from houses arranged in a row.

Each house contains a certain amount of money.

The robber cannot steal from two adjacent houses.

Find the maximum amount of money that can be stolen.

### Example

Input:

```java
int[] nums = {2, 7, 9, 3, 1};
```

Possible selections:

```text
House 0 + House 2 + House 4

2 + 9 + 1 = 12
```

Another selection:

```text
House 1 + House 3

7 + 3 = 10
```

Maximum amount:

```text
12
```

### Step 1: Define the DP State

```text
dp[i] = Maximum money that can be stolen
        from houses 0 to i
```

### Step 2: Identify Base Cases

For one house:

```text
dp[0] = nums[0]
```

For two houses:

```text
dp[1] = max(nums[0], nums[1])
```

### Step 3: Recurrence Relation

At each house, we have two choices.

**Choice 1: Pick the current house**

If we pick house `i`, we cannot pick house `i - 1`.

```text
pick = nums[i] + dp[i - 2]
```

**Choice 2: Skip the current house**

If we skip house `i`, the answer remains the best answer up to house `i - 1`.

```text
notPick = dp[i - 1]
```

Choose the maximum:

```text
dp[i] = max(pick, notPick)
```

### Java Implementation

```java
public class HouseRobber {

    public static int rob(int[] nums) {

        int n = nums.length;

        if (n == 0) {
            return 0;
        }

        if (n == 1) {
            return nums[0];
        }

        int[] dp = new int[n];

        dp[0] = nums[0];

        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {

            int pick = nums[i] + dp[i - 2];

            int notPick = dp[i - 1];

            dp[i] = Math.max(pick, notPick);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {

        int[] nums = {2, 7, 9, 3, 1};

        System.out.println(rob(nums));
    }
}
```

Output:

```text
12
```

### Dry Run

Input:

```text
nums = [2, 7, 9, 3, 1]
```

| Index | Money | Pick | Not Pick | DP Value |
|---|---:|---:|---:|---:|
| 0 | 2 | Base case | - | 2 |
| 1 | 7 | - | - | 7 |
| 2 | 9 | 9 + 2 = 11 | 7 | 11 |
| 3 | 3 | 3 + 7 = 10 | 11 | 11 |
| 4 | 1 | 1 + 11 = 12 | 11 | 12 |

Final DP array:

```text
[2, 7, 11, 11, 12]
```

Answer:

```text
12
```

---

## 8. House Robber with Space Optimization

In the previous solution, we used an array to store all DP values.

But each state only depends on the previous two states.

Therefore, we can use two variables.

### Java Implementation

```java
public class HouseRobberOptimized {

    public static int rob(int[] nums) {

        int prev2 = 0;
        int prev1 = 0;

        for (int num : nums) {

            int pick = num + prev2;

            int notPick = prev1;

            int current = Math.max(pick, notPick);

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }

    public static void main(String[] args) {

        int[] nums = {2, 7, 9, 3, 1};

        System.out.println(rob(nums));
    }
}
```

Output:

```text
12
```

### Understanding the Variables

| Variable | Meaning |
|---|---|
| `prev2` | Best answer two positions earlier |
| `prev1` | Best answer at the previous position |
| `current` | Best answer at the current position |

Complexity:

- Time: O(n)
- Auxiliary space: O(1)

---

## 9. Problem 6: Maximum Sum of Non-Adjacent Elements

This is a common DP problem and follows the same pattern as House Robber.

**Problem:**

Given an array of integers, find the maximum sum of elements such that no two selected elements are adjacent.

Example:

```java
int[] arr = {3, 2, 7, 10};
```

Possible selections:

```text
3 + 7 = 10
3 + 10 = 13
2 + 10 = 12
```

Maximum sum:

```text
13
```

### Java Implementation

```java
public class MaxNonAdjacentSum {

    public static int maximumSum(int[] arr) {

        int prev2 = 0;
        int prev1 = 0;

        for (int num : arr) {

            int pick = num + prev2;

            int notPick = prev1;

            int current = Math.max(pick, notPick);

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }

    public static void main(String[] args) {

        int[] arr = {3, 2, 7, 10};

        System.out.println(maximumSum(arr));
    }
}
```

Output:

```text
13
```

**Important:** This implementation assumes selecting no elements is allowed. Therefore, for an array containing only negative values, it returns `0`. If the problem requires selecting at least one element, the DP state and initialization must be adjusted.

---

## 10. Comparison of Common 1D DP Problems

| Problem | DP State | Recurrence |
|---|---|---|
| Fibonacci | `dp[i]` = Fibonacci at i | `dp[i-1] + dp[i-2]` |
| Climbing Stairs | Ways to reach step i | `dp[i-1] + dp[i-2]` |
| Min Cost Climbing Stairs | Minimum cost to reach i | `cost[i] + min(dp[i-1], dp[i-2])` |
| Frog Jump | Minimum energy to reach i | Minimum over valid jumps |
| House Robber | Maximum money up to i | `max(nums[i] + dp[i-2], dp[i-1])` |

Notice that many DP problems use similar recurrence structures.

The important part is understanding what each DP state represents.

---

## 11. How to Identify 1D DP Problems

Look for problems where:

1. The answer depends on previous indexes or states.
2. You need to calculate a minimum or maximum.
3. You need to count the number of possible ways.
4. The same subproblems appear repeatedly.
5. The state can be represented using one index.

Examples:

| Problem Requirement | Possible DP State |
|---|---|
| Number of ways to reach a step | `dp[i]` |
| Minimum cost to reach a position | `dp[i]` |
| Maximum profit up to a position | `dp[i]` |
| Maximum sum without adjacent elements | `dp[i]` |
| Minimum energy to reach a stone | `dp[i]` |

---

## 12. Common Mistakes in 1D DP

### Mistake 1: Not Defining the State Clearly

Before writing code, understand what the DP array represents.

For example:

```text
dp[i] = Minimum energy needed to reach stone i
```

### Mistake 2: Incorrect Base Cases

Every DP problem has its own base cases.

Do not copy base cases from another problem without understanding them.

### Mistake 3: Incorrect Transition

For House Robber:

```text
dp[i] = max(nums[i] + dp[i - 2], dp[i - 1])
```

Do not add both choices together because they are mutually exclusive.

### Mistake 4: Incorrect Loop Boundaries

For a DP array of size `n`, valid indexes are:

```text
0 to n - 1
```

Make sure the loop does not access an invalid index.

### Mistake 5: Forgetting Edge Cases

Always consider:

- Empty array.
- One-element array.
- Two-element array.
- Large input.
- Negative values, if allowed by the problem.

### Mistake 6: Optimizing Space Too Early

First write a correct DP array solution.

After understanding the dependencies, optimize space.

---

## 13. Frequently Asked Interview Questions

### Q1. What is 1D Dynamic Programming?

It is a DP approach where each state is represented using one index and stored in a one-dimensional array or a small number of variables.

### Q2. What is the difference between 1D DP and 2D DP?

1D DP typically uses one index to represent a state.

2D DP uses two indexes, commonly to represent two dimensions, such as row and column or two sequence positions.

### Q3. How do you solve Climbing Stairs?

Use:

```text
dp[i] = dp[i - 1] + dp[i - 2]
```

### Q4. What is the recurrence relation for House Robber?

```text
dp[i] = max(nums[i] + dp[i - 2], dp[i - 1])
```

### Q5. Why can we optimize the space in Fibonacci?

Because each Fibonacci value depends only on the previous two values.

### Q6. What is the time complexity of Frog Jump with K distances?

O(n × k), because each index checks up to `k` previous positions.

### Q7. What is the difference between Memoization and Tabulation?

Memoization uses recursion and caching.

Tabulation uses iteration and calculates states from smaller states.

### Q8. Can 1D DP be optimized to O(1) space?

Yes, when each state depends only on a fixed number of previous states.

For example, Fibonacci and House Robber can be optimized to O(1) auxiliary space.

---

## 14. Practice Problems

### Beginner

1. Fibonacci Number.
2. Climbing Stairs.
3. Min Cost Climbing Stairs.
4. Frog Jump.
5. Maximum Sum of Non-Adjacent Elements.
6. House Robber.

### Intermediate

1. Frog Jump with K Distances.
2. House Robber II.
3. Delete and Earn.
4. Decode Ways.
5. Maximum Alternating Subsequence Sum.
6. Solving Questions With Brainpower.

### Recommended LeetCode Problems

| Problem | Difficulty |
|---|---|
| [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Easy |
| [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | Easy |
| [House Robber](https://leetcode.com/problems/house-robber/) | Medium |
| [House Robber II](https://leetcode.com/problems/house-robber-ii/) | Medium |
| [Delete and Earn](https://leetcode.com/problems/delete-and-earn/) | Medium |
| [Decode Ways](https://leetcode.com/problems/decode-ways/) | Medium |
| [Solving Questions With Brainpower](https://leetcode.com/problems/solving-questions-with-brainpower/) | Medium |

---

## 15. Quick Revision

- 1D DP uses a one-dimensional array or a small number of variables.
- Define the DP state before writing the recurrence.
- Identify base cases.
- Build the recurrence relation.
- Calculate states in the correct order.
- Climbing Stairs uses the previous two states.
- Min Cost Climbing Stairs calculates the minimum accumulated cost.
- Frog Jump calculates minimum energy.
- House Robber uses pick and not-pick choices.
- Space optimization is possible when only a fixed number of previous states are needed.
- Always consider edge cases and integer overflow.

---

## 16. Self-Assessment Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain 1D Dynamic Programming.
- [ ] Define a DP state.
- [ ] Write a recurrence relation.
- [ ] Identify base cases.
- [ ] Solve Climbing Stairs.
- [ ] Solve Min Cost Climbing Stairs.
- [ ] Solve Frog Jump.
- [ ] Solve Frog Jump with K distances.
- [ ] Solve House Robber.
- [ ] Solve Maximum Sum of Non-Adjacent Elements.
- [ ] Optimize DP space when possible.
- [ ] Explain time and space complexity.
- [ ] Solve at least three 1D DP problems independently.

---

## 17. What's Next?

Next, we will learn **2D Dynamic Programming**.

File: `14-dynamic-programming/2d-dp.md`

We will cover:

- What is 2D DP?
- Two-dimensional DP arrays.
- Unique Paths.
- Unique Paths with Obstacles.
- Minimum Path Sum.
- Grid-based DP.
- Recursion, Memoization, and Tabulation.
- Space optimization.
- Java implementations and dry runs.