# Stack Data Structure

A **Stack** is a linear data structure that follows the **LIFO (Last In, First Out)** principle.

This means the element inserted last will be removed first.

Think of a stack of plates:

- You place a new plate on top.
- You remove the top plate first.
- You cannot directly remove a plate from the middle without moving the plates above it.

Example:

```text
       TOP
        ↓
      [30]  ← Last inserted
      [20]
      [10]  ← First inserted
```

If we remove an element, `30` will be removed first.

---

# 1. What Is LIFO?

LIFO means:

**Last In, First Out**

Example:

We insert:

```text
10
20
30
```

Stack:

```text
      TOP
       ↓
     [30]
     [20]
     [10]
```

Now perform a pop operation.

```text
      TOP
       ↓
     [20]
     [10]
```

The value `30` is removed because it was inserted last.

---

# 2. Basic Stack Operations

A stack mainly supports these operations:

| Operation | Description |
|---|---|
| push() | Insert an element |
| pop() | Remove the top element |
| peek() | View the top element without removing it |
| isEmpty() | Check whether the stack is empty |
| size() | Return the number of elements |

Example:

```text
Initial Stack:

[10]
[20]
[30] ← TOP
```

### Push

Insert an element:

```java
stack.push(40);
```

Result:

```text
[10]
[20]
[30]
[40] ← TOP
```

### Pop

Remove the top element:

```java
stack.pop();
```

Result:

```text
[10]
[20]
[30] ← TOP
```

### Peek

View the top element:

```java
stack.peek();
```

Output:

```text
30
```

The stack remains unchanged.

---

# 3. Implementing Stack in Java

Java provides several ways to implement a stack.

The recommended modern approach is to use:

```java
Deque<Integer> stack = new ArrayDeque<>();
```

Java also provides the older `Stack` class, but `ArrayDeque` is generally preferred for ordinary single-threaded stack operations.

Import:

```java
import java.util.ArrayDeque;
import java.util.Deque;
```

Example:

```java
public class Main {

    public static void main(String[] args) {

        Deque<Integer> stack = new ArrayDeque<>();

        // Push elements
        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println(stack);

        // Peek
        System.out.println("Top: " + stack.peek());

        // Pop
        System.out.println("Removed: " + stack.pop());

        // Current stack
        System.out.println(stack);

        // Size
        System.out.println("Size: " + stack.size());

        // Empty check
        System.out.println("Empty: " + stack.isEmpty());
    }
}
```

Output:

```text
[30, 20, 10]
Top: 30
Removed: 30
[20, 10]
Size: 2
Empty: false
```

---

# 4. Stack Methods in Java

| Method | Purpose |
|---|---|
| `push(value)` | Add element to top |
| `pop()` | Remove and return top |
| `peek()` | Return top without removing |
| `isEmpty()` | Check whether empty |
| `size()` | Return number of elements |
| `contains(value)` | Check whether element exists |
| `clear()` | Remove all elements |

Important:

Calling `pop()` or `peek()` on an empty `ArrayDeque` throws `NoSuchElementException`.

You can check first:

```java
if (!stack.isEmpty()) {
    int value = stack.pop();
}
```

---

# 5. Stack Implementation Using an Array

Understanding the internal implementation is important for interviews.

We can implement a stack using an array and a variable called `top`.

Initially:

```text
top = -1
```

Why `-1`?

Because the stack is empty and no array index is occupied.

## Java Implementation

```java
class ArrayStack {

    private int[] arr;
    private int top;
    private int capacity;

    ArrayStack(int capacity) {

        this.capacity = capacity;
        this.arr = new int[capacity];
        this.top = -1;
    }

    // Insert an element
    public void push(int value) {

        if (top == capacity - 1) {
            throw new IllegalStateException("Stack Overflow");
        }

        arr[++top] = value;
    }

    // Remove the top element
    public int pop() {

        if (isEmpty()) {
            throw new IllegalStateException("Stack Underflow");
        }

        return arr[top--];
    }

    // View the top element
    public int peek() {

        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }

        return arr[top];
    }

    // Check whether stack is empty
    public boolean isEmpty() {
        return top == -1;
    }

    // Check whether stack is full
    public boolean isFull() {
        return top == capacity - 1;
    }

    // Get stack size
    public int size() {
        return top + 1;
    }

    // Display stack
    public void display() {

        for (int i = top; i >= 0; i--) {
            System.out.println(arr[i]);
        }
    }
}
```

