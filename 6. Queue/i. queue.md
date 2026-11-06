# Queue Data Structure

A **Queue** is a linear data structure that follows the **FIFO (First In, First Out)** principle.

This means the element inserted first will be removed first.

Think of people standing in a line:

- The first person to enter the line gets served first.
- New people join at the end.
- People leave from the front.

Example:

```text
FRONT                         REAR
  ↓                             ↓
[10] → [20] → [30] → [40]
```

If we remove an element, `10` will be removed first.

---

# 1. What Is FIFO?

FIFO means:

**First In, First Out**

Suppose we insert:

```text
10
20
30
```

Queue:

```text
FRONT
  ↓
[10] → [20] → [30]
                  ↑
                 REAR
```

Now perform a dequeue operation.

```text
FRONT
  ↓
[20] → [30]
         ↑
        REAR
```

The value `10` is removed because it was inserted first.

---

# 2. Basic Queue Operations

| Operation | Description |
|---|---|
| enqueue | Insert an element at the rear |
| dequeue | Remove an element from the front |
| peek | View the front element |
| isEmpty | Check whether the queue is empty |
| size | Return the number of elements |

Example:

### Enqueue

Insert `40`:

```text
[10] → [20] → [30]
```

After:

```text
[10] → [20] → [30] → [40]
```

### Dequeue

Remove the front element:

```text
[10] → [20] → [30] → [40]
```

After:

```text
[20] → [30] → [40]
```

### Peek

Returns:

```text
20
```

The queue remains unchanged.

---

# 3. Implementing Queue in Java

Java provides the `Queue` interface.

A common implementation is:

```java
Queue<Integer> queue = new LinkedList<>();
```

However, for most ordinary queue operations, `ArrayDeque` is generally preferred because it avoids the extra node allocation used by `LinkedList`.

Example:

```java
import java.util.Queue;
import java.util.ArrayDeque;

public class Main {

    public static void main(String[] args) {

        Queue<Integer> queue = new ArrayDeque<>();

        // Insert elements
        queue.offer(10);
        queue.offer(20);
        queue.offer(30);

        System.out.println(queue);

        // View front element
        System.out.println("Front: " + queue.peek());

        // Remove front element
        System.out.println("Removed: " + queue.poll());

        System.out.println(queue);

        System.out.println("Size: " + queue.size());

        System.out.println("Empty: " + queue.isEmpty());
    }
}
```

Output:

```text
[10, 20, 30]
Front: 10
Removed: 10
[20, 30]
Size: 2
Empty: false
```

---

# 4. Important Queue Methods in Java

| Method | Description |
|---|---|
| `offer(value)` | Insert element |
| `poll()` | Remove and return front element |
| `peek()` | Return front without removing |
| `isEmpty()` | Check whether empty |
| `size()` | Return number of elements |
| `contains(value)` | Check whether an element exists |

Important difference:

```java
queue.poll();
```

Returns `null` if the queue is empty.

```java
queue.remove();
```

Throws `NoSuchElementException` if the queue is empty.

Similarly:

```java
queue.peek();
```

Returns `null` if empty.

```java
queue.element();
```

Throws `NoSuchElementException` if empty.

For safer queue operations, `offer()`, `poll()`, and `peek()` are often convenient.

---

# 5. Understanding Queue Operations

Initial queue:

```text
FRONT
  ↓
[10] → [20] → [30]
                  ↑
                 REAR
```

Perform:

```java
queue.offer(40);
```

Now:

```text
[10] → [20] → [30] → [40]
```

Perform:

```java
queue.poll();
```

Now:

```text
[20] → [30] → [40]
```

Perform:

```java
queue.peek();
```

Output:

```text
20
```

No element is removed.

---

# 6. Implementing Queue Using an Array

Understanding the internal implementation is important for interviews.

We can implement a basic queue using an array.

We maintain two indexes:

```text
front
rear
```

Initially:

```text
front = 0
rear = -1
```

However, a simple array implementation needs careful handling of removed positions. A circular queue solves this problem and is generally the better fixed-size array implementation.

---

# 7. Circular Queue

A **Circular Queue** is a queue in which the last array position connects logically back to the first position.

Why do we need it?

Consider an array of capacity `5`.

Initially:

```text
[10, 20, 30, 40, 50]
```

Remove three elements:

```text
[_, _, _, 40, 50]
```

There are now three unused positions at the beginning.

A simple linear queue may not reuse those positions if its rear has reached the end.

A circular queue reuses them by wrapping around to the beginning.

