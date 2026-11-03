# Linked List

A **Linked List** is a linear data structure where elements are stored in separate objects called **nodes**.

Unlike an array, linked-list elements are not required to be stored next to each other in memory.

Each node contains:

1. **Data**
2. A reference to the **next node**

Basic structure:

```text
[10 | next] → [20 | next] → [30 | next] → null
```

---

# 1. Why Do We Need Linked Lists?

Arrays are excellent when we need:

- Fast access by index
- Simple data storage
- Cache-friendly traversal

But arrays have limitations.

Suppose we have:

```text
[10, 20, 30, 40, 50]
```

If we want to insert `25` between `20` and `30`, we need to shift elements:

```text
[10, 20, 30, 40, 50]

          ↓

[10, 20, 25, 30, 40, 50]
```

This can take:

```text
O(n)
```

A linked list can insert a node by changing references.

```text
10 → 20 → 30

       ↓

10 → 20 → 25 → 30
```

The actual insertion can be:

```text
O(1)
```

if we already have a reference to the correct node.

---

# 2. Basic Structure

A linked list consists of nodes.

For example:

```text
10 → 20 → 30 → null
```

Each node looks conceptually like:

```text
+--------+--------+
|  data  |  next  |
+--------+--------+
```

For example:

```text
+----+------+
| 10 |  ────────┐
+----+------+   |
               ↓
           +----+------+
           | 20 |  ────────┐
           +----+------+   |
                          ↓
                      +----+------+
                      | 30 | null |
                      +-----------+
```

---

# 3. What Is a Node?

A node is an object that stores:

```text
data
next
```

In Java:

```java
class Node {

    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

Here:

```java
int data;
```

stores the value.

And:

```java
Node next;
```

stores the reference to the next node.

---

# 4. Creating Nodes

We can create nodes like this:

```java
Node first = new Node(10);
Node second = new Node(20);
Node third = new Node(30);
```

At this point they are separate objects.

```text
first  → [10 | null]

second → [20 | null]

third  → [30 | null]
```

Now connect them:

```java
first.next = second;
second.next = third;
```

The structure becomes:

```text
first
  ↓
[10] → [20] → [30] → null
```

---

# 5. What Is Head?

The **head** is the first node of a linked list.

Example:

```text
head
 ↓
10 → 20 → 30 → null
```

In Java:

```java
Node head = new Node(10);
```

Then:

```java
head.next = new Node(20);
head.next.next = new Node(30);
```

The important point is:

> We only need the reference to the first node to access the entire linked list.

---

# 6. What Is Tail?

The **tail** is the last node.

Example:

```text
head                    tail
 ↓                        ↓
10 → 20 → 30 → 40 → null
```

The tail's `next` is always:

```text
null
```

In some implementations, we maintain both:

```java
Node head;
Node tail;
```

This can make insertion at the end faster.

---

# 7. Linked List Terminology

| Term | Meaning |
|---|---|
| Node | Element of linked list |
| Head | First node |
| Tail | Last node |
| Next | Reference to next node |
| Null | End of linked list |
| Traversal | Visiting nodes one by one |

Example:

```text
head
 ↓
10 → 20 → 30 → 40 → null
                ↑
               tail
```

---

# 8. Creating a Simple Linked List

Let's create:

```text
10 → 20 → 30
```

Java:

```java
class Node {

    int data;
    Node next;

    Node(int data) {
        this.data = data;
    }
}

public class Main {

    public static void main(String[] args) {

        Node head = new Node(10);

        head.next = new Node(20);

        head.next.next = new Node(30);
    }
}
```

The structure becomes:

```text
head
 ↓
10 → 20 → 30 → null
```

---

# 9. Traversing a Linked List

Traversal means visiting every node.

Example:

```text
10 → 20 → 30 → null
```

We start from:

```text
head
```

Then repeatedly move to:

```text
current.next
```

---

## Java Implementation

```java
static void printList(Node head) {

    Node current = head;

    while (current != null) {

        System.out.print(current.data + " ");

        current = current.next;
    }
}
```

Output:

```text
10 20 30
```

---

# 10. Dry Run of Traversal

Given:

```text
head
 ↓