---

# 6. Understanding Array Stack Operations

Suppose:

```java
ArrayStack stack = new ArrayStack(5);
```

Initially:

```text
Array: [_, _, _, _, _]

top = -1
```

Push `10`:

```java
stack.push(10);
```

Now:

```text
[10, _, _, _, _]

top = 0
```

Push `20`:

```java
stack.push(20);
```

Now:

```text
[10, 20, _, _, _]

top = 1
```

Push `30`:

```java
stack.push(30);
```

Now:

```text
[10, 20, 30, _, _]

top = 2
```

Pop:

```java
stack.pop();
```

Returned value:

```text
30
```

Now:

```text
[10, 20, _, _, _]

top = 1
```

The top index moves backward.

---

# 7. Stack Overflow and Stack Underflow

## Stack Overflow

Stack overflow in an array-based implementation happens when we try to insert an element into a full stack.

Example:

```text
Capacity = 3

[10]
[20]
[30] ← TOP
```

Trying to push `40` causes overflow.

```java
if (top == capacity - 1) {
    throw new IllegalStateException("Stack Overflow");
}
```

## Stack Underflow

Stack underflow happens when we try to remove an element from an empty stack.

```java
if (isEmpty()) {
    throw new IllegalStateException("Stack Underflow");
}
```

---

# 8. Stack Implementation Using a Linked List

A stack can also be implemented using a linked list.

We insert and delete nodes at the head.

Example:

```text
TOP
 ↓
30 → 20 → 10 → null
```

Here, `30` is the top element.

## Java Implementation

```java
class LinkedListStack {

    static class Node {

        int data;
        Node next;

        Node(int data) {
            this.data = data;
        }
    }

    private Node top;
    private int size;

    // Push
    public void push(int value) {

        Node newNode = new Node(value);

        newNode.next = top;

        top = newNode;

        size++;
    }

    // Pop
    public int pop() {

        if (top == null) {
            throw new IllegalStateException("Stack Underflow");
        }

        int value = top.data;

        top = top.next;

        size--;

        return value;
    }

    // Peek
    public int peek() {

        if (top == null) {
            throw new IllegalStateException("Stack is empty");
        }

        return top.data;
    }

    public boolean isEmpty() {
        return top == null;
    }

    public int size() {
        return size;
    }
}
```

Complexity:

```text
Push: O(1)
Pop:  O(1)
Peek: O(1)
```

---

# 9. Array Stack vs Linked List Stack

| Feature | Array Stack | Linked List Stack |
|---|---|---|
| Memory | Contiguous array | Separate nodes |
| Push | O(1), fixed array | O(1) |
| Pop | O(1) | O(1) |
| Peek | O(1) | O(1) |
| Capacity | Fixed in our implementation | Grows dynamically until memory limits |
| Extra memory | No per-element link | Stores a next reference |
| Implementation | Uses index | Uses node references |

Java's `ArrayDeque` grows dynamically when needed, unlike the fixed-capacity array implementation above.

---

# 10. Pattern 1 — Reverse a String Using Stack

Problem:

Reverse the string:

```text
hello
```

Expected output:

```text
olleh
```

Idea:

1. Push every character into the stack.
2. Pop characters one by one.
3. Append them to the result.

Java:

```java
static String reverseString(String s) {

    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {
        stack.push(c);
    }

    StringBuilder result = new StringBuilder();

    while (!stack.isEmpty()) {
        result.append(stack.pop());
    }

    return result.toString();
}
```

