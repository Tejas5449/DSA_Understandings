# Doubly Linked List

A **Doubly Linked List** is a linear data structure in which each node contains three parts:

1. Data
2. Reference to the previous node (`prev`)
3. Reference to the next node (`next`)

Unlike a singly linked list, a doubly linked list allows traversal in both directions.

Example:

```text
null ← 10 ⇄ 20 ⇄ 30 → null
```

---

# 1. Why Do We Need a Doubly Linked List?

In a singly linked list, each node only points to the next node.

```text
10 → 20 → 30 → 40 → null
```

We can move forward, but we cannot directly move backward.

For example, if we are at node `30` and want to reach `20`, we must start again from the head.

A doubly linked list solves this problem.

```text
null ← 10 ⇄ 20 ⇄ 30 ⇄ 40 → null
```

Each node stores a reference to both its previous and next nodes.

Therefore, we can move:

- Forward using `next`
- Backward using `prev`

---

# 2. Structure of a Doubly Linked List

Each node contains:

```text
+----------+--------+----------+
|   prev   |  data  |   next   |
+----------+--------+----------+
```

Java implementation:

```java
class Node {

    int data;
    Node prev;
    Node next;

    Node(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}
```

Example:

```text
null ← [10] ⇄ [20] ⇄ [30] → null
```

For the node containing `20`:

```text
prev → 10
data → 20
next → 30
```

---

# 3. Head and Tail

A doubly linked list generally maintains two references:

- `head`: first node
- `tail`: last node

Example:

```text
head                       tail
 ↓                           ↓
10 ⇄ 20 ⇄ 30 ⇄ 40
```

The first node has:

```java
head.prev == null
```

The last node has:

```java
tail.next == null
```

These conditions are important when inserting and deleting nodes.

---

# 4. Creating a Doubly Linked List

Let's create three nodes:

```java
Node first = new Node(10);
Node second = new Node(20);
Node third = new Node(30);
```

Connect them:

```java
first.next = second;
second.prev = first;

second.next = third;
third.prev = second;
```

Set the head and tail:

```java
Node head = first;
Node tail = third;
```

Result:

```text
null ← 10 ⇄ 20 ⇄ 30 → null
```

Notice that we must update both directions when connecting nodes.

---

# 5. Traversing Forward

We can traverse from head to tail using `next`.

```java
static void printForward(Node head) {

    Node current = head;

    while (current != null) {

        System.out.print(current.data + " ");

        current = current.next;
    }
}
```

Example:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Output:

```text
10 20 30 40
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 6. Traversing Backward

We can also traverse from tail to head using `prev`.

```java
static void printBackward(Node tail) {

    Node current = tail;

    while (current != null) {

        System.out.print(current.data + " ");

        current = current.prev;
    }
}
```

Example:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Output:

```text
40 30 20 10
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 7. Insertion Operations

## A. Insert at Beginning

Current list:

```text
10 ⇄ 20 ⇄ 30
```

Insert `5`.

Expected result:

```text
5 ⇄ 10 ⇄ 20 ⇄ 30
```

Steps:

1. Create the new node.
2. Point its `next` to the current head.
3. Set the old head's `prev` to the new node.
4. Update head.

Java:

```java
static Node insertAtBeginning(Node head, int data) {

    Node newNode = new Node(data);

    if (head == null) {
        return newNode;
    }

    newNode.next = head;
    head.prev = newNode;

    return newNode;
}
```

Complexity:

```text
Time:  O(1)
Space: O(1)
```

If maintaining a tail reference, remember to set `tail` when inserting into an empty list.

---

## B. Insert at End

Current list:

```text
10 ⇄ 20 ⇄ 30
```

Insert `40`.

Expected result:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

If we have a tail reference:

```java
static Node insertAtEnd(Node head, Node tail, int data) {
    // This example is better handled using a list class
}
```

A complete operation is easier when head and tail are maintained together:

```java
static class DoublyLinkedList {

    Node head;
    Node tail;

    void insertAtEnd(int data) {

        Node newNode = new Node(data);

        if (head == null) {
            head = tail = newNode;
            return;
        }

        tail.next = newNode;
        newNode.prev = tail;

        tail = newNode;
    }
}
```

Complexity:

```text
Time:  O(1)
Space: O(1)
```

---

## C. Insert After a Given Node

Current list:

```text
10 ⇄ 20 ⇄ 40
```

Insert `30` after `20`.

Expected result:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Implementation:

```java
static void insertAfter(Node current, int data) {

    if (current == null) {
        return;
    }

    Node newNode = new Node(data);

    newNode.next = current.next;
    newNode.prev = current;

    if (current.next != null) {
        current.next.prev = newNode;
    }

    current.next = newNode;
}
```