10 → 20 → 30 → null
```

Initially:

```text
current = head
```

Therefore:

```text
current = 10
```

Print:

```text
10
```

Move:

```text
current = current.next
```

Now:

```text
current = 20
```

Print:

```text
20
```

Move again:

```text
current = 30
```

Print:

```text
30
```

Move:

```text
current = null
```

Loop stops.

---

# 11. Traversal Pattern

Memorize this basic pattern:

```java
Node current = head;

while (current != null) {

    // process current node

    current = current.next;
}
```

This pattern appears repeatedly in linked-list problems.

---

# 12. Time Complexity of Traversal

If there are `n` nodes:

```text
10 → 20 → 30 → ... → n
```

we visit every node once.

Therefore:

```text
Time: O(n)
Space: O(1)
```

The `current` variable uses constant extra space.

---

# 13. Finding the Length

Problem:

> Find the number of nodes in a linked list.

Example:

```text
10 → 20 → 30 → 40 → null
```

Answer:

```text
4
```

Java:

```java
static int length(Node head) {

    int count = 0;

    Node current = head;

    while (current != null) {

        count++;

        current = current.next;
    }

    return count;
}
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 14. Searching for a Value

Problem:

> Check whether a value exists in the linked list.

Example:

```text
10 → 20 → 30 → 40
```

Search:

```text
30
```

Answer:

```text
true
```

Implementation:

```java
static boolean contains(Node head, int target) {

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

---

# 15. Searching — Dry Run

List:

```text
10 → 20 → 30 → 40
```

Target:

```text
30
```

Check:

```text
10 == 30 → false
20 == 30 → false
30 == 30 → true
```

Return:

```text
true
```

Because we stop immediately when the value is found.

---

# 16. Inserting at the Beginning

Suppose:

```text
10 → 20 → 30
```

We want to insert:

```text
5
```

Result:

```text
5 → 10 → 20 → 30
```

The new node should point to the old head.

```text
newNode
   ↓
 [5] → [10] → [20] → [30]
         ↑
       old head
```

Java:

```java
static Node insertAtBeginning(Node head, int value) {

    Node newNode = new Node(value);

    newNode.next = head;

    return newNode;
}
```

Use it:

```java
head = insertAtBeginning(head, 5);
```

---

# 17. Why Is Beginning Insertion O(1)?

We only perform two operations:

```java
newNode.next = head;
head = newNode;
```

We don't need to traverse the list.

Therefore:

```text
Time: O(1)
Space: O(1)
```

---

# 18. Inserting at the End

Suppose:

```text
10 → 20 → 30
```

Insert:

```text
40
```

Result:

```text
10 → 20 → 30 → 40
```

If we only have `head`, we must find the last node.

```java
static Node insertAtEnd(Node head, int value) {

    Node newNode = new Node(value);

    if (head == null) {
        return newNode;
    }

    Node current = head;

    while (current.next != null) {
        current = current.next;
    }

    current.next = newNode;

    return head;
}
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 19. Why Does Insertion at the End Take O(n)?

Because we need to find:

```text
last node
```

Starting from:

```text
head
```

we move through:

```text
10 → 20 → 30 → 40
```

until:

```java
current.next == null
```

---

# 20. Maintaining a Tail Pointer

We can improve insertion at the end if we maintain:

```java
Node head;
Node tail;
```

Example:

```text
head                  tail
 ↓                      ↓
10 → 20 → 30 → null
```

To insert `40`:

```text
30.next = 40
tail = 40
```

No traversal is required.

Therefore:

```text
Insertion at end: O(1)
```

---

# 21. Inserting After a Given Node

Suppose:

```text
10 → 20 → 40
```

We want to insert:

```text
30
```

after `20`.

Before:

```text
20 → 40
```

Create:

```text
newNode = 30
```

Then:

```text
30.next = 40
20.next = 30
```

Final:

```text
10 → 20 → 30 → 40
```

Java:

```java
static void insertAfter(Node node, int value) {

    if (node == null) {
        return;
    }

    Node newNode = new Node(value);

    newNode.next = node.next;

    node.next = newNode;
}
```

If `node` is already known:

```text
Time: O(1)
```

---

# 22. Important Pointer Order

When inserting a node, be careful about the order.

Suppose:

```text
A → B
```

We want:

```text
A → X → B
```

Correct:

```java
X.next = A.next;
A.next = X;
```

Do **not** do:

```java
A.next = X;
X.next = A.next;
```

because after:

```java
A.next = X;
```

you have lost the reference to `B`.

This is one of the most common linked-list mistakes.

---

# 23. Deleting the First Node

Suppose:

```text
10 → 20 → 30
```

Delete the first node.

Result:

```text
20 → 30
```

We simply move head:

```java
head = head.next;
```

Method:

```java
static Node deleteFirst(Node head) {

    if (head == null) {
        return null;
    }

    return head.next;
}
```

Complexity:

```text
Time: O(1)
Space: O(1)
```

---

# 24. Deleting the Last Node

Suppose:

```text
10 → 20 → 30
```

Delete `30`.

Result:

```text
10 → 20
```

We need to find the node before the last node.

```text
10 → 20 → 30
     ↑
   previous
```

Then:

```java
previous.next = null;
```

Implementation:

```java
static Node deleteLast(Node head) {

    if (head == null || head.next == null) {
        return null;
    }

    Node current = head;

    while (current.next.next != null) {
        current = current.next;
    }

    current.next = null;

    return head;
}
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 25. Deleting a Node by Value

Problem:

```text
10 → 20 → 30 → 40
```

Delete:

```text
30
```

Result:

```text
10 → 20 → 40
```

We need the previous node:

```text
20 → 30 → 40
↑
previous
```

Then:

```java
previous.next = current.next;
```

Implementation:

```java
static Node deleteValue(Node head, int value) {

    if (head == null) {
        return null;
    }

    if (head.data == value) {
        return head.next;
    }

    Node current = head;

    while (current.next != null) {

        if (current.next.data == value) {

            current.next = current.next.next;

            return head;
        }

        current = current.next;
    }

    return head;
}
```

---

# 26. Visualizing Deletion

Before:

```text
10 → 20 → 30 → 40
     ↑    ↑
 previous current
```

We want to skip `30`.

Set:

```text
20.next = 40
```

Result:

```text
10 → 20 ─────→ 40
         

30 is no longer part of the list.
```

---

# 27. Updating a Node

To update a node's value:

```java
static boolean updateValue(Node head, int oldValue, int newValue) {

    Node current = head;

    while (current != null) {

        if (current.data == oldValue) {

            current.data = newValue;

            return true;
        }

        current = current.next;
    }

    return false;
}
```

Example:

```text
10 → 20 → 30
```

Update:

```text
20 → 25
```

Result:

```text
10 → 25 → 30
```

---

# 28. Complete Basic Linked List Class

A simple implementation can look like this:

```java
class LinkedList {

    Node head;

    static class Node {

        int data;
        Node next;

        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    void insertAtBeginning(int data) {

        Node newNode = new Node(data);

        newNode.next = head;

        head = newNode;
    }

    void insertAtEnd(int data) {

        Node newNode = new Node(data);

        if (head == null) {
            head = newNode;
            return;
        }

        Node current = head;

        while (current.next != null) {
            current = current.next;
        }

        current.next = newNode;
    }

    void print() {

        Node current = head;

        while (current != null) {

            System.out.print(current.data + " ");

            current = current.next;
        }

        System.out.println();
    }
}
```

---

# 29. Using Our Linked List

```java
public class Main {