Dry run:

```text
Input: hello

Push:

h
e
l
l
o ← TOP

Pop:

o
l
l
e
h

Output: olleh
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 11. Pattern 2 — Valid Parentheses

This is one of the most popular stack interview problems.

Problem:

Check whether brackets are balanced.

Example:

```text
"()[]{}"
```

Output:

```text
true
```

Another example:

```text
"([)]"
```

Output:

```text
false
```

Why?

Because the opening and closing brackets do not match in the correct order.

## Approach

Use a stack.

1. If an opening bracket appears, push it.
2. If a closing bracket appears, check the top.
3. If the top matches, pop it.
4. Otherwise, return false.
5. At the end, the stack must be empty.

## Java Implementation

```java
static boolean isValid(String s) {

    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {

        if (c == '(' || c == '[' || c == '{') {

            stack.push(c);

        } else {

            if (stack.isEmpty()) {
                return false;
            }

            char top = stack.pop();

            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {

                return false;
            }
        }
    }

    return stack.isEmpty();
}
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 12. Valid Parentheses — Dry Run

Input:

```text
{[()]}
```

| Character | Operation | Stack |
|---|---|---|
| `{` | Push | `{` |
| `[` | Push | `{ [` |
| `(` | Push | `{ [ (` |
| `)` | Match and pop | `{ [` |
| `]` | Match and pop | `{` |
| `}` | Match and pop | Empty |

At the end:

```text
Stack is empty
```

Therefore:

```text
true
```

---

# 13. Pattern 3 — Next Greater Element

Problem:

For each array element, find the next greater element to its right.

Example:

```text
Input:  [4, 5, 2, 10, 8]

Output: [5, 10, 10, -1, -1]
```

Explanation:

| Element | Next Greater Element |
|---|---|
| 4 | 5 |
| 5 | 10 |
| 2 | 10 |
| 10 | -1 |
| 8 | -1 |

We use a **Monotonic Stack**.

A monotonic stack maintains elements in a particular increasing or decreasing order.

## Java Implementation

```java
static int[] nextGreaterElement(int[] arr) {

    int n = arr.length;

    int[] result = new int[n];

    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = n - 1; i >= 0; i--) {

        while (!stack.isEmpty() && stack.peek() <= arr[i]) {
            stack.pop();
        }

        if (stack.isEmpty()) {
            result[i] = -1;
        } else {
            result[i] = stack.peek();
        }

        stack.push(arr[i]);
    }

    return result;
}
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

Although there is a nested loop, each element is pushed and popped at most once.

Therefore, total time is O(n).

---

# 14. Pattern 4 — Min Stack

Problem:

Design a stack that supports:

- push
- pop
- top
- getMin

All operations should take O(1) time.

We can maintain two stacks:

1. Main stack
2. Minimum stack

## Java Implementation

```java
class MinStack {

    Deque<Integer> stack = new ArrayDeque<>();
    Deque<Integer> minStack = new ArrayDeque<>();

    public void push(int value) {

        stack.push(value);

        if (minStack.isEmpty() || value <= minStack.peek()) {
            minStack.push(value);
        }
    }

