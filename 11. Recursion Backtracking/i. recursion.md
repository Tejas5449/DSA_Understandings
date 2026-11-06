# Recursion in Java

## 1. What Is Recursion?

**Recursion is a programming technique in which a method calls itself to solve a problem.**

Instead of solving the entire problem at once, recursion breaks it into smaller versions of the same problem.

A recursive method continues calling itself until it reaches a condition that stops the recursion.

This stopping condition is called the **base case**.

### Real-Life Example

Imagine standing in front of a line of people and asking:

"Who is the last person in this line?"

You ask the next person, who asks the next person, and so on.

Eventually, someone reaches the end and says:

"I am the last person."

The answer then travels backward through the line.

This is similar to recursion:

- Each person represents a recursive call.
- Reaching the last person represents the base case.
- Returning the answer represents the return process.

## 2. Basic Structure of Recursion

A recursive method generally contains two important parts:

1. Base case
2. Recursive call

### Syntax

```java
returnType methodName(parameters) {

    // Base case
    if (stoppingCondition) {
        return result;
    }

    // Recursive call
    return methodName(smallerInput);
}
```

### Example: Print Numbers from 1 to 5

```java
public class RecursionExample {

    static void printNumbers(int n) {

        // Base case
        if (n > 5) {
            return;
        }

        System.out.println(n);

        // Recursive call
        printNumbers(n + 1);
    }

    public static void main(String[] args) {

        printNumbers(1);
    }
}
```

**Output:**

```text
1
2
3
4
5
```

### Explanation

- Initially, `printNumbers(1)` is called.
- It prints `1`.
- It calls `printNumbers(2)`.
- It continues until `n` becomes `6`.
- The base condition becomes true, so the method returns.

## 3. Understanding the Call Stack

Java uses a call stack to manage method calls.

Whenever a method is called, a new stack frame is created.

When the method finishes, its stack frame is removed.

Consider:

```java
static void printNumbers(int n) {

    if (n > 3) {
        return;
    }

    System.out.println(n);

    printNumbers(n + 1);
}
```

Calling `printNumbers(1)` creates these calls:

```text
printNumbers(1)
    |
printNumbers(2)
    |
printNumbers(3)
    |
printNumbers(4)
    |
Base case reached
```

The calls then return in reverse order:

```text
printNumbers(4) returns
printNumbers(3) returns
printNumbers(2) returns
printNumbers(1) returns
```

This behavior follows the **LIFO (Last In, First Out)** principle.

## 4. Base Case and Recursive Case

These are the two most important concepts in recursion.

### Base Case

The condition that stops further recursive calls.

Example:

```java
if (n == 0) {
    return;
}
```

Without a correct base case, recursion may continue indefinitely and eventually cause a `StackOverflowError`.

### Recursive Case

The part where the method calls itself with a smaller or simpler input.

Example:

```java
printNumbers(n - 1);
```

A correct recursive case must make progress toward the base case.

## 5. Recursion Example: Print Numbers from N to 1

```java
public class PrintDescending {

    static void print(int n) {

        if (n == 0) {
            return;
        }

        System.out.println(n);

        print(n - 1);
    }

    public static void main(String[] args) {

        print(5);
    }
}
```

**Output:**

```text
5
4
3
2
1
```

## 6. Recursion Example: Print Numbers from 1 to N

There are two common approaches.

### Approach 1: Increase the Parameter

```java
static void printAscending(int current, int n) {

    if (current > n) {
        return;
    }

    System.out.println(current);

    printAscending(current + 1, n);
}
```

Usage:

```java
printAscending(1, 5);
```

### Approach 2: Print While Returning

```java
static void printAscending(int n) {

    if (n == 0) {
        return;
    }

    printAscending(n - 1);

    System.out.println(n);
}
```

Usage:

```java
printAscending(5);
```

**Output:**

```text
1
2
3
4
5
```

Notice that the second approach prints after the recursive call returns.

## 7. Recursion Example: Factorial

The factorial of a number is the product of all positive integers from that number down to `1`.

Formula:

\[
n! = n \times (n-1) \times \cdots \times 1
\]

Examples:

```text
5! = 5 × 4 × 3 × 2 × 1 = 120

4! = 4 × 3 × 2 × 1 = 24

0! = 1
```

### Recursive Formula

\[
n! = n \times (n-1)!
\]

Base case:

\[
0! = 1
\]

### Java Implementation

```java
public class Factorial {

    static long factorial(int n) {

        if (n == 0 || n == 1) {
            return 1;
        }

        return n * factorial(n - 1);
    }

    public static void main(String[] args) {

        System.out.println(factorial(5));
    }
}
```

