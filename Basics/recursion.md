# Recursion

Recursion is one of the most important concepts in Data Structures and Algorithms.

It is used heavily in:

- Trees
- Graphs
- Backtracking
- Dynamic Programming
- Divide and Conquer
- Binary Search
- Merge Sort
- Quick Sort

Before learning advanced DSA, it is important to understand how recursion works internally.

---

# 1. What is Recursion?

**Recursion is a technique where a function calls itself to solve a smaller version of the same problem.**

For example:

```java
void print(int n) {
    if (n == 0) {
        return;
    }

    System.out.println(n);
    print(n - 1);
}
```

Here:

```text
print(5)
    ↓
print(4)
    ↓
print(3)
    ↓
print(2)
    ↓
print(1)
    ↓
print(0)
```

The function keeps calling itself until it reaches a condition where it stops.

That stopping condition is called the **base case**.

---

# 2. Two Important Parts of Recursion

Every recursive function generally contains two important parts:

1. Base Case
2. Recursive Case

---

## 2.1 Base Case

The **base case** tells the function when to stop calling itself.

Example:

```java
if (n == 0) {
    return;
}
```

Without a base case, the function may continue calling itself forever.

Example:

```java
void print(int n) {
    System.out.println(n);
    print(n - 1);
}
```

There is no stopping condition.

Eventually, Java will throw:

```text
StackOverflowError
```

---

## 2.2 Recursive Case

The recursive case is where the function calls itself with a smaller or simpler input.

Example:

```java
print(n - 1);
```

The problem becomes smaller:

```text
n
n - 1
n - 2
n - 3
...
```

A recursive solution should eventually reach the base case.

---

# 3. Basic Structure of Recursion

A typical recursive function looks like this:

```java
returnType function(parameters) {

    // Base Case
    if (condition) {
        return value;
    }

    // Recursive Case
    return function(smallerInput);
}
```

For example:

```java
int factorial(int n) {

    if (n == 0) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

Here:

```text
Base Case:
n == 0

Recursive Case:
factorial(n - 1)
```

---

# 4. Simple Example — Print Numbers

Let's print numbers from `1` to `5`.

## Code

```java
public class Main {

    static void printNumbers(int n) {

        if (n == 0) {
            return;
        }

        printNumbers(n - 1);

        System.out.println(n);
    }

    public static void main(String[] args) {

        printNumbers(5);
    }
}
```

Output:

```text
1
2
3
4
5
```

---

# 5. How Does This Work?

When we call:

```java
printNumbers(5);
```

The function does not immediately print `5`.

First, it calls:

```java
printNumbers(4);
```

Then:

```text
printNumbers(4)
    ↓
printNumbers(3)
    ↓
printNumbers(2)
    ↓
printNumbers(1)
    ↓
printNumbers(0)
```

When `n == 0`, the function returns.

Then the previous function continues.

So:

```text
printNumbers(0)
    ↓
return

printNumbers(1)
    ↓
print 1

printNumbers(2)
    ↓
print 2

printNumbers(3)
    ↓
print 3

printNumbers(4)
    ↓
print 4

printNumbers(5)
    ↓
print 5
```

---

# 6. Recursion and the Call Stack

Understanding the **call stack** is extremely important.

Consider:

```java
static void print(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    print(n - 1);
}
```

Calling:

```java
print(3);
```

creates stack frames.

Initially:

```text
print(3)
```

Then:

```text
print(3)
print(2)
```

Then:

```text
print(3)
print(2)
print(1)
```

Then:

```text
print(3)
print(2)
print(1)
print(0)
```

`print(0)` reaches the base case and returns.

Then:

```text
print(1)
```

returns.

Then:

```text
print(2)
```

returns.

Finally:

```text
print(3)
```

returns.

---

# 7. Visualizing the Call Stack

For:

```java
print(3);
```

The stack grows:

```text
        ┌─────────┐
        │ print(1)│
        ├─────────┤
        │ print(2)│
        ├─────────┤
        │ print(3)│
        └─────────┘
```

After `print(1)` finishes:

```text
        ┌─────────┐
        │ print(2)│
        ├─────────┤
        │ print(3)│
        └─────────┘
```

After `print(2)` finishes:

```text
        ┌─────────┐
        │ print(3)│
        └─────────┘
