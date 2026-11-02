# Chapter 1 — DSA Basics

Welcome to my **Data Structures and Algorithms** learning repository.

This chapter covers the fundamental concepts that are required before starting Data Structures and Algorithms.

## Topics Covered

1. [What is DSA?](#1-what-is-dsa)
2. [What is an Algorithm?](#2-what-is-an-algorithm)
3. [What is a Data Structure?](#3-what-is-a-data-structure)
4. [Time Complexity](#4-time-complexity)
5. [Big O Notation](#5-big-o-notation)
6. [Common Time Complexities](#6-common-time-complexities)
7. [How to Calculate Time Complexity](#7-how-to-calculate-time-complexity)
8. [Space Complexity](#8-space-complexity)
9. [Time-Space Tradeoff](#9-time-space-tradeoff)
10. [Best, Average and Worst Case](#10-best-average-and-worst-case)
11. [Recursion](#11-recursion)
12. [Base Case and Recursive Case](#12-base-case-and-recursive-case)
13. [Call Stack](#13-call-stack)
14. [Recursion Examples](#14-recursion-examples)
15. [Recursion vs Iteration](#15-recursion-vs-iteration)
16. [Common Beginner Mistakes](#16-common-beginner-mistakes)
17. [Interview Questions](#17-interview-questions)
18. [Practice Problems](#18-practice-problems)
19. [Key Takeaways](#19-key-takeaways)

---

# 1. What is DSA?

**DSA** stands for:

> **Data Structures and Algorithms**

It is one of the most important foundations of programming and problem solving.

DSA helps us answer two questions:

1. How should we **store data**?
2. How should we **process that data efficiently**?

For example, suppose we have:

```text
10, 20, 30, 40, 50
```

We need to decide:

- How should we store these values?
- How can we search for a value?
- How can we insert a new value?
- How can we delete a value?
- How can we sort the values?

DSA gives us the concepts and techniques to solve these problems efficiently.

---

# 2. What is an Algorithm?

An **algorithm** is a step-by-step procedure used to solve a problem.

For example, suppose we want to find the largest number in an array.

Input:

```text
[10, 25, 7, 50, 30]
```

We can solve it using these steps:

```text
1. Assume the first number is the largest.
2. Compare it with the next number.
3. If the next number is larger, update the largest number.
4. Continue until the array ends.
5. Return the largest number.
```

Java implementation:

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

Output:

```text
50
```

The code is the implementation of the algorithm.

---

# 3. What is a Data Structure?

A **data structure** is a way of organizing and storing data so that we can use it efficiently.

Some common data structures are:

```text
Array
Linked List
Stack
Queue
HashMap
HashSet
Tree
Heap
Graph
```

For example:

## Array

```java
int[] numbers = {10, 20, 30, 40, 50};
```

Arrays store elements in an indexed structure.

We can access:

```java
numbers[0]; // 10
numbers[2]; // 30
numbers[4]; // 50
```

---

## Stack

A Stack follows:

> **LIFO — Last In, First Out**

Example:

```text
        ┌───────┐
        │   30  │ ← Top
        ├───────┤
        │   20  │
        ├───────┤
        │   10  │
        └───────┘
```

If we remove an element:

```text
30
```

comes out first.

---

## Queue

A Queue follows:

> **FIFO — First In, First Out**

Example:

```text
10 → 20 → 30 → 40
↑
Front
```

`10` will be removed first.

---

# 4. Time Complexity

Time Complexity describes:

> **How the amount of work performed by an algorithm grows as the input size grows.**

We usually represent the input size using:

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
n = 1,000,000
```

the loop runs approximately 1,000,000 times.

Therefore:

```text
Time Complexity = O(n)
```

---

# 5. Big O Notation

**Big O notation** is used to describe the growth rate of an algorithm.

Some common Big O complexities are:

```text
O(1)
O(log n)
O(n)
O(n log n)
O(n²)
O(n³)
O(2ⁿ)
O(n!)
```

The general order from better to worse is:

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

As the input becomes very large, algorithms with lower complexity generally perform better.

---

# 6. Common Time Complexities

## 6.1 O(1) — Constant Time

The amount of work does not depend on the input size.

Example:

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

### Important

`O(1)` does not mean the operation takes exactly one second.

It means the amount of work remains constant as the input grows.

---

# 6.2 O(n) — Linear Time

Example:

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

The loop runs `n` times.

Therefore:

```text
Time Complexity = O(n)
```

Example:

```text
n = 5       → 5 operations
n = 100     → 100 operations
n = 10,000  → 10,000 operations
```

The work grows linearly with the input.

---

# 6.3 O(log n) — Logarithmic Time

A common example is **Binary Search**.

Suppose we have:

```text
[10, 20, 30, 40, 50, 60, 70, 80]
```

Instead of checking every element, Binary Search repeatedly divides the search area into half.

```text
8 elements
    ↓
4 elements
    ↓
2 elements
    ↓
1 element
```

Therefore:

```text
Time Complexity = O(log n)
```

Binary Search requires the data to be sorted.

---

# 6.4 O(n log n)

This complexity commonly appears in efficient sorting algorithms.

Examples:

```text
Merge Sort
Heap Sort
Average-case Quick Sort
```

For example:

```text
Time Complexity = O(n log n)
```

Merge Sort divides the array repeatedly and processes the elements at each level.

---

# 6.5 O(n²) — Quadratic Time

Example:

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        System.out.println(i + " " + j);
    }
}
```

The outer loop runs `n` times.

For every iteration, the inner loop also runs `n` times.

Therefore:

```text
n × n = n²
```

So:

```text
Time Complexity = O(n²)
```

This type of complexity commonly appears when comparing every element with every other element.

---

# 6.6 O(n³) — Cubic Time

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

There are three nested loops.

Therefore:

```text
O(n × n × n)

= O(n³)
```

---

# 6.7 O(2ⁿ) — Exponential Time

Exponential complexity commonly appears in recursive algorithms where each function call creates multiple additional calls.

Example:

```java
public static int fibonacci(int n) {

    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

The number of calls grows very quickly.

Therefore, the simple recursive Fibonacci implementation has exponential time complexity.

```text
O(2ⁿ)
```

---

# 6.8 O(n!) — Factorial Time

Factorial complexity is commonly seen when generating all possible permutations.

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

This grows extremely quickly.

Therefore:

```text
Time Complexity = O(n!)
```

---

# 7. How to Calculate Time Complexity

Learning how to calculate Big O is extremely important for DSA interviews.

---

## Example 1 — One Loop

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

We ignore constants:

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

The loops are nested.

Therefore:

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

## Example 5 — Loop with a Constant

```java
for (int i = 0; i < n; i++) {

    System.out.println(i);
}

for (int i = 0; i < 100; i++) {

    System.out.println(i);
}
```

First loop:

```text
O(n)
```

Second loop:

```text
O(100)
```

Since `100` is a constant:

```text
O(100) = O(1)
```

Therefore:

```text
O(n) + O(1)

= O(n)
```

---

## Example 6 — Dominant Term

Consider:

```text
O(n² + n + 10)
```

When `n` becomes very large, `n²` grows much faster than `n`.

Therefore:

```text
O(n² + n + 10)

= O(n²)
```

We keep the dominant term.

---

# 8. Space Complexity

Space Complexity describes:

> **How much additional memory an algorithm requires as the input grows.**

Example:

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

We only use a few variables:

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

# 8.1 O(n) Space

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

# 8.2 O(n²) Space

Consider:

```java
int[][] matrix = new int[n][n];
```

We create:

```text
n × n
```

elements.

Therefore:

```text
Space Complexity = O(n²)
```

---

# 9. Time-Space Tradeoff

Sometimes we use additional memory to make an algorithm faster.

This is called a:

> **Time-Space Tradeoff**

Example: Two Sum.

## Brute Force

```text
Time:  O(n²)
Space: O(1)
```

## Using HashMap

```text
Time:  O(n)
Space: O(n)
```

We use additional memory to improve execution time.

This concept is extremely important in DSA.

---

# 10. Best, Average and Worst Case

An algorithm can behave differently depending on the input.

Consider searching for an element in an array:

```text
[10, 20, 30, 40, 50]
```

Suppose we search for `10`.

It is the first element.

Only one comparison is required.

This is the:

> **Best Case**

```text
Best Case = O(1)
```

---

If we search for `50`, we may need to check every element.

This can be:

```text
Worst Case = O(n)
```

If the searched value is somewhere in the middle, we can consider this an average scenario.

```text
Average Case = O(n)
```

For Linear Search:

```text
Best Case:    O(1)
Average Case: O(n)
Worst Case:   O(n)
```

---

# 11. Recursion

Recursion is a technique where a function calls itself.

Example:

```java
public static void printNumber(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    printNumber(n - 1);
}
```

Calling:

```java
printNumber(5);
```

produces:

```text
5
4
3
2
1
```

The function keeps calling itself with a smaller value.

---

# 12. Base Case and Recursive Case

Every recursive function generally contains two important parts:

## 12.1 Base Case

The condition that stops recursion.

Example:

```java
if (n == 0) {
    return;
}
```

Without a proper base case, recursion may continue indefinitely and eventually cause:

```text
StackOverflowError
```

---

## 12.2 Recursive Case

The part where the function calls itself.

Example:

```java
printNumber(n - 1);
```

Complete example:

```java
public static void printNumber(int n) {

    // Base Case
    if (n == 0) {
        return;
    }

    // Work
    System.out.println(n);

    // Recursive Case
    printNumber(n - 1);
}
```

---

# 13. Call Stack

To understand recursion, we need to understand the **Call Stack**.

Consider:

```java
printNumber(3);
```

The calls happen like this:

```text
printNumber(3)
       ↓
printNumber(2)
       ↓
printNumber(1)
       ↓
printNumber(0)
       ↓
return
```

The stack looks conceptually like:

```text
┌─────────────────┐
│ printNumber(0)  │
├─────────────────┤
│ printNumber(1)  │
├─────────────────┤
│ printNumber(2)  │
├─────────────────┤
│ printNumber(3)  │
└─────────────────┘
```

When the base case is reached, the functions return in reverse order.

This is called:

> **Last In, First Out (LIFO)**

This is the same principle used by a Stack data structure.

---

# 14. Recursion Examples

## 14.1 Print Numbers from N to 1

```java
public static void printNumbers(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    printNumbers(n - 1);
}
```

Calling:

```java
printNumbers(5);
```

Output:

```text
5
4
3
2
1
```

### Complexity

Each number is processed once.

```text
Time: O(n)
Space: O(n)
```

The space is `O(n)` because of the recursive call stack.

---

# 14.2 Print Numbers from 1 to N

```java
public static void printNumbers(int n) {

    if (n == 0) {
        return;
    }

    printNumbers(n - 1);

    System.out.println(n);
}
```

Calling:

```java
printNumbers(5);
```

Output:

```text
1
2
3
4
5
```

Notice the difference.

In the previous example, we printed **before** the recursive call.

Here, we print **after** the recursive call.

---

# 14.3 Factorial

Factorial of `n`:

```text
n! = n × (n-1) × (n-2) × ... × 1
```

For example:

```text
5! = 5 × 4 × 3 × 2 × 1

5! = 120
```

Recursive implementation:

```java
public static int factorial(int n) {

    if (n == 0 || n == 1) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

Calling:

```java
factorial(5);
```

Dry run:

```text
factorial(5)
= 5 × factorial(4)

= 5 × 4 × factorial(3)

= 5 × 4 × 3 × factorial(2)

= 5 × 4 × 3 × 2 × factorial(1)

= 5 × 4 × 3 × 2 × 1

= 120
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 14.4 Sum of Numbers

Find:

```text
1 + 2 + 3 + 4 + 5
```

Recursive solution:

```java
public static int sum(int n) {

    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}
```

Calling:

```java
sum(5);
```

Dry run:

```text
sum(5)
= 5 + sum(4)

= 5 + 4 + sum(3)

= 5 + 4 + 3 + sum(2)

= 5 + 4 + 3 + 2 + sum(1)

= 5 + 4 + 3 + 2 + 1 + sum(0)

= 15
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 15. Recursion vs Iteration

The same problem can often be solved using either recursion or a loop.

## Recursive Approach

```java
public static int sum(int n) {

    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}
```

Space:

```text
O(n)
```

because recursive calls are stored on the call stack.

---

## Iterative Approach

```java
public static int sum(int n) {

    int result = 0;

    for (int i = 1; i <= n; i++) {
        result += i;
    }

    return result;
}
```

Space:

```text
O(1)
```

### Comparison

| Approach | Time | Space |
|---|---:|---:|
| Recursion | O(n) | O(n) |
| Iteration | O(n) | O(1) |

Recursion can make some problems easier to understand, especially:

```text
Trees
Graphs
Backtracking
Divide and Conquer
Dynamic Programming
```

But recursion uses call-stack memory.

---

# 16. Common Beginner Mistakes

## Mistake 1 — Forgetting the Base Case

Wrong:

```java
public static void count(int n) {

    System.out.println(n);

    count(n - 1);
}
```

There is no stopping condition.

This can eventually cause:

```text
StackOverflowError
```

Correct:

```java
public static void count(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    count(n - 1);
}
```

---

## Mistake 2 — Confusing Nested and Separate Loops

Separate loops:

```java
for (...) {
}

for (...) {
}
```

Usually:

```text
O(n)
```

Nested loops:

```java
for (...) {

    for (...) {

    }
}
```

Usually:

```text
O(n²)
```

---

## Mistake 3 — Assuming More Code Means Higher Complexity

This:

```java
System.out.println("A");
System.out.println("B");
System.out.println("C");
System.out.println("D");
```

is still:

```text
O(1)
```

because the number of operations is constant.

---

## Mistake 4 — Forgetting Auxiliary Space

When analyzing an algorithm, don't only consider time.

Always ask:

```text
Time?
Space?
```

---

# 17. Interview Questions

## Question 1

What is an algorithm?

### Answer

An algorithm is a step-by-step procedure used to solve a particular problem.

---

## Question 2

What is a data structure?

### Answer

A data structure is a way of organizing and storing data so that it can be accessed and modified efficiently.

---

## Question 3

What is Big O?

### Answer

Big O notation describes how the time or space requirements of an algorithm grow as the input size increases.

---

## Question 4

What is the time complexity of:

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

### Answer

```text
O(n)
```

---

## Question 5

What is the time complexity of:

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

---

## Question 6

What is the space complexity of:

```java
int[] result = new int[n];
```

### Answer

```text
O(n)
```

---

## Question 7

What is the difference between time and space complexity?

### Answer

**Time Complexity** measures how the execution time grows with input size.

**Space Complexity** measures how the additional memory requirement grows with input size.

---

## Question 8

What is a base case in recursion?

### Answer

A base case is the condition that stops recursive calls.

Example:

```java
if (n == 0) {
    return;
}
```

---

## Question 9

What happens if recursion has no proper base case?

### Answer

The recursive calls may continue until the call stack is exhausted, resulting in a `StackOverflowError`.

---

## Question 10

What is the difference between recursion and iteration?

### Answer

Recursion uses function calls to repeat an operation, while iteration generally uses loops such as `for` and `while`.

Recursion uses additional call-stack memory.

---

# 18. Practice Problems

Try solving these yourself before looking for solutions.

## Easy

### Problem 1 — Find Maximum

Given:

```text
[10, 25, 7, 50, 30]
```

Find the maximum element.

Expected output:

```text
50
```

Think about:

```text
Time Complexity?
Space Complexity?
```

---

### Problem 2 — Find Minimum

Given:

```text
[10, 25, 7, 50, 30]
```

Find the minimum element.

Expected output:

```text
7
```

---

### Problem 3 — Sum of Array

Given:

```text
[10, 20, 30, 40]
```

Return:

```text
100
```

---

### Problem 4 — Count Even Numbers

Given:

```text
[10, 15, 20, 25, 30]
```

Expected output:

```text
3
```

---

### Problem 5 — Reverse Print

Given:

```text
1 2 3 4 5
```

Print:

```text
5 4 3 2 1
```

Try solving it using recursion.

---

## Complexity Practice

Find the time complexity of each.

### Problem 6

```java
for (int i = 0; i < n; i++) {

}
```

---

### Problem 7

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

    }
}
```

---

### Problem 8

```java
for (int i = 0; i < n; i++) {

}

for (int j = 0; j < n; j++) {

}
```

---

### Problem 9

```java
for (int i = 0; i < n; i++) {

    for (int j = 0; j < n; j++) {

        for (int k = 0; k < n; k++) {

        }
    }
}
```

---

### Problem 10

```java
int[] numbers = new int[n];
```

Find the space complexity.

---

# 19. Key Takeaways

Before moving to the next chapter, make sure you understand these concepts:

```text
DSA
 ↓
Data Structures + Algorithms
```

### Data Structure

A way of organizing and storing data.

Examples:

```text
Array
Linked List
Stack
Queue
HashMap
Tree
Graph
```

### Algorithm

A step-by-step procedure for solving a problem.

### Time Complexity

Describes how execution work grows with input size.

### Space Complexity

Describes how additional memory usage grows with input size.

### Important Complexities

```text
O(1)       → Constant
O(log n)   → Logarithmic
O(n)       → Linear
O(n log n) → Linearithmic
O(n²)      → Quadratic
O(2ⁿ)      → Exponential
O(n!)      → Factorial
```

### Recursion

A function calling itself.

Every recursive solution should generally have:

```text
Base Case
+
Recursive Case
```

### Most Important Interview Habit

Whenever you write a solution, ask yourself:

```text
1. What is the time complexity?
2. What is the space complexity?
3. Can I improve the time?
4. Can I reduce the space?
5. Is there a better data structure?
6. Is there a known problem-solving pattern?
```

---

# Chapter 1 Complete

The foundation is now ready.

Next chapters will build on these concepts:

```text
Chapter 1 — Basics
        ↓
Chapter 2 — Arrays
        ↓
Chapter 3 — Strings
        ↓
Chapter 4 — Linked List
        ↓
Chapter 5 — Stack
        ↓
Chapter 6 — Queue
        ↓
Chapter 7 — Hashing
        ↓
Chapter 8 — Searching & Sorting
        ↓
Chapter 9 — Trees
        ↓
Chapter 10 — Heap
        ↓
Chapter 11 — Graphs
        ↓
Chapter 12 — Recursion & Backtracking
        ↓
Chapter 13 — Greedy
        ↓
Chapter 14 — Dynamic Programming
        ↓
Interview Problem Solving
```

> **Goal:** Don't just memorize solutions. Understand the data structure, identify the pattern, write the solution, analyze its complexity, and explain why it works.
