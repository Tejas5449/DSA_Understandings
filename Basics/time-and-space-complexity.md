# Time & Space Complexity

## 1. What is Time Complexity?

When we write a program, we want to understand:

> **How does the running time of our program change when the input size becomes larger?**

Consider this array:

```java
int[] numbers = {10, 20, 30, 40, 50};
```

Suppose we want to find `30`.

We can check each element one by one:

```java
for (int i = 0; i < numbers.length; i++) {

    if (numbers[i] == 30) {
        System.out.println("Found");
        break;
    }
}
```

For 5 elements, this is very fast.

But imagine the input grows:

```text
5 elements
100 elements
10,000 elements
1,000,000 elements
```

The important question becomes:

> **How does the amount of work grow as the input size grows?**

This is what **Time Complexity** helps us understand.

---

# 2. What is Big O Notation?

**Big O notation** describes how the time or space requirements of an algorithm grow as the input size increases.

We normally represent the input size using:

```text
n
```

For example:

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

If:

```text
n = 10
```

the loop runs approximately 10 times.

If:

```text
n = 100
```

the loop runs approximately 100 times.

If:

```text
n = 1,000
```

the loop runs approximately 1,000 times.

Therefore:

```text
Time Complexity = O(n)
```

---

# 3. Why Do We Use Big O?

We don't usually measure algorithms using exact execution time because execution time depends on many external factors:

- Computer hardware
- CPU speed
- Programming language
- Compiler
- Operating system
- Background processes
- Input data

Instead, we focus on how the algorithm **scales** as the input becomes larger.

For example:

```text
Algorithm A → O(n)

Algorithm B → O(n²)
```

For small inputs, both may appear fast.

But when `n` becomes very large, `O(n²)` grows much faster than `O(n)`.

Therefore, Big O helps us compare algorithms based on their growth rate.

---

# 4. Common Time Complexities

The most common complexities you should know are:

| Big O | Name | Example |
|---|---|---|
| `O(1)` | Constant | Array access |
| `O(log n)` | Logarithmic | Binary Search |
| `O(n)` | Linear | Single loop |
| `O(n log n)` | Linearithmic | Merge Sort |
| `O(n²)` | Quadratic | Nested loops |
| `O(n³)` | Cubic | Three nested loops |
| `O(2ⁿ)` | Exponential | Some recursive algorithms |
| `O(n!)` | Factorial | Permutations |

A useful order to remember:

```text
O(1)
   ↓
O(log n)
   ↓
O(n)
   ↓
O(n log n)
   ↓
O(n²)
   ↓
O(n³)
   ↓
O(2ⁿ)
   ↓
O(n!)
```

Generally, the lower the growth rate, the better the algorithm performs for large inputs.

---

# 5. O(1) — Constant Time

`O(1)` means the amount of work does not depend on the input size.

Consider:

```java
int[] numbers = {10, 20, 30, 40, 50};

System.out.println(numbers[2]);
```

We directly access index `2`.

Whether the array contains:

```text
5 elements
```

or:

```text
5,000,000 elements
```

we still access one specific index.

Therefore:

```text
Time Complexity = O(1)
```

### Another Example

```java
int a = 10;
int b = 20;

int sum = a + b;
```

The number of operations remains constant.

Therefore:

```text
Time Complexity = O(1)
```

### Important

`O(1)` does **not** mean one second.

It means:

> The amount of work does not grow with the input size.

---

# 6. O(n) — Linear Time

`O(n)` means the amount of work grows linearly with the input size.

Example:

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

The loop executes `n` times.

Therefore:

```text
Time Complexity = O(n)
```

### Example

```text
n = 5

5 operations
```

```text
n = 100

100 operations
```

```text
n = 10,000

10,000 operations
```

The amount of work increases proportionally with `n`.

---

# 7. O(log n) — Logarithmic Time

`O(log n)` means the input is reduced significantly, usually by a factor such as `2`, during each step.

The most common example is:

> **Binary Search**

Suppose we have a sorted array:

```text
[10, 20, 30, 40, 50, 60, 70, 80]
```

We want to find:

```text
70
```

Instead of checking every element, Binary Search repeatedly divides the search area in half.

```text
8 elements
     ↓
4 elements
     ↓
2 elements
     ↓
1 element
```

The search space keeps getting smaller.

Therefore:

```text
Time Complexity = O(log n)
```

### Important

Binary Search requires the data to be sorted.

---

# 8. O(n log n) — Linearithmic Time

`O(n log n)` is commonly found in efficient sorting algorithms.

Examples include:

```text
Merge Sort
Heap Sort
Average-case Quick Sort
```

Merge Sort works by repeatedly dividing the array.

Conceptually:

```text
             n
           /   \
         n/2   n/2
        / \     / \
      n/4 n/4 n/4 n/4
```

The division creates approximately `log n` levels.

At each level, we process approximately `n` elements.

Therefore:

```text
n × log n

= O(n log n)
```

---

# 9. O(n²) — Quadratic Time

`O(n²)` usually appears when we have two nested loops.

Example:

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        System.out.println(i + " " + j);
    }
}
```

The outer loop runs:

```text
n times
```

For every iteration of the outer loop, the inner loop runs:

```text
n times
```

Therefore:

```text
n × n

= n²
```

So:

```text
Time Complexity = O(n²)
```

### Example

If:

```text
n = 10
```

approximately:

```text
10 × 10 = 100 operations
```

If:

```text
n = 1,000
```

approximately:

```text
1,000 × 1,000 = 1,000,000 operations
```

This shows why quadratic algorithms become expensive for large inputs.

---

# 10. O(n³) — Cubic Time

`O(n³)` usually occurs with three nested loops.

Example:

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        for (int k = 0; k < n; k++) {

            System.out.println(i + j + k);
        }
    }
}
```

The loops execute:

```text
n × n × n
```

Therefore:

```text
Time Complexity = O(n³)
```

---

# 11. O(2ⁿ) — Exponential Time

`O(2ⁿ)` means the amount of work approximately doubles as the input increases by one.

This commonly appears in recursive algorithms that create multiple branches.

Example:

```java
public static int fibonacci(int n) {

    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

The function creates multiple recursive calls.

Conceptually:

```text
                  fibonacci(n)
                 /           \
        fibonacci(n-1)     fibonacci(n-2)
          /       \
       ...        ...
```

The number of calls grows very quickly.

The simple recursive Fibonacci implementation has approximately:

```text
Time Complexity = O(2ⁿ)
```

---

# 12. O(n!) — Factorial Time

`O(n!)` is one of the fastest-growing complexity classes.

It commonly appears when generating all permutations.

For example:

```text
3! = 3 × 2 × 1
   = 6
```

```text
5! = 5 × 4 × 3 × 2 × 1
   = 120
```

```text
10! = 3,628,800
```

The number of possibilities grows extremely quickly.

Therefore:

```text
Time Complexity = O(n!)
```

---

# 13. How to Calculate Big O

Learning to calculate Big O is one of the most important DSA skills.

Let's look at different examples.

---

## Example 1 — Single Loop

```java
for (int i = 0; i < n; i++) {

    System.out.println(i);
}
```

The loop runs `n` times.

Therefore:

```text
O(n)
```

---

## Example 2 — Two Separate Loops

```java
for (int i = 0; i < n; i++) {

}

for (int j = 0; j < n; j++) {

}
```

First loop:

```text
O(n)
```

Second loop:

```text
O(n)
```

Total:

```text
O(n) + O(n)

= O(2n)
```

We ignore the constant:

```text
O(n)
```

---

## Example 3 — Nested Loops

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

    }
}
```

Because the loops are nested:

```text
O(n × n)

= O(n²)
```

---

## Example 4 — Three Nested Loops

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        for (int k = 0; k < n; k++) {

        }
    }
}
```

Therefore:

```text
O(n × n × n)