    public void pop() {

        if (stack.isEmpty()) {
            return;
        }

        int removed = stack.pop();

        if (removed == minStack.peek()) {
            minStack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```

Example:

Push:

```text
5, 3, 7, 2
```

Main stack:

```text
2
7
3
5
```

Minimum stack:

```text
2
3
5
```

Current minimum:

```text
2
```

After popping `2`, the minimum becomes:

```text
3
```

Complexity:

```text
Push:   O(1)
Pop:    O(1)
Top:    O(1)
GetMin: O(1)
```

---

# 15. Pattern 5 — Evaluate Postfix Expression

A postfix expression places the operator after its operands.

Example:

```text
2 3 +
```

This means:

```text
2 + 3 = 5
```

Another example:

```text
2 3 4 * +
```

Evaluation:

```text
3 * 4 = 12
2 + 12 = 14
```

Use a stack:

1. Push numbers.
2. When an operator appears, pop two numbers.
3. Apply the operator.
4. Push the result.

Java:

```java
static int evaluatePostfix(String[] tokens) {

    Deque<Integer> stack = new ArrayDeque<>();

    for (String token : tokens) {

        if (token.equals("+") ||
            token.equals("-") ||
            token.equals("*") ||
            token.equals("/")) {

            int b = stack.pop();
            int a = stack.pop();

            switch (token) {

                case "+":
                    stack.push(a + b);
                    break;

                case "-":
                    stack.push(a - b);
                    break;

                case "*":
                    stack.push(a * b);
                    break;

                case "/":
                    stack.push(a / b);
                    break;
            }

        } else {
            stack.push(Integer.parseInt(token));
        }
    }

    return stack.pop();
}
```

Example:

```java
String[] expression = {"2", "3", "4", "*", "+"};

System.out.println(evaluatePostfix(expression));
```

Output:

```text
14
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

This implementation assumes valid input, integer operands, and nonzero divisors.

---

# 16. Pattern 6 — Daily Temperatures

Problem:

Given daily temperatures, find how many days you must wait until a warmer temperature.

Example:

```text
Input:
[73, 74, 75, 71, 69, 72, 76, 73]

Output:
[1, 1, 4, 2, 1, 1, 0, 0]
```

We use a monotonic decreasing stack of indexes.

```java
static int[] dailyTemperatures(int[] temperatures) {

    int n = temperatures.length;

    int[] result = new int[n];

    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {

        while (!stack.isEmpty() &&
               temperatures[i] > temperatures[stack.peek()]) {

            int previousIndex = stack.pop();

            result[previousIndex] = i - previousIndex;
        }

        stack.push(i);
    }

    return result;
}
```

Why store indexes instead of temperatures?

Because we need to calculate:

```text
current index - previous index
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 17. Stack Applications

Stacks are useful in many programming problems.

| Application | Why Stack Is Useful |
|---|---|
| Function calls | Tracks method execution |
| Recursion | Uses the call stack |
| Undo/Redo | Stores previous actions |
| Browser navigation | Tracks history |
| Expression evaluation | Processes operators and operands |
| Parentheses validation | Matches opening and closing brackets |
| DFS | Can manage traversal |
| Monotonic stack | Finds next greater/smaller elements |
| Backtracking | Tracks choices and state |

---

# 18. Stack vs Queue

These are two important linear data structures.

| Feature | Stack | Queue |
|---|---|---|
| Principle | LIFO | FIFO |
| Insertion | Top | Rear |
| Removal | Top | Front |
| Main operation | Push / Pop | Offer / Poll |
| Example | Stack of plates | People in a line |

Stack:

```text
Push → [10] [20] [30] ← Pop
```

Queue:

```text
Remove ← [10] [20] [30] ← Insert
```

We will cover Queue in the next chapter.

---

# 19. Time Complexity Cheat Sheet

| Operation | Time Complexity |
|---|---:|
| Push | O(1) |
| Pop | O(1) |
| Peek | O(1) |
| isEmpty | O(1) |
| Size | O(1) |
| Search | O(n) |
| Reverse string using stack | O(n) |
| Valid parentheses | O(n) |
| Next greater element | O(n) |
| Min Stack operations | O(1) |

---

# 20. Common Mistakes

### Mistake 1: Popping an empty stack

Always check:

```java
if (!stack.isEmpty()) {
    stack.pop();
}
```

### Mistake 2: Confusing peek and pop

```java
stack.peek();
```

Returns the top element without removing it.

```java
stack.pop();
```

Removes and returns the top element.

### Mistake 3: Confusing LIFO and FIFO

```text
Stack → LIFO
Queue → FIFO
```

### Mistake 4: Incorrect operator order

For postfix evaluation:

```java
int b = stack.pop();
int a = stack.pop();
```

Then calculate:

```java
a - b
```

not:

```java
b - a
```

### Mistake 5: Using a stack when another pattern is simpler

Not every problem needs a stack. First identify whether the problem involves reverse order, nested structures, previous elements, or next greater/smaller elements.

---

# 21. Interview Questions

## Beginner

1. What is a stack?
2. Explain LIFO.
3. What are push, pop, and peek?
4. What is stack overflow?
5. What is stack underflow?
6. How do you implement a stack using an array?
7. How do you implement a stack using a linked list?
8. What is the difference between Stack and Queue?
9. Why is `ArrayDeque` preferred for ordinary stack operations in Java?

## Intermediate

1. Reverse a string using a stack.
2. Check valid parentheses.
3. Implement Min Stack.
4. Find the next greater element.
5. Evaluate a postfix expression.
6. Find the daily temperatures answer.
7. Remove adjacent duplicates from a string.
8. Simplify a Unix file path.

## Advanced

1. Largest rectangle in a histogram.
2. Maximal rectangle.
3. Trapping Rain Water using a stack.
4. Stock span problem.
5. Basic calculator.
6. Asteroid collision.
7. Online Stock Span.

---

# 22. Practice Problems

## Easy

- Implement stack using an array.
- Implement stack using a linked list.
- Reverse a string using a stack.
- Valid Parentheses.
- Remove adjacent duplicates.
- Implement Min Stack.

## Medium

- Next Greater Element.
- Daily Temperatures.
- Evaluate Reverse Polish Notation.
- Simplify Path.
- Asteroid Collision.
- Online Stock Span.

## Hard

- Largest Rectangle in Histogram.
- Maximal Rectangle.
- Trapping Rain Water.
- Basic Calculator.
- Sum of Subarray Minimums.

---

# 23. Quick Revision Cheat Sheet

```java
Deque<Integer> stack = new ArrayDeque<>();

// Insert
stack.push(10);

// Remove top
stack.pop();

// Read top
stack.peek();

// Check empty
stack.isEmpty();

// Size
stack.size();
```

Remember:

```text
LIFO = Last In, First Out

Push = Insert

Pop = Remove top

Peek = Read top
```

Pattern recognition:

| Problem Clue | Think |
|---|---|
| Reverse order | Stack |
| Balanced brackets | Stack |
| Nested structures | Stack |
| Next greater element | Monotonic Stack |
| Next smaller element | Monotonic Stack |
| Previous greater element | Monotonic Stack |
| Minimum in O(1) | Two Stacks |
| Expression evaluation | Stack |

---

# 24. Final Takeaway

A stack is a simple but powerful data structure.

The most important concept is:

```text
Last In, First Out
```

Remember these three operations:

```java
stack.push(value);
stack.pop();
stack.peek();
```

Focus on understanding:

1. Stack implementation using arrays.
2. Stack implementation using linked lists.
3. Valid parentheses.
4. Next greater element.
5. Min Stack.
6. Postfix expression evaluation.
7. Monotonic Stack.

Once these concepts are clear, many stack interview problems become easier to recognize and solve.

---

# Checklist

- [ ] Understand LIFO.
- [ ] Understand push, pop, and peek.
- [ ] Implement stack using an array.
- [ ] Implement stack using a linked list.
- [ ] Understand overflow and underflow.
- [ ] Use Java `ArrayDeque`.
- [ ] Reverse a string using stack.
- [ ] Solve Valid Parentheses.
- [ ] Understand Monotonic Stack.
- [ ] Solve Next Greater Element.
- [ ] Implement Min Stack.
- [ ] Evaluate postfix expressions.
- [ ] Solve Daily Temperatures.
- [ ] Understand stack complexity.
- [ ] Practice easy and medium stack problems.

---

# Next Topic

```text
06-queue/queue.md
```

Next, we will learn **Queue Data Structure**, including FIFO, enqueue, dequeue, circular queues, deque, priority queues, Java implementation, and interview problems.