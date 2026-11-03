# Space Complexity

## 1. What is Space Complexity?

When we write a program, we don't only care about how much time it takes.

We also need to ask:

> **How much memory does the algorithm require as the input size grows?**

This is called **Space Complexity**.

For example:

```java
int number = 10;
```

This variable uses a fixed amount of memory.

But consider:

```java
int[] numbers = new int[n];
```

Here, the amount of memory depends on `n`.

If:

```text
n = 10
```

we store 10 integers.

If:

```text
n = 1,000,000
```

we store 1,000,000 integers.

Therefore, the memory usage grows with `n`.

```text
Space Complexity = O(n)
```

---

# 2. Why is Space Complexity Important?

An algorithm can be very fast but use too much memory.

For example, suppose we have two solutions:

```text
Solution A

Time:  O(n)
Space: O(1)
```

and:

```text
Solution B

Time:  O(n)
Space: O(n)
```

Both may solve the problem correctly.

But Solution A uses less additional memory.

On the other hand, sometimes using extra memory can make an algorithm significantly faster.

This is called a:

> **Time-Space Tradeoff**

We will see this later.

---

# 3. What Does Space Complexity Measure?

Space Complexity describes how much memory an algorithm needs as the input size increases.

There are two concepts you should understand:

```text
1. Input Space
2. Auxiliary Space
```

---

# 4. Input Space

Input space is the memory already occupied by the input provided to the algorithm.

For example:

```java
public static int findMaximum(int[] numbers) {
    // ...
}
```

The array:

```java
int[] numbers
```

is input to the function.

If the array contains `n` elements, the input itself requires:

```text
O(n)
```

space.

However, we generally don't count the input itself when talking about **auxiliary space**.

---

# 5. Auxiliary Space

Auxiliary Space means:

> **The extra memory used by the algorithm apart from the input.**

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

The input array already exists.

We only use:

```text
max
number
```

These are constant-sized variables.

Therefore:

```text
Auxiliary Space = O(1)
```

This distinction is very important in DSA interviews.

---

# 6. O(1) Space — Constant Space

`O(1)` space means the algorithm uses a constant amount of additional memory.

Example:

```java
public static int sum(int a, int b) {

    int result = a + b;

    return result;
}
```

We use only a fixed number of variables.

Whether the input is:

```text
10
```

or:

```text
1,000,000
```

the number of variables remains the same.

Therefore:

```text
Space Complexity = O(1)
```

---

## Another Example

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

We don't create another array.

Therefore:

```text
Auxiliary Space = O(1)
```

Even though the input array may contain millions of elements.

---

# 7. O(n) Space — Linear Space

`O(n)` space means memory usage grows linearly with the input size.

Example:

```java
int[] result = new int[n];
```

If:

```text
n = 10
```

we create an array of 10 elements.

If:

```text
n = 1000
```

we create an array of 1000 elements.

Therefore:

```text
Space Complexity = O(n)
```

---

## Example: Copy an Array

```java
public static int[] copyArray(int[] numbers) {

    int[] result = new int[numbers.length];

    for (int i = 0; i < numbers.length; i++) {

        result[i] = numbers[i];
    }

    return result;
}
```

We create another array containing `n` elements.

Therefore:

```text
Time Complexity = O(n)
Auxiliary Space = O(n)
```

---

# 8. O(n²) Space — Quadratic Space

Consider:

```java
int[][] matrix = new int[n][n];
```

This creates:

```text
n × n
```

elements.

Therefore:

```text
Space Complexity = O(n²)
```

For example:

```text
n = 3
```

creates:

```text
3 × 3 = 9
```

elements.

For:

```text
n = 100
```

we have:

```text
100 × 100 = 10,000
```

elements.

---

# 9. O(n × m) Space

Sometimes there are two different input sizes.

Example:

```java
int[][] matrix = new int[n][m];
```

The matrix contains:

```text
n × m
```

elements.

Therefore:

```text
Space Complexity = O(n × m)
```

Don't automatically write `O(n²)` unless `n` and `m` represent the same input size.

---

# 10. Space Complexity of Variables

Consider:

```java
int a = 10;
int b = 20;
int c = 30;
int d = 40;
```

There are a fixed number of variables.

Therefore:

```text
Space Complexity = O(1)
```

Even if we have 100 fixed variables:

```text
Space Complexity = O(1)
```

because the number of variables does not grow with the input.

---

# 11. Space Complexity of an Array

Consider:

```java
int[] numbers = new int[n];
```

The number of elements depends on `n`.

Therefore:

```text
Space Complexity = O(n)
```

---

# 12. Space Complexity of a Matrix

Consider:

```java
int[][] matrix = new int[n][n];
```

There are:

```text
n × n
```

elements.

Therefore:

```text
Space Complexity = O(n²)
```