= O(n³)
```

---

# 14. Ignore Constants

Consider:

```text
O(2n)
```

We simplify this to:

```text
O(n)
```

Why?

Because Big O focuses on the **growth rate**, not the exact number of operations.

Similarly:

```text
O(5n)
→ O(n)
```

```text
O(100n)
→ O(n)
```

---

# 15. Ignore Lower-Order Terms

Consider:

```text
O(n² + n)
```

When `n` becomes very large, `n²` grows much faster than `n`.

Therefore:

```text
O(n² + n)

= O(n²)
```

Another example:

```text
O(n³ + n² + n + 10)
```

The dominant term is:

```text
n³
```

Therefore:

```text
O(n³)
```

### Rule

> **Keep the term that grows the fastest.**

---

# 16. Sequential vs Nested Loops

This is a very common interview question.

### Sequential Loops

```java
for (int i = 0; i < n; i++) {

}

for (int j = 0; j < n; j++) {

}
```

Complexity:

```text
O(n) + O(n)

= O(n)
```

---

### Nested Loops

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

    }
}
```

Complexity:

```text
O(n × n)

= O(n²)
```

### Remember

```text
Sequential loops → Add

Nested loops → Multiply
```

This is a useful beginner rule.

---

# 17. Different Input Sizes

Sometimes loops depend on different input variables.

Example:

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < m; j++) {

    }
}
```

The complexity is:

```text
O(n × m)
```

We should **not** automatically write `O(n²)` because `n` and `m` may be different.

---

# 18. Space Complexity

Time Complexity tells us:

> How much work does the algorithm perform?

Space Complexity tells us:

> How much additional memory does the algorithm require?

Consider:

```java
public static int findMaximum(int[] numbers) {

    int max = numbers[0];

    for (int number : numbers) {

        if (number > max) {
            max = number;
        }
    }

    return max;
}
```

We only create a few variables:

```text
max
number
```

The number of additional variables does not depend on `n`.

Therefore:

```text
Auxiliary Space = O(1)
```

---

# 19. O(n) Space

Consider:

```java
int[] result = new int[n];
```

The size of the new array depends on `n`.

Therefore:

```text
Space Complexity = O(n)
```

---

# 20. O(n²) Space

Consider:

```java
int[][] matrix = new int[n][n];
```

The matrix contains:

```text
n × n
```

elements.

Therefore:

```text
Space Complexity = O(n²)
```

---

# 21. Input Space vs Auxiliary Space

This distinction is important.

Suppose we have:

```java
public static int findMaximum(int[] numbers) {

    int max = numbers[0];

    for (int number : numbers) {

        if (number > max) {
            max = number;
        }
    }

    return max;
}
```

The input array already exists.

We don't create another array.

So the **auxiliary space** is:

```text
O(1)
```

When discussing algorithm space complexity, interviewers often care about **auxiliary space**, meaning the additional memory used by the algorithm.

---

# 22. Time-Space Tradeoff

Sometimes we use more memory to make an algorithm faster.

This is called a:

> **Time-Space Tradeoff**

A classic example is **Two Sum**.

Suppose:

```text
numbers = [2, 7, 11, 15]
target = 9
```

We need to find:

```text
2 + 7 = 9
```

### Brute Force

We can compare every pair.

```text
Time:  O(n²)
Space: O(1)
```

### Using HashMap

We can store values in a HashMap.

```text
Time:  O(n)
Space: O(n)
```

We use additional memory to reduce the time.

This is one of the most important concepts in DSA.

---

# 23. Best, Average and Worst Case

An algorithm can behave differently depending on the input.

Consider Linear Search:

```java
int[] numbers = {10, 20, 30, 40, 50};
```

Suppose we search for:

```text
10
```

It is the first element.

Only one comparison is required.

Therefore:

```text
Best Case = O(1)
```

---

If we search for:

```text
50
```

we may need to check every element.

Therefore:

```text
Worst Case = O(n)
```

---

If the element is somewhere in the middle, we can consider the average behavior.

```text
Average Case = O(n)
```

Therefore, for a basic Linear Search:

```text
Best Case:    O(1)
Average Case: O(n)
Worst Case:   O(n)
```

---

# 24. Complexity of Common Operations

Here is a quick reference:

| Operation | Complexity |
|---|---:|
| Access array element | `O(1)` |
| Update array element | `O(1)` |
| Linear Search | `O(n)` |
| Binary Search | `O(log n)` |
| Traverse array | `O(n)` |
| Compare every pair | `O(n²)` |
| Merge Sort | `O(n log n)` |

We will study these data structures and algorithms in detail in later chapters.

---

# 25. Complexity Growth Example

Let's compare how the number of operations grows.

For `n = 10`:

| Complexity | Approx. Operations |
|---|---:|
| `O(1)` | 1 |
| `O(log n)` | ~3 |
| `O(n)` | 10 |
| `O(n log n)` | ~30 |
| `O(n²)` | 100 |
| `O(n³)` | 1,000 |
| `O(2ⁿ)` | 1,024 |
| `O(n!)` | 3,628,800 |

This is why choosing the right algorithm becomes increasingly important as the input grows.

---

# 26. Real-World Example

Imagine you have a list of:

```text
1,000,000 users
```

You want to find a specific user.

### Approach 1 — Linear Search

Check users one by one.

```text
O(n)
```

In the worst case, we may inspect:

```text
1,000,000 users
```

---

### Approach 2 — Binary Search

If the data is sorted appropriately, we can repeatedly divide the search space.

```text
O(log n)
```

Approximately:

```text
1,000,000
     ↓