**Output:**

```text
120
```

### Dry Run

Calling:

```java
factorial(5);
```

Recursive calls:

```text
factorial(5)
= 5 * factorial(4)

= 5 * 4 * factorial(3)

= 5 * 4 * 3 * factorial(2)

= 5 * 4 * 3 * 2 * factorial(1)

= 5 * 4 * 3 * 2 * 1
```

Final answer:

```text
120
```

### Complexity

- Time Complexity: `O(n)`
- Auxiliary Space Complexity: `O(n)` because of the recursion stack.

**Important:** This implementation uses `long`, but even `long` cannot store arbitrarily large factorial values. For very large values, use `BigInteger`.

## 8. Recursion Example: Sum of First N Natural Numbers

We need to calculate:

\[
1 + 2 + 3 + \cdots + n
\]

Recursive formula:

\[
sum(n) = n + sum(n-1)
\]

Base case:

\[
sum(0) = 0
\]

### Java Implementation

```java
public class SumOfNumbers {

    static int sum(int n) {

        if (n == 0) {
            return 0;
        }

        return n + sum(n - 1);
    }

    public static void main(String[] args) {

        System.out.println(sum(5));
    }
}
```

**Output:**

```text
15
```

### Dry Run

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

- Time: `O(n)`
- Auxiliary Space: `O(n)`

## 9. Recursion Example: Fibonacci Series

The Fibonacci sequence is a series in which each number is the sum of the previous two numbers.

```text
0, 1, 1, 2, 3, 5, 8, 13, 21...
```

Recursive formula:

\[
fib(n) = fib(n-1) + fib(n-2)
\]

Base cases:

\[
fib(0) = 0
\]

\[
fib(1) = 1
\]

### Java Implementation

```java
public class Fibonacci {

    static int fibonacci(int n) {

        if (n == 0) {
            return 0;
        }

        if (n == 1) {
            return 1;
        }

        return fibonacci(n - 1) + fibonacci(n - 2);
    }

    public static void main(String[] args) {

        for (int i = 0; i < 7; i++) {

            System.out.print(fibonacci(i) + " ");
        }
    }
}
```

**Output:**

```text
0 1 1 2 3 5 8
```

### Understanding the Recursive Tree

For `fibonacci(4)`:

```text
                  fib(4)
                 /      \
             fib(3)     fib(2)
             /   \       /   \
         fib(2) fib(1) fib(1) fib(0)
          /   \
      fib(1) fib(0)
```

Notice that `fib(2)` is calculated more than once.

This repeated calculation makes the basic recursive implementation inefficient.

### Complexity

- Time Complexity: `O(2^n)` as a simple upper-bound estimate.
- Auxiliary Space Complexity: `O(n)` due to maximum recursion depth.

We can improve this using Dynamic Programming, which we'll study later.

## 10. Recursion Example: Reverse a String

We can reverse a string recursively by processing characters from the end.

### Java Implementation

```java
public class ReverseString {

    static String reverse(String str) {

        if (str == null || str.length() <= 1) {
            return str;
        }

        return reverse(str.substring(1)) + str.charAt(0);
    }

    public static void main(String[] args) {

        System.out.println(reverse("hello"));
    }
}
```

**Output:**

```text
olleh
```

### Explanation

```text
reverse("hello")
= reverse("ello") + "h"

= reverse("llo") + "e" + "h"

= reverse("lo") + "l" + "e" + "h"

= reverse("o") + "l" + "l" + "e" + "h"

= "olleh"
```

**Note:** Java's `substring()` creates new string objects or string representations, and repeated concatenation can make this implementation more expensive than `O(n)`. For large strings, prefer an iterative approach or a character-array solution.

## 11. Recursion Example: Check Whether a String Is a Palindrome

A palindrome reads the same forward and backward.

Examples:

```text
madam
level
racecar
```

We can compare characters from both ends and move toward the center.

### Java Implementation

```java
public class Palindrome {

    static boolean isPalindrome(
            String str,
            int left,
            int right) {

        if (left >= right) {
            return true;
        }

        if (str.charAt(left) != str.charAt(right)) {
            return false;
        }

        return isPalindrome(
                str,
                left + 1,
                right - 1
        );
    }

    public static void main(String[] args) {

        String str = "madam";

        boolean result = isPalindrome(
                str,
                0,
                str.length() - 1
        );

        System.out.println(result);
    }
}
```

**Output:**

```text
true
```

Complexity:

- Time: `O(n)`
- Auxiliary Space: `O(n)` due to recursion.

## 12. Recursion Example: Find Maximum Element in an Array

We can find the maximum element recursively by processing one element at a time.

### Java Implementation

```java
public class ArrayMaximum {

    static int findMax(int[] arr, int index) {

        // Base case
        if (index == arr.length - 1) {
            return arr[index];
        }

        // Find maximum in remaining elements
        int maxInRest = findMax(arr, index + 1);

        // Compare current element with remaining maximum
        return Math.max(arr[index], maxInRest);
    }

    public static void main(String[] args) {

        int[] arr = {10, 25, 5, 40, 15};

        System.out.println(findMax(arr, 0));
    }
}
```

**Output:**

```text
40
```

Complexity:

- Time: `O(n)`
- Auxiliary Space: `O(n)`.

**Note:** This implementation assumes the array is non-empty.

## 13. Recursion Example: Binary Search

Binary Search can also be implemented recursively.

It works on a sorted array.

At each step, we compare the target with the middle element and search only one half of the array.

### Java Implementation

```java
public class RecursiveBinarySearch {

    static int binarySearch(
            int[] arr,
            int left,
            int right,
            int target) {

        if (left > right) {
            return -1;
        }

        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;
        }

        if (target < arr[mid]) {

            return binarySearch(
                    arr,
                    left,
                    mid - 1,
                    target
            );
        }

        return binarySearch(
                arr,
                mid + 1,
                right,
                target
        );
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50, 60};

        int result = binarySearch(
                arr,
                0,
                arr.length - 1,
                40
        );

        System.out.println(result);
    }
}
```

**Output:**

```text
3
```

Complexity:

- Time: `O(log n)`
- Auxiliary Space: `O(log n)` due to recursive calls.

## 14. Types of Recursion

Understanding these types helps when solving different problems.

### 1. Direct Recursion

A method calls itself directly.

```java
static void method(int n) {

    if (n == 0) {
        return;
    }

    method(n - 1);
}
```

### 2. Indirect Recursion

Two or more methods call each other.

```java
static void methodA(int n) {

    if (n <= 0) {
        return;
    }

    System.out.println("A: " + n);

    methodB(n - 1);
}

static void methodB(int n) {

    if (n <= 0) {
        return;
    }

    System.out.println("B: " + n);

    methodA(n - 1);
}
```

### 3. Tail Recursion

A recursive call is the last operation performed by the method.

```java
static int factorialTail(int n, int result) {

    if (n == 0) {
        return result;
    }

    return factorialTail(n - 1, n * result);
}
```

Usage:

```java
System.out.println(factorialTail(5, 1));
```

Output:

```text
120
```

Java does not guarantee tail-call optimization. Tail recursion can still consume stack space.

### 4. Head Recursion

The recursive call happens before the remaining operations.

```java
static void printHead(int n) {

    if (n == 0) {
        return;
    }

    printHead(n - 1);

    System.out.println(n);
}
```

Output for `printHead(5)`:

```text
1
2
3
4
5
```

## 15. Recursion vs Iteration

| Feature | Recursion | Iteration |
|---|---|---|
| Definition | Method calls itself | Uses loops |
| Control | Base case | Loop condition |
| Memory | Uses call stack | Usually uses constant extra space |
| Performance | Has method-call overhead | Usually less overhead |
| Readability | Often simpler for tree and divide-and-conquer problems | Often simpler for straightforward repetition |
| Risk | StackOverflowError | Infinite loop if condition is incorrect |

### Example: Factorial Using Iteration

```java
static long factorialIterative(int n) {

    long result = 1;

    for (int i = 2; i <= n; i++) {
        result *= i;
    }

    return result;
}
```

Both recursive and iterative approaches produce the same result.

The iterative implementation uses `O(1)` auxiliary space, while the recursive implementation uses `O(n)` stack space.

## 16. How to Think About Recursive Problems

When solving a recursive problem, follow these steps.

### Step 1: Identify the Smaller Problem

Ask:

Can I solve this problem using a smaller version of the same problem?

For factorial:

```text
factorial(5) = 5 * factorial(4)
```

### Step 2: Define the Base Case

Ask:

When should recursion stop?

For factorial:

```java
if (n == 0) {
    return 1;
}
```

### Step 3: Define the Recursive Relationship

For factorial:

```java
return n * factorial(n - 1);
```

### Step 4: Check Progress

Ensure that every recursive call moves toward the base case.

For example:

```java
factorial(n - 1);
```

moves toward zero.

But:

```java
factorial(n + 1);
```