---

# 13. Space Complexity of a HashMap

Consider:

```java
HashMap<Integer, String> map = new HashMap<>();

for (int i = 0; i < n; i++) {

    map.put(i, "value");
}
```

The HashMap can contain up to `n` entries.

Therefore:

```text
Auxiliary Space = O(n)
```

The time complexity of inserting entries is a separate question.

Here we are only analyzing memory.

---

# 14. Space Complexity of a HashSet

Example:

```java
HashSet<Integer> set = new HashSet<>();

for (int number : numbers) {

    set.add(number);
}
```

In the worst case, the set can contain `n` elements.

Therefore:

```text
Auxiliary Space = O(n)
```

---

# 15. Space Complexity of Stack

Suppose we create:

```java
Stack<Integer> stack = new Stack<>();
```

and add `n` elements:

```java
for (int i = 0; i < n; i++) {

    stack.push(i);
}
```

The stack contains up to `n` elements.

Therefore:

```text
Space Complexity = O(n)
```

---

# 16. Space Complexity of Queue

Similarly:

```java
Queue<Integer> queue = new LinkedList<>();
```

If we add `n` elements:

```java
for (int i = 0; i < n; i++) {

    queue.add(i);
}
```

The queue can contain `n` elements.

Therefore:

```text
Space Complexity = O(n)
```

---

# 17. Recursion and Space Complexity

Recursion is very important when analyzing space complexity.

Consider:

```java
public static void countDown(int n) {

    if (n == 0) {
        return;
    }

    System.out.println(n);

    countDown(n - 1);
}
```

Calling:

```java
countDown(5);
```

creates calls like:

```text
countDown(5)
    ↓
countDown(4)
    ↓
countDown(3)
    ↓
countDown(2)
    ↓
countDown(1)
    ↓
countDown(0)
```

These function calls are stored in the **call stack**.

The maximum depth is approximately `n`.

Therefore:

```text
Time Complexity = O(n)
Space Complexity = O(n)
```

The `O(n)` space comes from the recursive call stack.

---

# 18. Recursion with O(1) Additional Variables

Consider:

```java
public static int factorial(int n) {

    if (n == 0 || n == 1) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

We only have a few local variables, but the recursive calls keep accumulating on the call stack.

For:

```text
factorial(5)
```

we have:

```text
factorial(5)
factorial(4)
factorial(3)
factorial(2)
factorial(1)
```

Therefore:

```text
Space Complexity = O(n)
```

This is an important interview concept:

> **A recursive algorithm can use O(n) space even if it creates no array or collection.**

---

# 19. Iteration vs Recursion

Consider calculating the sum from `1` to `n`.

### Recursive Solution

```java
public static int sum(int n) {

    if (n == 0) {
        return 0;
    }

    return n + sum(n - 1);
}
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

because of the call stack.

---

### Iterative Solution

```java
public static int sum(int n) {

    int result = 0;

    for (int i = 1; i <= n; i++) {

        result += i;
    }

    return result;
}
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

The iterative version uses constant additional memory.

---

# 20. Time-Space Tradeoff

Sometimes we deliberately use more memory to make an algorithm faster.

This is called:

> **Time-Space Tradeoff**

Consider the Two Sum problem.

Input:

```text
numbers = [2, 7, 11, 15]
target = 9
```

We need to find:

```text
2 + 7 = 9
```

---

## Approach 1 — Brute Force

We compare every pair.

```java
for (int i = 0; i < numbers.length; i++) {

    for (int j = i + 1; j < numbers.length; j++) {

        if (numbers[i] + numbers[j] == target) {
            // Found
        }
    }
}
```

Complexity:

```text
Time:  O(n²)
Space: O(1)
```

---

## Approach 2 — HashMap

We can store values in a HashMap.

```java
HashMap<Integer, Integer> map = new HashMap<>();

for (int i = 0; i < numbers.length; i++) {

    int required = target - numbers[i];

    if (map.containsKey(required)) {
        // Found
    }

    map.put(numbers[i], i);
}
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

We use more memory:

```text
O(n)
```

to improve the time:

```text
O(n²) → O(n)
```

This is a classic example of a Time-Space Tradeoff.

---

# 21. In-Place Algorithms

An **in-place algorithm** modifies the input data directly instead of creating another data structure.

Example:

```java
public static void reverse(int[] numbers) {

    int left = 0;
    int right = numbers.length - 1;

    while (left < right) {

        int temp = numbers[left];

        numbers[left] = numbers[right];

        numbers[right] = temp;

        left++;
        right--;
    }
}
```

We don't create another array.

We only use:

```text
left
right
temp
```

Therefore:

```text
Time Complexity = O(n)
Auxiliary Space = O(1)
```

This is called an **in-place solution**.

---