```

Finally:

```text
Empty Stack
```

This is why recursion uses **additional memory**.

---

# 8. Example — Print 1 to N

```java
static void print1ToN(int n) {

    if (n == 0) {
        return;
    }

    print1ToN(n - 1);

    System.out.println(n);
}
```

Calling:

```java
print1ToN(5);
```

Output:

```text
1
2
3
4
5
```

---

# 9. Example — Print N to 1

We can simply print before making the recursive call.

```java
static void printNTo1(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    printNTo1(n - 1);
}
```

Calling:

```java
printNTo1(5);
```

Output:

```text
5
4
3
2
1
```

---

# 10. Important Difference

Compare these two functions.

### Print before recursion

```java
static void print(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    print(n - 1);
}
```

Output:

```text
5
4
3
2
1
```

### Print after recursion

```java
static void print(int n) {

    if (n == 0) {
        return;
    }

    print(n - 1);

    System.out.println(n);
}
```

Output:

```text
1
2
3
4
5
```

This is because statements after the recursive call execute while the recursion is **returning back**.

---

# 11. Example — Sum of Numbers

Problem:

Calculate:

```text
1 + 2 + 3 + 4 + 5
```

We can define:

```text
sum(n) = n + sum(n - 1)
```

## Code

```java
static int sum(int n) {

    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}
```

Calling:

```java
int result = sum(5);
```

Result:

```text
15
```

---

# 12. Dry Run — Sum

Consider:

```java
sum(5)
```

The recursive calls are:

```text
sum(5)
= 5 + sum(4)

= 5 + 4 + sum(3)

= 5 + 4 + 3 + sum(2)

= 5 + 4 + 3 + 2 + sum(1)

= 5 + 4 + 3 + 2 + 1 + sum(0)
```

Base case:

```text
sum(0) = 0
```

Now return:

```text
sum(1) = 1

sum(2) = 2 + 1 = 3

sum(3) = 3 + 3 = 6

sum(4) = 4 + 6 = 10