    public static void main(String[] args) {

        LinkedList list = new LinkedList();

        list.insertAtBeginning(20);
        list.insertAtBeginning(10);

        list.insertAtEnd(30);
        list.insertAtEnd(40);

        list.print();
    }
}
```

Output:

```text
10 20 30 40
```

---

# 30. Array vs Linked List

This is a very important interview comparison.

| Feature | Array | Linked List |
|---|---|---|
| Memory layout | Contiguous | Non-contiguous |
| Random access | O(1) | O(n) |
| Search | O(n) | O(n) |
| Insert at beginning | O(n) | O(1) |
| Delete at beginning | O(n) | O(1) |
| Insert at end | O(1) amortized for dynamic array | O(n) without tail |
| Insert at known node | O(n) shifting | O(1) |
| Extra memory | Low | Higher due to references |
| Cache performance | Usually better | Usually worse |

---

# 31. Why Linked List Random Access Is O(n)

Suppose:

```text
10 → 20 → 30 → 40 → 50
```

If we want:

```text
index = 4
```

we cannot directly jump to `50`.

We must traverse:

```text
10
 ↓
20
 ↓
30
 ↓
40
 ↓
50
```

Therefore:

```text
O(n)
```

An array can directly access:

```java
array[4]
```

which is:

```text
O(1)
```

---

# 32. Why Linked Lists Need Extra Memory

An array stores:

```text
10 20 30 40
```

A linked list stores:

```text
[10 | next]
[20 | next]
[30 | next]
[40 | null]
```

Each node needs an additional reference.

Therefore linked lists generally use more memory per stored element.

---

# 33. Linked List Variations

There are several types of linked lists.

## Singly Linked List

Each node points only to the next node.

```text
10 → 20 → 30 → null
```

---

## Doubly Linked List

Each node contains:

```text
previous
data
next
```

Structure:

```text
null ← 10 ⇄ 20 ⇄ 30 → null
```

---

## Circular Linked List

The last node points back to the first.

```text
10 → 20 → 30
↑         ↓
└─────────┘
```

These will be covered in more detail in:

```text
04-linked-list/singly-linked-list.md
04-linked-list/doubly-linked-list.md
```

---

# 34. Important Linked List Patterns

Several linked-list interview problems use specific patterns.

You should eventually become comfortable with:

```text
1. Traversal
2. Two Pointers
3. Fast and Slow Pointers
4. Dummy Node
5. Reversing Links
6. Finding Middle
7. Detecting Cycle
8. Merging Lists
```

The next linked-list chapters will build these concepts.

---

# 35. Fast and Slow Pointers

One of the most important linked-list techniques is using two pointers.

We usually call them:

```text
slow
fast
```

The slow pointer moves one step:

```text
slow = slow.next
```

The fast pointer moves two steps:

```text
fast = fast.next.next
```

Example:

```text
1 → 2 → 3 → 4 → 5
```

Movement:

```text
slow: 1 → 2 → 3 → 4 → 5

fast: 1 → 3 → 5
```

This technique can be used to find the middle of a linked list.

---

# 36. Finding the Middle Node

Example:

```text
1 → 2 → 3 → 4 → 5
```

Middle:

```text
3
```

Implementation:

```java
static Node findMiddle(Node head) {

    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {

        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

At the end:

```text
slow → middle
```

---

# 37. Why Fast and Slow Works

Suppose the list has:

```text
5 nodes
```

Fast moves twice as quickly as slow.

When:

```text
fast
```

reaches the end, `slow` has traveled approximately half the distance.

Therefore:

```text
slow = middle
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 38. Detecting a Cycle

Consider:

```text
1 → 2 → 3 → 4
        ↑     ↓
        └─────┘
```

There is a cycle.

A linked list normally ends at:

```text
null
```

A cyclic list does not.

We can use fast and slow pointers.

```java
static boolean hasCycle(Node head) {

    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {

        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            return true;
        }
    }

    return false;
}
```

This is called **Floyd's Cycle Detection Algorithm**.

---

# 39. Why Cycle Detection Works

Without a cycle:

```text
slow → ...
fast → ... → null
```

Fast eventually reaches `null`.

With a cycle:

```text
slow → →
       ↑ ↓
fast → →
```

Both pointers continue moving around the cycle.

Eventually:

```text
slow == fast
```

Therefore a cycle exists.

---

# 40. Dummy Node

A **dummy node** is an extra node placed before the actual head.

Instead of:

```text
head → 10 → 20 → 30
```

we create:

```text
dummy → 10 → 20 → 30
          ↑
         head
```

Dummy nodes simplify many insertion and deletion problems.

Example:

```java
Node dummy = new Node(0);
dummy.next = head;
```

Then:

```text
dummy.next
```

represents the actual list.

---

# 41. Why Use a Dummy Node?

Consider deleting the first node.

Without a dummy node, we need a special case:

```java
if (head.data == value) {
    head = head.next;
}
```

With a dummy node, the head behaves more like a normal node.

This can simplify:

- Delete nodes
- Insert nodes
- Merge lists
- Remove duplicates
- Partition lists

---

# 42. Reversing a Linked List

One of the most important linked-list interview problems is:

> Reverse a linked list.

Given:

```text
1 → 2 → 3 → 4 → null
```

Result:

```text
4 → 3 → 2 → 1 → null
```

The solution uses three references:

```text
previous
current
next
```

---

## Basic Idea

Initially:

```text
previous = null
current = head
```

Then:

```text
next = current.next
current.next = previous
previous = current
current = next
```

Repeat.

Implementation:

```java
static Node reverse(Node head) {

    Node previous = null;
    Node current = head;

    while (current != null) {

        Node next = current.next;

        current.next = previous;

        previous = current;

        current = next;
    }

    return previous;
}
```

---

# 43. Reverse Dry Run

Initial:

```text
1 → 2 → 3 → null
```

Variables:

```text
previous = null
current = 1
```

Save next:

```text
next = 2
```

Reverse:

```text
1 → null
```

Move:

```text
previous = 1
current = 2
```

Next:

```text
2 → 3
```

Reverse:

```text
2 → 1 → null
```

Move:

```text
previous = 2
current = 3
```

Reverse:

```text
3 → 2 → 1 → null
```

Finally:

```text
current = null
```

Return:

```text
previous
```

Therefore:

```text
3 → 2 → 1 → null
```

---

# 44. The Most Important Pointer Rule

When manipulating linked lists:

> **Save the next node before changing the current node's next reference.**

Correct:

```java
Node next = current.next;
current.next = previous;
```

If you change:

```java
current.next
```

first without saving the old reference, you may lose the rest of the list.

This is one of the most important concepts in linked-list problems.

---

# 45. Common Linked List Mistakes

## Mistake 1 — Losing the next node

Wrong:

```java
current.next = previous;
current = current.next;
```

You just changed `current.next`, so you may lose the original next node.

Correct:

```java
Node next = current.next;

current.next = previous;

current = next;
```

---

## Mistake 2 — Forgetting the empty list

Always consider:

```text
head == null
```

---

## Mistake 3 — Forgetting a single-node list

Example:

```text
10 → null
```

Many algorithms need special handling or careful conditions.

---

## Mistake 4 — Forgetting to update head

If you insert at the beginning:

```java
head = newNode;
```

If you reverse the list:

```java
head = reverse(head);
```

---

## Mistake 5 — Incorrect loop condition

For traversal:

```java
while (current != null)
```

For fast/slow:

```java
while (fast != null && fast.next != null)
```

Use the condition appropriate to the operation.

---

# 46. Linked List Complexity Cheat Sheet

Assuming a singly linked list:

| Operation | Complexity |
|---|---:|
| Access by index | O(n) |
| Search | O(n) |
| Traverse | O(n) |
| Insert at beginning | O(1) |
| Delete at beginning | O(1) |
| Insert at end without tail | O(n) |
| Insert at end with tail | O(1) |
| Delete at end | O(n) |
| Insert after known node | O(1) |
| Delete after known previous node | O(1) |
| Reverse | O(n) |
| Find middle | O(n) |
| Detect cycle | O(n) |

---

# 47. Interview Questions

### Beginner

1. What is a linked list?
2. What is a node?
3. What is the head?
4. What is the tail?
5. How do you traverse a linked list?
6. How do you find its length?
7. How do you search for an element?
8. How do you insert at the beginning?
9. How do you insert at the end?
10. How do you delete the first node?

### Intermediate

11. Reverse a linked list.
12. Find the middle node.
13. Detect a cycle.
14. Find the nth node from the end.
15. Merge two sorted linked lists.
16. Remove duplicates.
17. Delete a node by value.
18. Find the intersection of two linked lists.
19. Check whether a linked list is a palindrome.
20. Remove the nth node from the end.

### Advanced

21. Detect the starting point of a cycle.
22. Reverse nodes in groups of K.
23. Merge K sorted linked lists.
24. Sort a linked list.
25. Flatten a multilevel linked list.
26. Clone a linked list with random pointers.

---

# 48. Practice Problems

## Easy

- Create a linked list
- Print all nodes
- Find length
- Search an element
- Insert at beginning
- Insert at end
- Delete first node
- Delete last node
- Delete a value
- Find maximum value
- Find minimum value

## Medium

- Reverse linked list
- Find middle node
- Detect cycle
- Find nth node from end
- Remove duplicates
- Merge two sorted lists
- Check palindrome
- Find intersection
- Remove nth node from end

## Hard

- Reverse nodes in K groups
- Merge K sorted lists
- Sort linked list
- Detect cycle starting node
- Clone linked list with random pointer
- Flatten multilevel linked list

---

# 49. Quick Recognition Guide

When you see:

```text
"linked list"
```

think:

```text
Node
 ↓
data + next
```

When you see:

```text
"middle"
```

think:

```text
Slow + Fast Pointer
```

When you see:

```text
"cycle"
```

think:

```text
Floyd's Algorithm
Slow + Fast
```

When you see:

```text
"reverse"
```

think:

```text
Previous
Current
Next
```

When you see:

```text
"insert/delete around head"
```

think:

```text
Dummy Node
```

When you see:

```text
"nth from end"
```

think:

```text
Two Pointers
```

---

# 50. Final Takeaway

A linked list is fundamentally about **nodes and references**.

The basic structure is:

```text
head
 ↓
[data | next] → [data | next] → [data | next] → null
```

The most important concepts are:

```text
1. Node
2. Head
3. Tail
4. Traversal
5. Insertion
6. Deletion
7. Two Pointers
8. Fast and Slow Pointers
9. Dummy Node
10. Reversal
```

The most important code pattern is traversal:

```java
Node current = head;

while (current != null) {

    // process node

    current = current.next;
}
```

And the most important reversal pattern is:

```java
Node previous = null;
Node current = head;

while (current != null) {

    Node next = current.next;

    current.next = previous;

    previous = current;

    current = next;
}

head = previous;
```

Once you understand these patterns, most basic linked-list problems become much easier.

---

# Checklist

Before moving forward, make sure you understand:

- [ ] What a linked list is
- [ ] What a node contains
- [ ] What head means
- [ ] What tail means
- [ ] How `next` references work
- [ ] How to create nodes in Java
- [ ] How to traverse a linked list
- [ ] How to find length
- [ ] How to search
- [ ] Insert at beginning
- [ ] Insert at end
- [ ] Insert after a node
- [ ] Delete first node
- [ ] Delete last node
- [ ] Delete by value
- [ ] Array vs linked list
- [ ] Singly vs doubly linked list
- [ ] Fast and slow pointers
- [ ] Find middle node
- [ ] Detect cycle
- [ ] Dummy node
- [ ] Reverse a linked list
- [ ] Common pointer mistakes
- [ ] Linked-list time complexities

---

# Next Topic

The next file is:

```text
04-linked-list/singly-linked-list.md
```

There we will implement a **complete Singly Linked List in Java**, including insertion, deletion, searching, reversal, and interview-style problems.