# 22. Example: Reverse Array

Input:

```text
[10, 20, 30, 40, 50]
```

We use two pointers:

```text
left  → 10
right → 50
```

Swap:

```text
[50, 20, 30, 40, 10]
```

Move pointers:

```text
left  → 20
right → 40
```

Swap:

```text
[50, 40, 30, 20, 10]
```

Continue until:

```text
left >= right
```

Final result:

```text
[50, 40, 30, 20, 10]
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 23. O(1) vs O(n) Space

Let's compare two approaches.

### Approach A

```java
int[] result = new int[n];
```

Space:

```text
O(n)
```

---

### Approach B

Modify the existing array:

```java
int temp = numbers[left];

numbers[left] = numbers[right];

numbers[right] = temp;
```

Additional space:

```text
O(1)
```

Therefore, when possible, an in-place solution can reduce auxiliary space.

---

# 24. Best, Average and Worst Space

Just like time complexity, space usage can sometimes vary depending on the input.

However, when discussing space complexity in interviews, we commonly focus on the **maximum additional memory required**.

For example, a HashMap might contain:

```text
1 element
```

for one input and:

```text
n elements
```

for another input.

In the worst case:

```text
Space = O(n)
```

---

# 25. Space Complexity and Input Constraints

Always check the input constraints.

Suppose:

```text
1 <= n <= 100
```

Using:

```text
O(n)
```

additional memory is usually not concerning.

But suppose:

```text
1 <= n <= 10,000,000
```

An `O(n)` additional array could require significant memory.

Therefore:

> **Input constraints help determine whether your space complexity is acceptable.**

---

# 26. Common Space Complexities

| Space Complexity | Meaning | Example |
|---|---|---|
| `O(1)` | Constant | Few variables |
| `O(log n)` | Logarithmic | Some recursive/divide-and-conquer algorithms |
| `O(n)` | Linear | Array, HashMap, recursion depth |
| `O(n log n)` | Linearithmic | Some algorithms/data structures |
| `O(n²)` | Quadratic | Matrix |
| `O(2ⁿ)` | Exponential | Some recursive/backtracking algorithms |

---

# 27. How to Calculate Space Complexity

When analyzing a problem, follow these steps.

## Step 1 — Identify the input

Example:

```java
int[] numbers
```

This is input.

Don't automatically count it as auxiliary space.

---

## Step 2 — Look for additional data structures

Check whether the algorithm creates:

```text
Array
ArrayList
HashMap
HashSet
Stack
Queue
Matrix
```

---

## Step 3 — Check recursion

Ask:

> How deep can the recursive call stack become?

For:

```java
solve(n - 1)
```

called `n` times:

```text
Space = O(n)
```

---

## Step 4 — Look at variables

A fixed number of variables:

```text
O(1)
```

Variables that depend on `n`:

```text
O(n)
```

---

## Step 5 — Keep the largest space requirement

Suppose an algorithm uses:

```text
O(n) array
+
O(n) HashMap
+
O(1) variables
```

Total:

```text
O(n) + O(n) + O(1)
```

Simplify:

```text
O(n)
```

---

# 28. Space Complexity Examples

## Example 1

```java
int a = 10;
int b = 20;
int c = a + b;
```

Answer:

```text
O(1)
```

---

## Example 2

```java
int[] result = new int[n];
```

Answer:

```text
O(n)
```

---

## Example 3

```java
int[][] matrix = new int[n][n];
```

Answer:

```text
O(n²)
```

---

## Example 4

```java
HashMap<Integer, Integer> map = new HashMap<>();