sum(5) = 5 + 10 = 15
```

Final answer:

```text
15
```

---

# 13. Factorial

Factorial is one of the most common recursion examples.

Mathematically:

```text
5! = 5 × 4 × 3 × 2 × 1
```

We can define:

```text
n! = n × (n - 1)!
```

Therefore:

```text
factorial(n) = n × factorial(n - 1)
```

## Java

```java
static int factorial(int n) {

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

Result:

```text
120
```

---

# 14. Factorial Dry Run

```text
factorial(5)
```

becomes:

```text
5 × factorial(4)
```

Then:

```text
5 × 4 × factorial(3)
```

Then:

```text
5 × 4 × 3 × factorial(2)
```

Then:

```text
5 × 4 × 3 × 2 × factorial(1)
```

Base case:

```text
factorial(1) = 1
```

Now calculate:

```text
2 × 1 = 2

3 × 2 = 6

4 × 6 = 24

5 × 24 = 120
```

Final answer:

```text
120
```

---

# 15. Factorial Call Stack

For:

```java
factorial(5)
```

The stack becomes:

```text
factorial(5)
factorial(4)
factorial(3)
factorial(2)
factorial(1)
```

Then it starts returning:

```text
factorial(1) → 1
factorial(2) → 2
factorial(3) → 6
factorial(4) → 24
factorial(5) → 120
```

---

# 16. Power of a Number

Problem:

Calculate:

```text
2^5
```

We can write:

```text
2^5 = 2 × 2^4
```

Therefore:

```text
power(x, n) = x × power(x, n - 1)
```

## Code

```java
static int power(int x, int n) {

    if (n == 0) {
        return 1;
    }

    return x * power(x, n - 1);
}
```

Calling:

```java
power(2, 5);
```

Result:

```text
32
```

---

# 17. Count Digits

Problem:

Count the number of digits in:

```text
12345
```

We can remove one digit at a time using integer division.

```text
12345 / 10 = 1234
1234 / 10 = 123
123 / 10 = 12
12 / 10 = 1
1 / 10 = 0
```

## Code

```java
static int countDigits(int n) {

    if (n == 0) {
        return 0;
    }

    return 1 + countDigits(n / 10);
}
```

For:

```java
countDigits(12345);
```

Result:

```text
5
```

---

# 18. Reverse a String Using Recursion

Consider:

```text
hello
```

Expected:

```text
olleh
```

We can recursively process the remaining characters.

```java
static String reverse(String str) {

    if (str.isEmpty()) {
        return str;
    }

    return reverse(str.substring(1)) + str.charAt(0);
}
```

Calling:

```java
reverse("hello");
```

Result:

```text
olleh
```

### Dry Run

```text
reverse("hello")
    ↓
reverse("ello") + 'h'
    ↓
reverse("llo") + 'e' + 'h'
    ↓
reverse("lo") + 'l' + 'e' + 'h'
    ↓
reverse("o") + 'l' + 'l' + 'e' + 'h'
    ↓
reverse("") + 'o' + 'l' + 'l' + 'e' + 'h'
```

Result:

```text
olleh
```

> Note: Using `substring()` repeatedly can create extra work. In interviews, it is often better to use a character array or two-pointer approach when optimizing.

---

# 19. Find Maximum Element in an Array

Suppose:

```text
[10, 20, 5, 30, 15]
```

We want:

```text
30
```

A recursive solution:

```java
static int findMax(int[] arr, int index) {

    if (index == arr.length - 1) {
        return arr[index];
    }

    int maxOfRest = findMax(arr, index + 1);

    return Math.max(arr[index], maxOfRest);
}
```

Calling:

```java
int max = findMax(arr, 0);
```

Result:

```text
30
```

---

# 20. Recursion Tree

A recursion tree is a useful way to visualize recursive calls.

Consider:

```java
static int fibonacci(int n) {

    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

For:

```text
fibonacci(4)
```

The recursion tree is:

```text
                 fib(4)
                /      \
            fib(3)     fib(2)
           /    \       /   \
       fib(2) fib(1) fib(1) fib(0)
       /   \
   fib(1) fib(0)
```

Notice that:

```text
fib(2)
```

is calculated multiple times.

This is called **duplicate work**.

---

# 21. Fibonacci Recursion

Fibonacci sequence:

```text
0, 1, 1, 2, 3, 5, 8, 13...
```

Definition:

```text
fib(0) = 0
fib(1) = 1

fib(n) = fib(n - 1) + fib(n - 2)
```

Java:

```java
static int fibonacci(int n) {

    if (n <= 1) {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

Calling:

```java
fibonacci(6);
```

Result:

```text
8
```

---

# 22. Why Fibonacci Recursion Is Slow

Consider:

```text
fibonacci(5)
```

It creates multiple branches.

```text
                  fib(5)
                /        \
            fib(4)       fib(3)
           /    \        /   \
       fib(3)  fib(2) fib(2) fib(1)
```

Many values are calculated repeatedly.

For example:

```text
fib(2)
```

is calculated more than once.

The time complexity is approximately:

```text
O(2^n)
```

This is exponential time.

Later, Dynamic Programming will teach us how to optimize this.

---

# 23. Recursion Complexity

When analyzing recursion, consider:

1. Number of recursive calls
2. Work done inside each call
3. Maximum recursion depth

---

## Example

```java
static void print(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    print(n - 1);
}
```

There are `n` calls.

Therefore:

### Time Complexity

```text
O(n)
```

### Space Complexity

The maximum call stack depth is `n`.

Therefore:

```text
O(n)
```

---

# 24. Recursion Space Complexity

Even if we don't create an array or collection, recursion can consume memory.

For example:

```java
factorial(5)
```

creates:

```text
factorial(5)
factorial(4)
factorial(3)
factorial(2)
factorial(1)
```

Therefore:

```text
Space = O(n)
```

because of the call stack.

---

# 25. Recursion vs Iteration

The same problem can often be solved using either recursion or a loop.

## Recursion

```java
static int sum(int n) {

    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}
```

## Iteration

```java
static int sum(int n) {

    int result = 0;

    for (int i = 1; i <= n; i++) {
        result += i;
    }

    return result;
}
```

Both give:

```text
1 + 2 + ... + n
```

---

# 26. Recursion vs Iteration Comparison

| Feature | Recursion | Iteration |
|---|---|---|
| Uses function calls | Yes | No |
| Uses call stack | Yes | Usually no |
| Extra space | Often O(n) | Often O(1) |
| Code readability | Sometimes simpler | Sometimes simpler |
| Stack overflow risk | Yes | No |
| Useful for trees | Very useful | Possible |
| Useful for backtracking | Very useful | More difficult |
| Performance | Function-call overhead | Usually faster |

---

# 27. When Should You Use Recursion?

Recursion is useful when the problem naturally contains smaller versions of itself.

Common examples:

### Trees

```text
left subtree
right subtree
```

Each subtree is itself a tree.

---

### Graphs

DFS often uses recursion:

```text
visit node
    ↓
visit neighbors
        ↓
visit their neighbors
```

---

### Backtracking

Examples:

- N-Queens
- Sudoku
- Permutations
- Combinations
- Subsets
- Maze problems

---

### Divide and Conquer

Examples:

- Merge Sort
- Quick Sort
- Binary Search

---

# 28. Common Recursion Mistakes

## Mistake 1 — Missing Base Case

Wrong:

```java
static void print(int n) {
    System.out.println(n);
    print(n - 1);
}
```

There is no stopping condition.

---

## Mistake 2 — Base Case Never Reached

Wrong:

```java
static void print(int n) {

    if (n == 0) {
        return;
    }

    print(n + 1);
}
```

If `n` starts at `5`:

```text
5
6
7
8
...
```

It moves away from the base case.

---

## Mistake 3 — Wrong Return Value

Incorrect:

```java
static int factorial(int n) {

    if (n == 0) {
        return 0;
    }

    return n * factorial(n - 1);
}
```

The base case should be:

```java
return 1;
```

because:

```text
0! = 1
```

Correct:

```java
static int factorial(int n) {

    if (n == 0) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

---

# 29. Stack Overflow

Every recursive call requires a stack frame.

If recursion becomes too deep:

```text
function()
    ↓
function()
    ↓
function()
    ↓
function()
    ↓
...
```

Java may throw:

```text
StackOverflowError
```

Example:

```java
static void infiniteRecursion() {
    infiniteRecursion();
}
```

Calling:

```java
infiniteRecursion();
```

will eventually cause a stack overflow.

---

# 30. A Good Way to Think About Recursion

When solving a recursion problem, ask:

### Question 1

What is the smallest input?

This helps identify the base case.

### Question 2

How can I reduce the problem?

For example:

```text
n → n - 1
```

or:

```text
index → index + 1
```

### Question 3

What should I return from the smaller problem?

### Question 4

How can I use that result to solve the current problem?

---

# 31. Recursion Problem-Solving Template

Use this approach:

```text
Step 1:
Identify the base case.

Step 2:
Reduce the problem.

Step 3:
Call the function recursively.

Step 4:
Use the result of the recursive call.

Step 5:
Analyze time and space complexity.
```

---

# 32. Example — Sum of Array

Given:

```text
[1, 2, 3, 4, 5]
```

Find the sum.

We can define:

```text
sum(index)
```

The current element plus the remaining elements:

```text
arr[index] + sum(index + 1)
```

Code:

```java
static int arraySum(int[] arr, int index) {

    if (index == arr.length) {
        return 0;
    }

    return arr[index] + arraySum(arr, index + 1);
}
```

Calling:

```java
arraySum(arr, 0);
```

Result:

```text
15
```

---

# 33. Dry Run — Array Sum

Given:

```text
arr = [1, 2, 3]
```

Call:

```text
arraySum(arr, 0)
```

Becomes:

```text
1 + arraySum(arr, 1)
```

Then:

```text
1 + 2 + arraySum(arr, 2)
```

Then:

```text
1 + 2 + 3 + arraySum(arr, 3)
```

Base case:

```text
arraySum(arr, 3) = 0
```

Therefore:

```text
1 + 2 + 3 + 0
= 6
```

---

# 34. Example — Check Palindrome

A string is a palindrome if it reads the same forward and backward.

Examples:

```text
madam → palindrome
racecar → palindrome
hello → not palindrome
```

Recursive solution:

```java
static boolean isPalindrome(String str, int left, int right) {

    if (left >= right) {
        return true;
    }

    if (str.charAt(left) != str.charAt(right)) {
        return false;
    }

    return isPalindrome(str, left + 1, right - 1);
}
```

Usage:

```java
String str = "madam";

boolean result = isPalindrome(
    str,
    0,
    str.length() - 1
);
```

Result:

```text
true
```

---

# 35. Why Two Pointers Appear in Recursion

Notice:

```java
left + 1
right - 1
```

The problem gets smaller from both sides.

For:

```text
m a d a m
↑       ↑
L       R
```

Then:

```text
m a d a m
  ↑   ↑
  L   R
```

Then:

```text
m a d a m
    ↑
```

Once:

```text
left >= right
```

we know all characters matched.

---

# 36. Recursion Patterns You Should Learn

There are several common recursion patterns.

### Pattern 1 — Reduce by 1

```java
function(n - 1);
```

Examples:

- factorial
- sum
- printing numbers

---

### Pattern 2 — Move an Index

```java
function(index + 1);
```

Examples:

- array traversal
- searching
- array sum

---

### Pattern 3 — Two Pointers

```java
function(left + 1, right - 1);
```

Examples:

- palindrome
- reverse array

---

### Pattern 4 — Multiple Recursive Calls

```java
function(n - 1);
function(n - 2);
```

Examples:

- Fibonacci
- recursive tree problems
- some backtracking problems

---

### Pattern 5 — Divide the Problem

Example:

```text
left half
right half
```

Used in:

- Merge Sort
- Binary Search
- Divide and Conquer

---

# 37. Recursion and Trees

Recursion becomes especially important when we start learning trees.

Consider:

```text
        10
       /  \
      5    20
```

The left side is itself a smaller tree:

```text
    5
```

The right side is another smaller tree:

```text
    20
```

So we can recursively process:

```text
root
left subtree
right subtree
```

This is why tree traversal is naturally recursive.

For example:

```java
void traverse(Node root) {

    if (root == null) {
        return;
    }

    traverse(root.left);

    traverse(root.right);
}
```

We will study this in detail in the Trees section.

---

# 38. Recursion and Backtracking

Backtracking is built on recursion.

For example, generating subsets of:

```text
[1, 2, 3]
```

requires decisions such as:

```text
Take 1
Don't take 1
```

Then:

```text
Take 2
Don't take 2
```

And so on.

This creates a decision tree.

Later, in:

```text
11-recursion-backtracking/
```

we will study this in much more detail.

---

# 39. Recursion Complexity Cheat Sheet

| Recursion Pattern | Typical Time | Typical Space |
|---|---:|---:|
| One recursive call | O(n) | O(n) |
| Two recursive calls | Often O(2ⁿ) | O(n) |
| Divide into halves | Often O(log n) depth | O(log n) |
| Merge sort | O(n log n) | O(n) |
| Binary search | O(log n) | O(log n) recursive |
| Simple factorial | O(n) | O(n) |

These are general patterns. Always analyze the actual code rather than memorizing only the table.

---

# 40. How to Analyze a Recursive Function

Use this checklist:

### Step 1 — Count recursive calls

Example:

```java
function(n - 1);
```

One recursive call.

---

### Step 2 — Determine how input changes

Example:

```text
n → n - 1
```

There will be approximately `n` calls.

---

### Step 3 — Determine work inside each call

If each call performs constant work:

```text
O(1)
```

then total time is usually:

```text
O(n)
```

---

### Step 4 — Determine recursion depth

If maximum depth is:

```text
n
```

then stack space is:

```text
O(n)
```

---

# 41. Interview Questions

## Q1. What is recursion?

Recursion is a technique where a function calls itself to solve smaller instances of the same problem.

---

## Q2. What is a base case?

A base case is the condition that stops further recursive calls.

Example:

```java
if (n == 0) {
    return;
}
```

---

## Q3. What happens if there is no base case?

The recursive function continues indefinitely until the call stack is exhausted, resulting in:

```text
StackOverflowError
```

---

## Q4. Does recursion use extra memory?

Yes.

Recursive calls use the call stack.

For recursion depth `n`, the auxiliary stack space can be:

```text
O(n)
```

---

## Q5. Is recursion always better than iteration?

No.

Iteration often uses less memory and can be faster because it avoids repeated function-call overhead.

Recursion is preferred when it makes the problem structure easier to express, especially for trees, graphs, divide-and-conquer, and backtracking.

---

## Q6. What is stack overflow?

Stack overflow occurs when the program creates more stack frames than the available call stack can handle.

It commonly happens with:

- Infinite recursion
- Very deep recursion

---

## Q7. What is the difference between base case and recursive case?

### Base Case

Stops recursion.

```java
if (n == 0) {
    return;
}
```

### Recursive Case

Calls the function again with a smaller problem.

```java
return n + sum(n - 1);
```

---

## Q8. Why is Fibonacci recursion inefficient?

Because it repeatedly calculates the same subproblems.

For example:

```text
fib(2)
```

may be calculated multiple times.

Dynamic Programming can eliminate this duplicate work.

---

# 42. Practice Problems

Start with these in order.

## Easy

### 1. Print numbers from 1 to N

Example:

```text
Input: 5
Output: 1 2 3 4 5
```

---

### 2. Print numbers from N to 1

```text
Input: 5
Output: 5 4 3 2 1
```

---

### 3. Find sum from 1 to N

```text
Input: 5
Output: 15
```

---

### 4. Calculate factorial

```text
Input: 5
Output: 120
```

---

### 5. Calculate X raised to N

```text
Input: 2, 5
Output: 32
```

---

### 6. Count digits

```text
Input: 12345
Output: 5
```

---

### 7. Find maximum element in an array

```text
Input: [10, 20, 5, 30]
Output: 30
```

---

## Medium

### 8. Reverse a string recursively

```text
Input: "hello"
Output: "olleh"
```

---

### 9. Check whether a string is palindrome

```text
Input: "madam"
Output: true
```

---

### 10. Find sum of array recursively

```text
Input: [1, 2, 3, 4]
Output: 10
```

---

### 11. Find Fibonacci number

```text
Input: 7
Output: 13
```

---

### 12. Count occurrences of an element

```text
Input:
[1, 2, 2, 3, 2]
target = 2

Output:
3
```

---

# 43. Beginner Recursion Checklist

Before moving forward, make sure you can explain:

- [ ] What recursion is
- [ ] What a base case is
- [ ] What a recursive case is
- [ ] How the call stack works
- [ ] Why recursion can cause StackOverflowError
- [ ] How to print 1 to N recursively
- [ ] How to print N to 1 recursively
- [ ] How to calculate factorial
- [ ] How to calculate sum
- [ ] How to calculate power
- [ ] How to reverse a string
- [ ] How to check a palindrome
- [ ] How to recursively process an array
- [ ] How to calculate recursion time complexity
- [ ] How to calculate recursion space complexity
- [ ] Difference between recursion and iteration

---

# 44. Quick Revision

### Recursion

A function calling itself.

### Base Case

Condition that stops recursion.

### Recursive Case

The function calls itself with a smaller problem.

### Call Stack

Stores information about active function calls.

### Stack Overflow

Occurs when recursion becomes too deep.

### Important Formula

For factorial:

```text
factorial(n) = n × factorial(n - 1)
```

For sum:

```text
sum(n) = n + sum(n - 1)
```

For Fibonacci:

```text
fib(n) = fib(n - 1) + fib(n - 2)
```

For palindrome:

```text
compare left and right
→ move left forward
→ move right backward
```

---

# 45. What You Should Remember

The most important idea in recursion is:

> **Solve a smaller version of the same problem and trust the recursive call to solve that smaller problem.**

For every recursion problem, think:

```text
1. What is my base case?
2. How do I make the problem smaller?
3. What should the recursive call return?
4. How do I use that result?
5. What is the time complexity?
6. What is the recursion stack space?
```

A simple mental model is:

```text
Problem
   ↓
Smaller Problem
   ↓
Smaller Problem
   ↓
Base Case
   ↓
Return
   ↓
Build Answer
```

Once this becomes comfortable, topics such as **Trees, Graph DFS, Backtracking, Divide and Conquer, and Dynamic Programming** become much easier to understand.

---

# 46. Next Topic

You have now completed the three foundational topics:

```text
01-basics/
│
├── time-complexity.md
├── space-complexity.md
└── recursion.md
```

The next major section is:

```text
02-arrays/
```

Start with:

```text
02-arrays/arrays.md
```

Arrays are one of the most important DSA topics because many interview problems are built around:

- Traversal
- Searching
- Insertion
- Deletion
- Updating
- Sorting
- Two Pointers
- Sliding Window
- Prefix Sum
- Frequency Counting
- Subarrays
- Kadane's Algorithm