Conceptually:

```text
       [0]
    /       \
  [4]       [1]
   |         |
  [3] ←──── [2]
```

The next position is calculated using:

```java
(rear + 1) % capacity
```

The modulo operator brings the index back to zero after the last position.

---

# 8. Circular Queue Implementation in Java

```java
class CircularQueue {

    private int[] arr;
    private int front;
    private int rear;
    private int size;
    private int capacity;

    CircularQueue(int capacity) {

        this.capacity = capacity;
        this.arr = new int[capacity];

        this.front = 0;
        this.rear = -1;
        this.size = 0;
    }

    // Insert element
    public void enqueue(int value) {

        if (isFull()) {
            throw new IllegalStateException("Queue is full");
        }

        rear = (rear + 1) % capacity;

        arr[rear] = value;

        size++;
    }

    // Remove element
    public int dequeue() {

        if (isEmpty()) {
            throw new IllegalStateException("Queue is empty");
        }

        int value = arr[front];

        front = (front + 1) % capacity;

        size--;

        if (size == 0) {
            front = 0;
            rear = -1;
        }

        return value;
    }

    // Get front element
    public int peek() {

        if (isEmpty()) {
            throw new IllegalStateException("Queue is empty");
        }

        return arr[front];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == capacity;
    }

    public int size() {
        return size;
    }

    public void display() {

        if (isEmpty()) {
            System.out.println("Queue is empty");
            return;
        }

        for (int i = 0; i < size; i++) {

            int index = (front + i) % capacity;

            System.out.print(arr[index] + " ");
        }

        System.out.println();
    }
}
```

---

# 9. Circular Queue Dry Run

Suppose:

```java
CircularQueue queue = new CircularQueue(5);
```

Initially:

```text
front = 0
rear = -1
size = 0
```

Enqueue:

```java
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
queue.enqueue(40);
```

Array:

```text
[10, 20, 30, 40, _]
```

Now dequeue twice:

```java
queue.dequeue();
queue.dequeue();
```

Logical queue:

```text
[30, 40]
```

Indexes:

```text
front = 2
rear = 3
```

Now enqueue:

```java
queue.enqueue(50);
queue.enqueue(60);
queue.enqueue(70);
```

The rear wraps around.

Physical array:

```text
[60, 70, 30, 40, 50]
```

Logical queue:

```text
FRONT
  ↓
[30] → [40] → [50] → [60] → [70]
                                  ↑
                                 REAR
```

The logical order is maintained even though the physical array wraps around.

---

# 10. Circular Queue Complexity

| Operation | Time Complexity |
|---|---:|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| isEmpty | O(1) |
| isFull | O(1) |
| Size | O(1) |
| Display | O(n) |

---

# 11. Queue Implementation Using a Linked List

A queue can also be implemented using a linked list.

We maintain:

```text
front
rear
```

Insertion happens at the rear.

Deletion happens at the front.

Structure:

```text
FRONT                      REAR
  ↓                          ↓
[10] → [20] → [30] → [40] → null
```

Java implementation:

```java
class LinkedListQueue {

    static class Node {

        int data;
        Node next;

        Node(int data) {
            this.data = data;
        }
    }

    private Node front;
    private Node rear;
    private int size;

    // Enqueue
    public void enqueue(int value) {

        Node newNode = new Node(value);

        if (rear == null) {
            front = rear = newNode;
        } else {
            rear.next = newNode;
            rear = newNode;
        }

        size++;
    }

    // Dequeue
    public int dequeue() {

        if (front == null) {
            throw new IllegalStateException("Queue is empty");
        }

        int value = front.data;

        front = front.next;

        if (front == null) {
            rear = null;
        }

        size--;

        return value;
    }

    // Peek
    public int peek() {

        if (front == null) {
            throw new IllegalStateException("Queue is empty");
        }

        return front.data;
    }

    public boolean isEmpty() {
        return front == null;
    }

    public int size() {
        return size;
    }

    public void display() {

        Node current = front;

        while (current != null) {

            System.out.print(current.data + " ");

            current = current.next;
        }

        System.out.println();
    }
}
```

Complexity:

```text
Enqueue: O(1)
Dequeue: O(1)
Peek:    O(1)
```

---

# 12. What Is a Deque?

Deque means **Double-Ended Queue**.

It allows insertion and deletion from both ends.

Unlike a normal queue, we can:

- Insert at the front.
- Insert at the rear.
- Remove from the front.
- Remove from the rear.

Example:

```text
FRONT                         REAR
  ↓                             ↓
[10] ⇄ [20] ⇄ [30] ⇄ [40]
```

Java implementation:

```java
Deque<Integer> deque = new ArrayDeque<>();
```

Example:

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Main {

    public static void main(String[] args) {

        Deque<Integer> deque = new ArrayDeque<>();

        deque.offerFirst(10);
        deque.offerLast(20);
        deque.offerLast(30);

        System.out.println(deque);

        deque.pollFirst();
        deque.pollLast();

        System.out.println(deque);
    }
}
```

Output:

```text
[10, 20, 30]
[20]
```

Important methods:

| Method | Description |
|---|---|
| `offerFirst()` | Insert at front |
| `offerLast()` | Insert at rear |
| `pollFirst()` | Remove from front |
| `pollLast()` | Remove from rear |
| `peekFirst()` | View front |
| `peekLast()` | View rear |

---

# 13. Stack vs Queue vs Deque

| Feature | Stack | Queue | Deque |
|---|---|---|---|
| Principle | LIFO | FIFO | Both ends |
| Insert at front | No | No | Yes |
| Insert at rear | Top only | Yes | Yes |
| Remove from front | Top only | Yes | Yes |
| Remove from rear | No | No | Yes |
| Java type | `Deque` | `Queue` | `Deque` |

A Java `Deque` can also be used as a stack:

```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);

System.out.println(stack.pop());
```

---

# 14. What Is a Priority Queue?

A **Priority Queue** is a queue where elements are removed according to priority rather than insertion order.

In Java, `PriorityQueue` uses a heap internally.

By default, Java's `PriorityQueue` is a min-heap.

Example:

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.offer(30);
pq.offer(10);
pq.offer(20);

System.out.println(pq.poll());
```

Output:

```text
10
```

The smallest element is removed first.

Example:

```text
Priority Queue:

10 ← Highest priority
20
30
```

For a max-heap:

```java
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Collections.reverseOrder());
```

Now the largest element is removed first.

```java
maxHeap.offer(30);
maxHeap.offer(10);
maxHeap.offer(20);

System.out.println(maxHeap.poll());
```

Output:

```text
30
```

We will study heaps and priority queues in more detail in:

```text
09-heap/heap-priority-queue.md
```

---

# 15. Pattern 1 — Queue Using Two Stacks

Interview problem:

Implement a queue using two stacks.

We need FIFO behavior using LIFO data structures.

Use:

```text
inputStack
outputStack
```

### Approach

For enqueue:

- Push the new value into `inputStack`.

For dequeue:

- If `outputStack` is empty, transfer all elements from `inputStack`.
- Pop from `outputStack`.

### Java Implementation

```java
class QueueUsingStacks {

    private Deque<Integer> inputStack = new ArrayDeque<>();
    private Deque<Integer> outputStack = new ArrayDeque<>();

    public void enqueue(int value) {
        inputStack.push(value);
    }

    public int dequeue() {

        moveElements();

        if (outputStack.isEmpty()) {
            throw new IllegalStateException("Queue is empty");
        }

        return outputStack.pop();
    }

    public int peek() {

        moveElements();

        if (outputStack.isEmpty()) {
            throw new IllegalStateException("Queue is empty");
        }

        return outputStack.peek();
    }

    private void moveElements() {

        if (outputStack.isEmpty()) {

            while (!inputStack.isEmpty()) {
                outputStack.push(inputStack.pop());
            }
        }
    }

    public boolean isEmpty() {
        return inputStack.isEmpty() && outputStack.isEmpty();
    }
}
```

### Dry Run

Enqueue:

```text
10, 20, 30
```

Input stack:

```text
30 ← TOP
20
10
```

Output stack:

```text
Empty
```

Now dequeue.

Move all elements:

```text
Input Stack: Empty

Output Stack:

10 ← TOP
20
30
```

Pop:

```text
10
```

The first inserted element is removed first.

Complexity:

| Operation | Complexity |
|---|---:|
| Enqueue | O(1) |
| Dequeue | O(1) amortized |
| Peek | O(1) amortized |

Each element is transferred at most once.

---

# 16. Pattern 2 — First Non-Repeating Character in a Stream

Problem:

Characters arrive one by one.

After every character, find the first non-repeating character.

Example:

```text
Input: a a b c
```

Output:

```text
a -1 b b
```

Explanation:

| New Character | Current Stream | First Non-Repeating |
|---|---|---|
| a | a | a |
| a | aa | -1 |
| b | aab | b |
| c | aabc | b |