If `current` is the tail, the list class must also update its `tail` reference.

Complexity when the node is already known:

```text
Time:  O(1)
Space: O(1)
```

---

## D. Insert Before a Given Node

Current list:

```text
10 ⇄ 30 ⇄ 40
```

Insert `20` before `30`.

Expected result:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Implementation:

```java
static void insertBefore(Node current, int data) {

    if (current == null) {
        return;
    }

    Node newNode = new Node(data);

    newNode.prev = current.prev;
    newNode.next = current;

    if (current.prev != null) {
        current.prev.next = newNode;
    }

    current.prev = newNode;
}
```

If `current` is the head, the list class must also update `head`.

Complexity when the node is already known:

```text
Time:  O(1)
Space: O(1)
```

---

# 8. Deletion Operations

## A. Delete from Beginning

Current list:

```text
10 ⇄ 20 ⇄ 30
```

Delete `10`.

Expected result:

```text
20 ⇄ 30
```

Implementation:

```java
static Node deleteFromBeginning(Node head) {

    if (head == null) {
        return null;
    }

    Node newHead = head.next;

    if (newHead != null) {
        newHead.prev = null;
    }

    return newHead;
}
```

If the list becomes empty, its tail must also be set to `null`.

Complexity:

```text
Time:  O(1)
Space: O(1)
```

---

## B. Delete from End

Current list:

```text
10 ⇄ 20 ⇄ 30
```

Delete `30`.

Expected result:

```text
10 ⇄ 20
```

Implementation using a tail reference:

```java
void deleteFromEnd() {

    if (tail == null) {
        return;
    }

    if (head == tail) {
        head = tail = null;
        return;
    }

    tail = tail.prev;
    tail.next = null;
}
```

Complexity:

```text
Time:  O(1)
Space: O(1)
```

This is an important advantage over a singly linked list: we can access the previous node directly from the tail.

---

## C. Delete a Given Node

Current list:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Delete `30`.

We need to connect `20` directly to `40`.

```text
Before:

10 ⇄ 20 ⇄ 30 ⇄ 40

After:

10 ⇄ 20 ⇄ 40
```

Implementation:

```java
static Node deleteNode(Node head, Node target) {

    if (head == null || target == null) {
        return head;
    }

    if (target.prev != null) {
        target.prev.next = target.next;
    } else {
        head = target.next;
    }

    if (target.next != null) {
        target.next.prev = target.prev;
    }

    target.prev = null;
    target.next = null;

    return head;
}
```

This method handles deletion of the head, tail, and middle nodes, provided the target belongs to the list.

If the list class maintains `tail`, update it when deleting the last node.

Complexity when the node reference is already known:

```text
Time:  O(1)
Space: O(1)
```

---

# 9. Searching in a Doubly Linked List

Searching works similarly to a singly linked list.

```java
static boolean search(Node head, int target) {

    Node current = head;

    while (current != null) {

        if (current.data == target) {
            return true;
        }

        current = current.next;
    }

    return false;
}
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 10. Reversing a Doubly Linked List

This is a common interview question.

Original:

```text
10 ⇄ 20 ⇄ 30 ⇄ 40
```

Reversed:

```text
40 ⇄ 30 ⇄ 20 ⇄ 10
```

For every node, we swap its `next` and `prev` references.

```java
static Node reverse(Node head) {

    Node current = head;
    Node newHead = null;

    while (current != null) {

        Node next = current.next;

        current.next = current.prev;
        current.prev = next;

        newHead = current;

        current = next;
    }

    return newHead;
}
```

### Dry Run

Initially:

```text
10 ⇄ 20 ⇄ 30
```

For node `10`:

- Save the original next node (`20`).
- Swap `next` and `prev`.
- Move to the saved node.

Repeat for `20` and `30`.

At the end, `newHead` points to `30`.

Result:

```text
30 ⇄ 20 ⇄ 10
```

If using a list class, update both references:

```java
Node oldHead = head;
head = reverse(head);
tail = oldHead;
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 11. Complete Doubly Linked List Implementation

Here is a reusable implementation that maintains `head`, `tail`, and `size`.