500,000
     ↓
250,000
     ↓
...
     ↓
1
```

This requires far fewer steps.

The important lesson is:

> **The choice of algorithm matters when the input becomes large.**

---

# 27. Common Mistakes

## Mistake 1 — Thinking O(1) Means One Second

Wrong:

> O(1) means the program takes one second.

Correct:

> O(1) means the amount of work does not grow with the input size.

---

## Mistake 2 — Adding Nested Loops

Wrong:

```text
O(n + n) = O(n²)
```

Correct:

```text
O(n + n) = O(n)
```

---

## Mistake 3 — Multiplying Separate Loops

Wrong:

```text
Two separate loops = O(n²)
```

Correct:

```text
O(n) + O(n) = O(n)
```

---

## Mistake 4 — Forgetting Space Complexity

When solving a DSA problem, always ask:

```text
Time Complexity?
Space Complexity?
```

---

## Mistake 5 — Ignoring the Input Constraints

Suppose a problem says:

```text
1 <= n <= 100
```

An `O(n²)` solution might be completely acceptable.

But if:

```text
1 <= n <= 1,000,000
```

an `O(n²)` solution may be too slow.

Therefore:

> **Always check the input constraints before choosing an approach.**

---

# 28. How to Analyze Any Algorithm

Whenever you write a solution, follow these steps.

### Step 1 — Identify the input size

Usually:

```text
n
```

For two inputs:

```text
n and m
```

---

### Step 2 — Count loops

Ask:

```text
How many times does each loop run?
```

---

### Step 3 — Check whether loops are nested

Sequential:

```text
O(n) + O(n)
```

Nested:

```text
O(n) × O(n)
```

---

### Step 4 — Check recursion

Ask:

```text
How many recursive calls are made?
How deep can the recursion go?
```

---

### Step 5 — Check additional data structures

Look for:

```text
Arrays
HashMaps
HashSets
Stacks
Queues
Matrices
```

---

### Step 6 — Simplify

Remove constants:

```text
O(2n) → O(n)
```

Keep the dominant term:

```text
O(n² + n) → O(n²)
```

---

# 29. Interview Answer Format

When an interviewer asks:

> "What is the time and space complexity of your solution?"

Don't simply answer:

> `O(n)`.

Give a reason.

For example:

> **"The time complexity is O(n) because we traverse the array once. The auxiliary space is O(n) because we store the elements in a HashMap."**

This demonstrates that you understand the reasoning behind the complexity.

---

# 30. Interview Questions

## Question 1

What is Time Complexity?

### Answer

Time Complexity describes how the number of operations performed by an algorithm grows as the input size increases.

---

## Question 2

What is Space Complexity?

### Answer

Space Complexity describes how the additional memory required by an algorithm grows as the input size increases.

---

## Question 3

What is Big O notation?

### Answer

Big O notation describes the growth rate of an algorithm's time or space requirements as the input size increases.

---

## Question 4

What is the complexity of:

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

### Answer

```text
O(n)
```

Because the loop runs `n` times.

---

## Question 5

What is the complexity of:

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

    }
}
```