moves away from zero and can cause infinite recursion.

### Step 5: Dry Run

Manually trace the calls and return values.

This helps identify incorrect base cases and recursive relationships.

## 17. Common Recursion Mistakes

1. **Missing base case**

   The method keeps calling itself until the stack overflows.

2. **Incorrect base case**

   The method stops too early or returns the wrong value.

3. **Not making progress**

   Recursive calls do not move toward the stopping condition.

4. **Incorrect return statement**

   The result of the recursive call is ignored when it should be returned or combined.

5. **Not understanding the call stack**

   Remember that recursive calls return in reverse order.

6. **Repeated calculations**

   Basic recursive Fibonacci calculates the same values many times.

7. **Using recursion for unnecessarily large inputs**

   Deep recursion can cause `StackOverflowError`.

8. **Integer overflow**

   Recursive calculations can exceed the range of `int` or `long`.

## 18. Important Recursion Interview Questions

### Beginner

1. What is recursion?
2. What is a base case?
3. What happens if a recursive method has no base case?
4. What is a call stack?
5. What is the difference between recursion and iteration?
6. What is direct recursion?
7. What is indirect recursion?

### Intermediate

1. Write a recursive program to calculate factorial.
2. Print numbers from `N` to `1` using recursion.
3. Find the sum of digits using recursion.
4. Reverse a string using recursion.
5. Check whether a string is a palindrome.
6. Find the maximum element in an array using recursion.
7. Implement binary search using recursion.
8. Explain the time and space complexity of recursive Fibonacci.

### Advanced

1. What is tail recursion?
2. What is head recursion?
3. What is a recursion tree?
4. What is stack overflow?
5. How can repeated recursive calculations be optimized?
6. What is the relationship between recursion and Dynamic Programming?
7. How does recursion help in tree traversal?
8. What is the difference between recursion and backtracking?

## 19. Practice Problems

Practice these problems to improve recursive thinking.

### Easy

1. [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)
2. [Reverse String](https://leetcode.com/problems/reverse-string/)
3. [Power of Two](https://leetcode.com/problems/power-of-two/)
4. [Palindrome Number](https://leetcode.com/problems/palindrome-number/)

### Medium

1. [Pow(x, n)](https://leetcode.com/problems/powx-n/)
2. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
3. [K-th Symbol in Grammar](https://leetcode.com/problems/k-th-symbol-in-grammar/)
4. [Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/)

## 20. Quick Revision

| Concept | Meaning |
|---|---|
| Recursion | A method calls itself |
| Base case | Stops recursive calls |
| Recursive case | Calls the method with a smaller problem |
| Call stack | Stores active method calls |
| Direct recursion | Method calls itself |
| Indirect recursion | Methods call each other |
| Tail recursion | Recursive call is the last operation |
| Head recursion | Recursive call happens before remaining work |
| Stack overflow | Call stack exceeds its available capacity |
| Recursion tree | Visual representation of recursive calls |

### Complexity Summary

| Problem | Time | Auxiliary Space |
|---|---|---|
| Print N numbers | `O(n)` | `O(n)` |
| Factorial | `O(n)` | `O(n)` |
| Sum of N numbers | `O(n)` | `O(n)` |
| Fibonacci (naive) | `O(2^n)` upper bound | `O(n)` |
| Palindrome check | `O(n)` | `O(n)` |
| Array maximum | `O(n)` | `O(n)` |
| Binary Search | `O(log n)` | `O(log n)` |

## 21. Learning Checklist

- [ ] Understand what recursion is.
- [ ] Understand base cases and recursive cases.
- [ ] Understand how Java manages recursive calls using the call stack.
- [ ] Print numbers from `1` to `N`.
- [ ] Print numbers from `N` to `1`.
- [ ] Calculate factorial recursively.
- [ ] Calculate the sum of natural numbers.
- [ ] Generate Fibonacci numbers.
- [ ] Reverse a string recursively.
- [ ] Check a palindrome using recursion.
- [ ] Find the maximum element in an array.
- [ ] Implement recursive binary search.
- [ ] Understand recursion vs iteration.
- [ ] Practice at least three recursion problems.

---

## Conclusion

Recursion is one of the most important concepts in DSA.

The key idea is:

**Break a large problem into smaller versions of the same problem until you reach a base case.**

Before moving to backtracking, make sure you understand:

1. Base cases.
2. Recursive calls.
3. Call stack behavior.
4. Return values.
5. Dry-running recursive methods.

These concepts will make backtracking, tree algorithms, graph algorithms, and Dynamic Programming easier to understand.