```java
class DoublyLinkedList {

    static class Node {

        int data;
        Node prev;
        Node next;

        Node(int data) {
            this.data = data;
        }
    }

    private Node head;
    private Node tail;
    private int size;

    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return head == null;
    }

    public void insertAtBeginning(int data) {

        Node newNode = new Node(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }

        size++;
    }

    public void insertAtEnd(int data) {

        Node newNode = new Node(data);

        if (tail == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }

        size++;
    }

    public void insertAtPosition(int position, int data) {

        if (position < 0 || position > size) {
            throw new IndexOutOfBoundsException("Invalid position");
        }

        if (position == 0) {
            insertAtBeginning(data);
            return;
        }

        if (position == size) {
            insertAtEnd(data);
            return;
        }

        Node current = getNode(position);

        Node newNode = new Node(data);

        newNode.prev = current.prev;
        newNode.next = current;

        current.prev.next = newNode;
        current.prev = newNode;

        size++;
    }

    public void deleteFromBeginning() {

        if (head == null) {
            return;
        }

        if (head == tail) {
            head = tail = null;
        } else {
            head = head.next;
            head.prev = null;
        }

        size--;
    }

    public void deleteFromEnd() {

        if (tail == null) {
            return;
        }

        if (head == tail) {
            head = tail = null;
        } else {
            tail = tail.prev;
            tail.next = null;
        }

        size--;
    }

    public boolean deleteByValue(int value) {

        Node current = head;

        while (current != null) {

            if (current.data == value) {

                if (current == head) {
                    deleteFromBeginning();
                } else if (current == tail) {
                    deleteFromEnd();
                } else {
                    current.prev.next = current.next;
                    current.next.prev = current.prev;
                    size--;
                }

                return true;
            }

            current = current.next;
        }

        return false;
    }

    public boolean search(int value) {

        Node current = head;

        while (current != null) {

            if (current.data == value) {
                return true;
            }

            current = current.next;
        }

        return false;
    }

    private Node getNode(int position) {

        Node current;

        if (position < size / 2) {

            current = head;

            for (int i = 0; i < position; i++) {
                current = current.next;
            }

        } else {

            current = tail;

            for (int i = size - 1; i > position; i--) {
                current = current.prev;
            }
        }

        return current;
    }

    public void printForward() {

        Node current = head;

        while (current != null) {

            System.out.print(current.data + " ⇄ ");

            current = current.next;
        }

        System.out.println("null");
    }

    public void printBackward() {

        Node current = tail;

        while (current != null) {

            System.out.print(current.data + " ⇄ ");

            current = current.prev;
        }

        System.out.println("null");
    }

    public void reverse() {

        Node current = head;

        while (current != null) {

            Node next = current.next;

            current.next = current.prev;
            current.prev = next;

            current = next;
        }

        Node temp = head;
        head = tail;
        tail = temp;
    }
}
```

---

# 12. Using the Doubly Linked List

```java
public class Main {

    public static void main(String[] args) {

        DoublyLinkedList list = new DoublyLinkedList();

        list.insertAtEnd(10);
        list.insertAtEnd(20);
        list.insertAtEnd(30);

        System.out.println("Forward:");
        list.printForward();

        System.out.println("Backward:");
        list.printBackward();

        list.insertAtBeginning(5);

        System.out.println("After insertion:");
        list.printForward();

        list.deleteByValue(20);

        System.out.println("After deletion:");
        list.printForward();

        list.reverse();

        System.out.println("After reversal:");
        list.printForward();

        System.out.println("Size: " + list.size());
    }
}
```

Output:

```text
Forward:
10 ⇄ 20 ⇄ 30 ⇄ null

Backward:
30 ⇄ 20 ⇄ 10 ⇄ null

After insertion:
5 ⇄ 10 ⇄ 20 ⇄ 30 ⇄ null

After deletion:
5 ⇄ 10 ⇄ 30 ⇄ null

After reversal:
30 ⇄ 10 ⇄ 5 ⇄ null

Size: 3
```

---

# 13. Singly Linked List vs Doubly Linked List

| Feature | Singly Linked List | Doubly Linked List |
|---|---|---|
| References per node | 1 | 2 |
| Forward traversal | Yes | Yes |
| Backward traversal | No | Yes |
| Memory usage | Lower | Higher |
| Insert at beginning | O(1) | O(1) |
| Insert at end with tail | O(1) | O(1) |
| Delete last with tail | O(n) | O(1) |
| Delete known node | Requires previous node | O(1) |
| Implementation | Simpler | More complex |

---

# 14. Advantages of Doubly Linked Lists

1. Supports forward and backward traversal.
2. Easier deletion when a node reference is already available.
3. Efficient insertion before or after a known node.
4. Efficient deletion from both ends when head and tail are maintained.
5. Useful for implementing deques and navigation systems.

# 15. Disadvantages of Doubly Linked Lists

1. Requires extra memory for the `prev` reference.
2. More pointer updates are needed.
3. More opportunities for incorrect references.
4. Implementation is more complex than a singly linked list.

---