Use:

- Queue to maintain character order.
- Frequency array to count occurrences.

Java:

```java
static List<Character> firstNonRepeating(char[] stream) {

    int[] frequency = new int[26];

    Queue<Character> queue = new ArrayDeque<>();

    List<Character> result = new ArrayList<>();

    for (char c : stream) {

        frequency[c - 'a']++;

        queue.offer(c);

        while (!queue.isEmpty() &&
               frequency[queue.peek() - 'a'] > 1) {

            queue.poll();
        }

        if (queue.isEmpty()) {
            result.add('#');
        } else {
            result.add(queue.peek());
        }
    }

    return result;
}
```

This implementation assumes lowercase English letters.

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# 17. Pattern 3 — Sliding Window Maximum

Problem:

Find the maximum value in every window of size `k`.

Example:

```text
Input:
[1, 3, -1, -3, 5, 3, 6, 7]

k = 3

Output:
[3, 3, 5, 5, 6, 7]
```

We use a **Deque** to store indexes of useful elements.

The deque maintains decreasing values from front to back.

Java:

```java
static int[] maxSlidingWindow(int[] nums, int k) {

    if (nums == null || k <= 0 || k > nums.length) {
        return new int[0];
    }

    int n = nums.length;

    int[] result = new int[n - k + 1];

    Deque<Integer> deque = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {

        // Remove indexes outside the current window
        while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
            deque.pollFirst();
        }

        // Remove smaller values from the back
        while (!deque.isEmpty() &&
               nums[deque.peekLast()] <= nums[i]) {

            deque.pollLast();
        }

        deque.offerLast(i);

        // Store maximum once the first window is complete
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }

    return result;
}
```

Complexity:

```text
Time:  O(n)
Space: O(k)
```

Why O(n)?

Every index enters the deque once and leaves it at most once.

---

# 18. Pattern 4 — BFS Using Queue

Queue is an important data structure for graph and tree traversal.

BFS means **Breadth-First Search**.

It visits nodes level by level.

Example tree:

```text
        1
       / \
      2   3
     / \
    4   5
```

BFS traversal:

```text
1 → 2 → 3 → 4 → 5
```

Java:

```java
static void bfs(TreeNode root) {

    if (root == null) {
        return;
    }

    Queue<TreeNode> queue = new ArrayDeque<>();

    queue.offer(root);

    while (!queue.isEmpty()) {

        TreeNode current = queue.poll();

        System.out.print(current.val + " ");

        if (current.left != null) {
            queue.offer(current.left);
        }

        if (current.right != null) {
            queue.offer(current.right);
        }
    }
}
```

This is a preview of tree and graph traversal.

You will study BFS in detail in:

```text
10-graphs/bfs.md
```

---

# 19. Real-World Applications of Queue

| Application | Explanation |
|---|---|
| Printer queue | Documents are printed in order |
| CPU scheduling | Processes wait for execution |
| Customer support | Requests wait for service |
| Messaging systems | Messages are processed in sequence |
| BFS | Visits nodes level by level |
| Task scheduling | Tasks wait for execution |
| Network packets | Packets can be processed in arrival order |
| Streaming data | Incoming elements are processed sequentially |

---

# 20. Queue vs Stack vs Linked List

| Feature | Stack | Queue | Linked List |
|---|---|---|---|
| Principle | LIFO | FIFO | Sequential node links |
| Insert | Top | Rear | Depends on operation |
| Delete | Top | Front | Depends on operation |
| Main operations | Push / Pop | Enqueue / Dequeue | Insert / Delete |
| Java structure | `Deque` | `Queue` | `LinkedList` or custom nodes |

---

# 21. Time Complexity Cheat Sheet

| Operation | Time Complexity |
|---|---:|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| isEmpty | O(1) |
| Size | O(1) |
| Search | O(n) |
| Circular queue insertion | O(1) |
| Circular queue deletion | O(1) |
| Deque insertion/removal at ends | O(1) |
| Priority queue insertion | O(log n) |
| Priority queue removal | O(log n) |
| BFS | O(V + E) |

For `ArrayDeque`, ordinary operations at either end are amortized O(1).

---

# 22. Common Mistakes

### Mistake 1 — Confusing FIFO and LIFO

```text
Stack → LIFO

Queue → FIFO
```

### Mistake 2 — Removing from the wrong end

Normal queue:

```text
Insertion → Rear

Removal → Front
```

### Mistake 3 — Forgetting empty queue checks