### Answer

```text
O(n²)
```

Because the inner loop runs `n` times for every iteration of the outer loop.

---

## Question 6

What is the complexity of Binary Search?

### Answer

```text
O(log n)
```

Because the search space is divided approximately in half during each step.

---

## Question 7

What is the complexity of Merge Sort?

### Answer

```text
O(n log n)
```

---

## Question 8

Why do we ignore constants in Big O?

### Answer

Because Big O focuses on the growth rate of an algorithm rather than the exact number of operations.

For example:

```text
O(5n)
```

is simplified to:

```text
O(n)
```

---

## Question 9

Why do we remove lower-order terms?

### Answer

Because the dominant term determines the growth rate for very large input sizes.

For example:

```text
O(n² + n)
```

becomes:

```text
O(n²)
```

---

## Question 10

What is a Time-Space Tradeoff?

### Answer

A Time-Space Tradeoff means using additional memory to improve the execution time of an algorithm.

Example:

```text
Two Sum

Brute Force:
Time:  O(n²)
Space: O(1)

HashMap:
Time:  O(n)
Space: O(n)
```

---

# 31. Practice Questions

Try to solve these yourself.

## Problem 1

What is the time complexity?

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

---

## Problem 2

What is the time complexity?

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        System.out.println(i + j);
    }
}
```

---

## Problem 3

What is the time complexity?

```java
for (int i = 0; i < n; i++) {

}

for (int j = 0; j < n; j++) {

}
```

---

## Problem 4

What is the time complexity?

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; i < n; j++) {

    }
}
```

**Be careful!**

Don't answer immediately. Check the loop condition carefully.

---

## Problem 5

What is the space complexity?

```java
int[] result = new int[n];
```

---

## Problem 6

What is the space complexity?

```java
int[][] matrix = new int[n][n];
```

---

## Problem 7

What is the complexity?

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < m; j++) {

    }
}
```

---

## Problem 8

What is the complexity?

```text
O(n² + n + 100)
```

---

## Problem 9

What is the complexity?

```text
O(5n + 10)
```

---

## Problem 10

What is the complexity?

```text
O(n³ + n² + n)
```

---

# 32. Quick Revision

Before moving to the next topic, remember:

```text
O(1)
→ Constant

O(log n)
→ Logarithmic

O(n)
→ Linear

O(n log n)
→ Linearithmic

O(n²)
→ Quadratic

O(n³)
→ Cubic

O(2ⁿ)
→ Exponential

O(n!)
→ Factorial
```

### Important Rules

```text
Sequential loops
→ Add complexities

Nested loops
→ Multiply complexities

Ignore constants
→ O(2n) = O(n)

Keep the dominant term
→ O(n² + n) = O(n²)
```

### Always Ask

```text
1. What is the input size?
2. How many times does each loop execute?
3. Are loops nested or sequential?
4. Is recursion involved?
5. What additional memory is being used?
6. Can the solution be optimized?
```

---

# 33. Final Summary

Time Complexity helps us understand how the running time of an algorithm grows as the input becomes larger.

Space Complexity helps us understand how the additional memory requirement grows.

The most important complexities to remember are:

```text
O(1)
O(log n)
O(n)
O(n log n)
O(n²)
O(2ⁿ)
O(n!)
```

The goal of DSA is not simply to write code that works.

The goal is to write code that:

```text
Works
   +
Uses appropriate data structures
   +
Has good time complexity
   +
Uses reasonable memory
   +
Can handle large inputs
```

> **Good DSA solutions are not only correct — they are efficient.**