# 16. Real-World Applications

Doubly linked lists are useful in systems where movement in both directions is required.

Examples:

### Browser Navigation

```text
Previous Page ⇄ Current Page ⇄ Next Page
```

### Music Playlist

```text
Previous Song ⇄ Current Song ⇄ Next Song
```

### Undo and Redo

```text
Previous State ⇄ Current State ⇄ Next State
```

### LRU Cache

A doubly linked list is commonly combined with a HashMap to support efficient removal and movement of cache entries.

You will encounter this pattern in advanced DSA problems.

---

# 17. Common Mistakes

### Mistake 1: Updating only one direction

Wrong:

```java
newNode.next = head;
head = newNode;
```

Correct:

```java
newNode.next = head;
head.prev = newNode;
head = newNode;
```

### Mistake 2: Forgetting to update tail

When deleting the last node, update:

```java
tail = tail.prev;
tail.next = null;
```

### Mistake 3: Not handling an empty list

Always consider:

```java
head == null
```

### Mistake 4: Not handling a single-node list

If:

```java
head == tail
```

deleting that node must make both references `null`.

### Mistake 5: Incorrect reversal

Save the original next node before swapping references:

```java
Node next = current.next;

current.next = current.prev;
current.prev = next;

current = next;
```

---

# 18. Time Complexity Cheat Sheet

Assuming both head and tail references are maintained:

| Operation | Complexity |
|---|---:|
| Insert at beginning | O(1) |
| Insert at end | O(1) |
| Delete from beginning | O(1) |
| Delete from end | O(1) |
| Search | O(n) |
| Access by index | O(n) |
| Insert at position | O(n) |
| Delete known node | O(1) |
| Forward traversal | O(n) |
| Backward traversal | O(n) |
| Reverse list | O(n) |

The position-based operations require traversal to find the required node.

---

# 19. Interview Questions

## Beginner

1. What is a doubly linked list?
2. What is the difference between singly and doubly linked lists?
3. What does `prev` represent?
4. How do you traverse backward?
5. How do you insert at the beginning?
6. How do you insert at the end?
7. How do you delete the last node?

## Intermediate

1. Reverse a doubly linked list.
2. Insert a node before a given node.
3. Delete a given node in O(1).
4. Find a node by value.
5. Implement a deque using a doubly linked list.
6. Find pairs with a given sum in a sorted doubly linked list.

## Advanced

1. Implement an LRU cache.
2. Implement browser history navigation.
3. Design an undo/redo system.
4. Reverse a doubly linked list in groups.
5. Flatten a multilevel doubly linked list.

---

# 20. Practice Problems

## Easy

- Create a doubly linked list.
- Print forward.
- Print backward.
- Insert at beginning.
- Insert at end.
- Delete from beginning.
- Delete from end.
- Search for a value.
- Find the length.

## Medium

- Reverse a doubly linked list.
- Insert before a given node.
- Insert after a given node.
- Delete a given node.
- Find pairs with a given sum.
- Remove duplicates from a sorted doubly linked list.

## Hard

- Implement an LRU cache.
- Flatten a multilevel doubly linked list.
- Reverse nodes in groups.
- Design browser history navigation.

---

# 21. Quick Revision

```text
Singly Linked List:

10 → 20 → 30 → null

Doubly Linked List:

null ← 10 ⇄ 20 ⇄ 30 → null
```

Remember:

```text
next → Forward movement

prev → Backward movement
```

For insertion:

```java
newNode.next = current;
newNode.prev = current.prev;

current.prev.next = newNode;
current.prev = newNode;
```

For deletion of a known middle node:

```java
target.prev.next = target.next;
target.next.prev = target.prev;
```

For reversal:

```java
Node next = current.next;

current.next = current.prev;
current.prev = next;

current = next;
```

---

# Checklist

- [ ] Understand the doubly linked list structure.
- [ ] Understand `prev` and `next`.
- [ ] Create nodes in Java.
- [ ] Maintain head and tail.
- [ ] Traverse forward.
- [ ] Traverse backward.
- [ ] Insert at beginning.
- [ ] Insert at end.
- [ ] Insert before a node.
- [ ] Insert after a node.
- [ ] Delete from beginning.
- [ ] Delete from end.
- [ ] Delete a given node.
- [ ] Reverse a doubly linked list.
- [ ] Handle empty and single-node lists.
- [ ] Understand singly vs doubly linked lists.
- [ ] Understand the complexity of each operation.

---

# Next Topic

```text
05-stack/stack.md
```

Next, we will learn **Stack Data Structure**, including LIFO, push, pop, peek, Java implementation, and common interview problems such as valid parentheses and next greater element.