for (int i = 0; i < n; i++) {
    map.put(i, i);
}
```

Answer:

```text
O(n)
```

---

## Example 5

```java
public static void print(int n) {

    if (n == 0) {
        return;
    }

    print(n - 1);
}
```

Answer:

```text
O(n)
```

because of the recursive call stack.

---

# 29. Important Interview Concept: Does the Input Count?

Suppose we have:

```java
public static void print(int[] numbers) {

    for (int number : numbers) {

        System.out.println(number);
    }
}
```

The input array contains `n` elements.

The input itself uses:

```text
O(n)
```

memory.

But the algorithm doesn't create additional storage proportional to `n`.

Therefore:

```text
Auxiliary Space = O(1)
```

A good interview answer is:

> "The input array takes O(n) space, but the algorithm uses O(1) auxiliary space."

This shows a strong understanding of space complexity.

---

# 30. Interview Questions

## Question 1

What is Space Complexity?

### Answer

Space Complexity describes how the memory requirements of an algorithm grow as the input size increases.

---

## Question 2

What is Auxiliary Space?

### Answer

Auxiliary Space is the additional memory used by an algorithm excluding the input data.

---

## Question 3

What is the space complexity of:

```java
int[] result = new int[n];
```

### Answer

```text
O(n)
```

because the array contains `n` elements.

---

## Question 4

What is the space complexity of:

```java
int[][] matrix = new int[n][n];
```

### Answer

```text
O(n²)
```

because the matrix contains `n × n` elements.

---

## Question 5

What is the auxiliary space of a simple loop?

```java
for (int i = 0; i < n; i++) {

    System.out.println(i);
}
```

### Answer

```text
O(1)
```

The loop doesn't create additional memory proportional to `n`.

---

## Question 6

What is the space complexity of recursive factorial?

```java
factorial(n)
```

### Answer

```text
O(n)
```

because there can be `n` recursive calls on the call stack.

---

## Question 7

What is the space complexity of Two Sum using a HashMap?

### Answer

```text
O(n)
```

because the HashMap can store up to `n` elements.

---

## Question 8

What is an in-place algorithm?

### Answer

An in-place algorithm modifies the input directly and uses only a small amount of additional memory.

Example:

```text
Reverse an array using two pointers
```

with:

```text
Auxiliary Space = O(1)
```

---

## Question 9

What is a Time-Space Tradeoff?

### Answer

It means using additional memory to improve execution time.

For example:

```text
Brute Force Two Sum:
Time:  O(n²)
Space: O(1)

HashMap Two Sum:
Time:  O(n)
Space: O(n)
```

---

# 31. Practice Problems

Try solving these before checking the answers.

## Problem 1

What is the auxiliary space?

```java
public static int sum(int a, int b) {

    int result = a + b;

    return result;
}
```

---

## Problem 2

What is the auxiliary space?

```java
int[] result = new int[n];
```

---

## Problem 3

What is the auxiliary space?

```java
HashMap<Integer, Integer> map = new HashMap<>();

for (int i = 0; i < n; i++) {

    map.put(i, i);
}
```

---

## Problem 4

What is the auxiliary space?

```java
int[][] matrix = new int[n][n];
```

---

## Problem 5

What is the auxiliary space?

```java
public static void count(int n) {

    if (n == 0) {
        return;
    }

    count(n - 1);
}
```

---

## Problem 6

What is the auxiliary space?

```java
public static void print(int[] numbers) {

    for (int number : numbers) {

        System.out.println(number);
    }
}
```

---

## Problem 7

Which solution uses less auxiliary space?

### Solution A

```java
int[] result = new int[n];
```

### Solution B

```java
int temp;
int left;
int right;
```

Explain why.

---

## Problem 8

What is the space complexity?

```java
for (int i = 0; i < n; i++) {

    int[] temp = new int[n];
}
```

Think carefully about whether the arrays are all alive at the same time.

---

# 32. Quick Revision

Remember these rules:

```text
Fixed number of variables
→ O(1)
```

```text
Array of size n
→ O(n)
```

```text
HashMap containing n elements
→ O(n)
```

```text
HashSet containing n elements
→ O(n)
```

```text
Matrix n × n
→ O(n²)
```

```text
Recursion depth n
→ O(n)
```

```text
Two-dimensional structure n × m
→ O(n × m)
```

---

# 33. Time vs Space Complexity

Always analyze both.

| Concept | Question |
|---|---|
| Time Complexity | How much work does the algorithm perform? |
| Space Complexity | How much additional memory does it use? |

For example:

```text
HashMap Two Sum

Time:  O(n)
Space: O(n)
```

Whereas:

```text
Brute Force Two Sum

Time:  O(n²)
Space: O(1)
```

---

# 34. Final Summary

Space Complexity tells us how the memory requirements of an algorithm grow with the input size.

The most important concepts are:

```text
O(1)
→ Constant Space

O(n)
→ Linear Space

O(n²)
→ Quadratic Space

Auxiliary Space
→ Extra memory used by the algorithm

Input Space
→ Memory required to store the input

Call Stack
→ Memory used by recursive function calls

In-Place Algorithm
→ Uses very little additional memory
```

The most important interview habit is:

> **Whenever you solve a DSA problem, always ask both "What is my time complexity?" and "What is my auxiliary space complexity?"**

A good solution is not just one that works.

It should also use an appropriate amount of:

```text
Time
+
Memory
```

---

# 35. What to Remember

Before moving to the next topic, make sure you can explain these without looking at the notes:

```text
1. What is Space Complexity?
2. What is Auxiliary Space?
3. What is the difference between Input Space and Auxiliary Space?
4. Why is an array of size n O(n)?
5. Why is a matrix n × n O(n²)?
6. Why does recursion use additional space?
7. What is an in-place algorithm?
8. What is a Time-Space Tradeoff?
9. Why does HashMap usually require O(n) space?
10. How do you calculate auxiliary space?
```

> **Remember: Efficient programming is about managing both time and memory.**