Before accessing the front element, check:

```java
if (!queue.isEmpty()) {
    queue.peek();
}
```

### Mistake 4 — Incorrect circular queue index

Use:

```java
(rear + 1) % capacity
```

This ensures the index wraps around.

### Mistake 5 — Not updating rear after deleting the last node

In a linked-list queue, when the last node is removed:

```java
front = null;
rear = null;
```

Otherwise, the queue can become inconsistent.

### Mistake 6 — Confusing Queue and PriorityQueue

A normal queue follows insertion order.

A priority queue removes elements according to priority.

---

# 23. Interview Questions

## Beginner

1. What is a Queue?
2. Explain FIFO.
3. What are enqueue and dequeue?
4. What is the difference between Stack and Queue?
5. How do you implement a queue using an array?
6. How do you implement a queue using a linked list?
7. What is a circular queue?
8. What is a deque?
9. What is a priority queue?

## Intermediate

1. Implement a circular queue.
2. Implement a queue using two stacks.
3. Implement a stack using two queues.
4. Find the first non-repeating character in a stream.
5. Generate binary numbers from 1 to N using a queue.
6. Implement a deque.
7. Find the maximum in every sliding window.
8. Perform level-order traversal of a binary tree.

## Advanced

1. Sliding Window Maximum.
2. Rotten Oranges.
3. Shortest path in an unweighted graph.
4. First negative number in every window.
5. Design a task scheduling system.
6. Implement an LRU cache using a HashMap and doubly linked list.

---

# 24. Practice Problems

## Easy

- Implement Queue using an array.
- Implement Queue using a linked list.
- Implement Circular Queue.
- Implement Stack using Queue.
- Generate binary numbers from 1 to N.
- Reverse the first K elements of a queue.
- Find the first non-repeating character in a stream.

## Medium

- Implement Queue using Two Stacks.
- First Negative Number in Every Window.
- Rotten Oranges.
- Number of Recent Calls.
- Sliding Window Maximum.
- Binary Tree Level Order Traversal.
- Rotting Oranges.

## Hard

- Sliding Window Maximum.
- Shortest Path in an Unweighted Graph.
- Design LRU Cache.
- Shortest Subarray with Sum at Least K.
- Minimum Operations to Reach a Target State.

---

# 25. Quick Revision Cheat Sheet

```java
Queue<Integer> queue = new ArrayDeque<>();

// Insert
queue.offer(10);

// Remove front
queue.poll();

// View front
queue.peek();

// Check empty
queue.isEmpty();

// Size
queue.size();
```

For Deque:

```java
Deque<Integer> deque = new ArrayDeque<>();

deque.offerFirst(10);
deque.offerLast(20);

deque.pollFirst();
deque.pollLast();

deque.peekFirst();
deque.peekLast();
```

Remember:

```text
Queue → FIFO

Enqueue → Insert at rear

Dequeue → Remove from front

Peek → View front

Circular Queue → Reuse array positions

Deque → Insert and remove from both ends

PriorityQueue → Remove according to priority
```

---

# 26. Final Takeaway

A Queue is a linear data structure that follows:

```text
First In, First Out
```

Focus on these concepts:

1. FIFO principle
2. Enqueue and dequeue
3. Java Queue interface
4. Array-based queue
5. Circular queue
6. Linked-list queue
7. Deque
8. PriorityQueue
9. Queue using two stacks
10. Sliding Window Maximum
11. BFS using Queue

The most important Java operations are:

```java
queue.offer(value);
queue.poll();
queue.peek();
```

Practice implementing a circular queue and solving sliding-window problems. These are especially useful for interviews.

---

# Checklist

- [ ] Understand FIFO.
- [ ] Understand enqueue and dequeue.
- [ ] Use Java Queue interface.
- [ ] Implement queue using an array.
- [ ] Understand circular queue.
- [ ] Implement circular queue.
- [ ] Implement queue using a linked list.
- [ ] Understand Deque.
- [ ] Understand PriorityQueue.
- [ ] Implement queue using two stacks.
- [ ] Find first non-repeating character in a stream.
- [ ] Understand Sliding Window Maximum.
- [ ] Understand BFS using Queue.
- [ ] Learn queue time complexities.
- [ ] Practice easy and medium queue problems.

---

# Next Topic

```text
07-hashing/hashmap-hashset.md
```

Next, we will learn **Hashing, HashMap and HashSet**, including hashing fundamentals, collision handling, frequency counting, duplicate detection, and important interview problems.