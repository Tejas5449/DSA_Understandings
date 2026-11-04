# Singly Linked List

A **Singly Linked List** is a linear data structure in which each node contains:

1. Data
2. A reference to the next node

Each node points only to the next node. We can move forward through the list, but cannot directly move backward.

Example:

```text
head
 ↓
10 → 20 → 30 → 40 → null
```

The last node points to `null`, which indicates the end of the list.

---

# 1. Structure of a Singly Linked List

Every node contains two fields:

```text
+--------+--------+
|  data  |  next  |
+--------+--------+
```

Java implementation:

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

Example:

```java
Node first = new Node(10);
Node second = new Node(20);
Node third = new Node(30);

first.next = second;
second.next = third;

Node head = first;
```

Result:

```text
head
 ↓
10 → 20 → 30 → null
```

---

# 2. Complete Singly Linked List Implementation

Let's create a reusable Singly Linked List class.

It will support:

- Insert at beginning
- Insert at end
- Insert at a particular position
- Delete from beginning
- Delete from end
- Delete by value
- Search
- Find length
- Print the list
- Reverse the list
- Find the middle node

## Java Implementation

```java
class SinglyLinkedList {

    static class Node {

        int data;
        Node next;

        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node head;
    private Node tail;
    private int size;

    // Get the size of the list
    public int size() {
        return size;
    }

    // Check whether the list is empty
    public boolean isEmpty() {
        return head == null;
    }

    // Insert at the beginning
    public void insertAtBeginning(int data) {

        Node newNode = new Node(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head = newNode;
        }

        size++;
    }

    // Insert at the end
    public void insertAtEnd(int data) {

        Node newNode = new Node(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            tail = newNode;
        }

        size++;
    }

    // Insert at a zero-based position
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

        Node newNode = new Node(data);
        Node current = head;

        for (int i = 0; i < position - 1; i++) {
            current = current.next;
        }

        newNode.next = current.next;
        current.next = newNode;

        size++;
    }

    // Delete from the beginning
    public void deleteFromBeginning() {

        if (head == null) {
            return;
        }

        head = head.next;
        size--;

        if (head == null) {
            tail = null;
        }
    }

    // Delete from the end
    public void deleteFromEnd() {

        if (head == null) {
            return;
        }

        if (head == tail) {
            head = tail = null;
            size--;
            return;
        }

        Node current = head;

        while (current.next != tail) {
            current = current.next;
        }

        current.next = null;
        tail = current;

        size--;
    }

    // Delete the first occurrence of a value
    public boolean deleteByValue(int value) {

        if (head == null) {
            return false;
        }

        if (head.data == value) {
            deleteFromBeginning();
            return true;
        }

        Node current = head;

        while (current.next != null) {

            if (current.next.data == value) {

                if (current.next == tail) {
                    tail = current;
                }

                current.next = current.next.next;
                size--;

                return true;
            }

            current = current.next;
        }

        return false;
    }

    // Search for a value
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

    // Find the index of a value
    public int indexOf(int value) {

        Node current = head;
        int index = 0;

        while (current != null) {

            if (current.data == value) {
                return index;
            }

            current = current.next;
            index++;
        }

        return -1;
    }

    // Print the list
    public void printList() {

        Node current = head;

        while (current != null) {

            System.out.print(current.data + " → ");

            current = current.next;
        }

        System.out.println("null");
    }

    // Reverse the list
    public void reverse() {

        Node previous = null;
        Node current = head;

        tail = head;

        while (current != null) {

            Node next = current.next;

            current.next = previous;

            previous = current;
            current = next;
        }

        head = previous;
    }

    // Find the middle node
    public Integer findMiddle() {

        if (head == null) {
            return null;
        }

        Node slow = head;
        Node fast = head;

        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;
        }

        return slow.data;
    }
}
```

---

# 3. Using the Singly Linked List

Now let's use our implementation.

```java
public class Main {

    public static void main(String[] args) {

        SinglyLinkedList list = new SinglyLinkedList();

        list.insertAtEnd(10);
        list.insertAtEnd(20);
        list.insertAtEnd(30);

        list.printList();

        list.insertAtBeginning(5);

        list.printList();

        list.insertAtPosition(2, 15);

        list.printList();

        System.out.println("Size: " + list.size());

        System.out.println("Search 20: " + list.search(20));

        list.deleteByValue(15);

        list.printList();

        list.reverse();

        list.printList();

        System.out.println("Middle: " + list.findMiddle());
    }
}
```

Possible output:

```text
10 → 20 → 30 → null
5 → 10 → 20 → 30 → null
5 → 10 → 15 → 20 → 30 → null
Size: 5
Search 20: true
5 → 10 → 20 → 30 → null
30 → 20 → 10 → 5 → null
Middle: 10
```

---

# 4. Insertion Operations

## A. Insert at Beginning

Current list:

```text
10 → 20 → 30 → null
```

Insert `5`.

Steps:

```java
Node newNode = new Node(5);

newNode.next = head;

head = newNode;
```

Result:

```text
5 → 10 → 20 → 30 → null
```

Complexity:

```text
Time:  O(1)
Space: O(1)
```

## B. Insert at End

Current list:

```text
10 → 20 → 30 → null
```

Insert `40`.

Because we maintain a `tail` reference:

```java
tail.next = newNode;
tail = newNode;
```

Result:

```text
10 → 20 → 30 → 40 → null
```

Complexity:

```text
Time:  O(1)
Space: O(1)
```

Without a tail reference, we would need to traverse the list, making insertion O(n).

## C. Insert at a Specific Position

Suppose:

```text
10 → 20 → 40 → null
```

Insert `30` at index `2`.

Remember that positions are zero-based:

| Value | Index |
|---|---:|
| 10 | 0 |
| 20 | 1 |
| 40 | 2 |

We need to insert before the existing node at index `2`.

First, find the previous node:

```text
10 → 20 → 40
     ↑
   current
```

Then update references:

```java
newNode.next = current.next;
current.next = newNode;
```

Result:

```text
10 → 20 → 30 → 40 → null
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 5. Deletion Operations

## A. Delete from Beginning

Before:

```text
10 → 20 → 30 → null
```

Operation:

```java
head = head.next;
```

After:

```text
20 → 30 → null
```

Complexity:

```text
Time: O(1)
```

## B. Delete from End

Before:

```text
10 → 20 → 30 → null
```

We need the node before the tail.

```text
10 → 20 → 30
     ↑
   current
```

Update:

```java
current.next = null;
tail = current;
```

After:

```text
10 → 20 → null
```

Complexity:

```text
Time: O(n)
```

Even though we maintain a tail reference, a singly linked list cannot move backward from the tail to find the previous node.

## C. Delete by Value

Before:

```text
10 → 20 → 30 → 40 → null
```

Delete `30`.

Find the previous node (`20`) and skip the target node:

```java
current.next = current.next.next;
```

After:

```text
10 → 20 → 40 → null
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 6. Searching in a Singly Linked List

Problem: Find whether `30` exists.

```text
10 → 20 → 30 → 40 → null
```

Implementation:

```java
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
```

The algorithm checks each node until it finds the value or reaches `null`.

Complexity:

```text
Best case:  O(1)
Worst case: O(n)
Space:      O(1)
```

---

# 7. Reverse a Singly Linked List

This is one of the most frequently asked linked-list interview problems.

Before:

```text
10 → 20 → 30 → 40 → null
```

After:

```text
40 → 30 → 20 → 10 → null
```

We use three references:

```java
Node previous = null;
Node current = head;
Node next;
```

Implementation:

```java
public void reverse() {

    Node previous = null;
    Node current = head;

    tail = head;

    while (current != null) {

        Node next = current.next;

        current.next = previous;

        previous = current;
        current = next;
    }

    head = previous;
}
```

### Dry Run

Initial:

```text
previous = null
current = 10
```

First iteration:

```text
10 → null

previous = 10
current = 20
```

Second iteration:

```text
20 → 10 → null

previous = 20
current = 30
```

Third iteration:

```text
30 → 20 → 10 → null

previous = 30
current = 40
```

Fourth iteration:

```text
40 → 30 → 20 → 10 → null

previous = 40
current = null
```

Finally:

```java
head = previous;
```

The new head is `40`.

Complexity:

```text
Time:  O(n)
Space: O(1)
```

Important: Save `current.next` before reversing the link. Otherwise, you can lose access to the remaining nodes.

---

# 8. Find the Middle Node

Use the **Fast and Slow Pointer** technique.

Example:

```text
10 → 20 → 30 → 40 → 50 → null
```

Initially:

```java
Node slow = head;
Node fast = head;
```

Movement:

- Slow moves one node.
- Fast moves two nodes.

When fast reaches the end, slow points to the middle.

```java
public Integer findMiddle() {

    if (head == null) {
        return null;
    }

    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {

        slow = slow.next;
        fast = fast.next.next;
    }

    return slow.data;
}
```

For an even number of nodes:

```text
10 → 20 → 30 → 40
```

This implementation returns the second middle node:

```text
30
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 9. Find the Nth Node from the End

Problem:

```text
10 → 20 → 30 → 40 → 50 → null
```

Find the 2nd node from the end.

Answer:

```text
40
```

Use two pointers.

1. Move `fast` forward by `n` nodes.
2. Move `slow` and `fast` together until `fast` reaches `null`.
3. `slow` will point to the required node.

```java
static Integer nthFromEnd(Node head, int n) {

    if (n <= 0) {
        return null;
    }

    Node slow = head;
    Node fast = head;

    for (int i = 0; i < n; i++) {

        if (fast == null) {
            return null;
        }

        fast = fast.next;
    }

    while (fast != null) {

        slow = slow.next;
        fast = fast.next;
    }

    return slow.data;
}
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 10. Detect a Cycle

A normal singly linked list ends with:

```text
null
```

But sometimes a node points to an earlier node.

Example:

```text
1 → 2 → 3 → 4
        ↑     ↓
        └─────┘
```

This creates a cycle.

Use Floyd's Cycle Detection Algorithm:

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

Why does it work?

- Without a cycle, `fast` eventually reaches `null`.
- With a cycle, both pointers move around the cycle.
- Eventually, they meet.

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

# 11. Merge Two Sorted Linked Lists

Problem:

List 1:

```text
1 → 3 → 5 → null
```

List 2:

```text
2 → 4 → 6 → null
```

Expected result:

```text
1 → 2 → 3 → 4 → 5 → 6 → null
```

Use a dummy node to simplify the implementation.

```java
static Node mergeSortedLists(Node list1, Node list2) {

    Node dummy = new Node(0);
    Node current = dummy;

    while (list1 != null && list2 != null) {

        if (list1.data <= list2.data) {

            current.next = list1;
            list1 = list1.next;

        } else {

            current.next = list2;
            list2 = list2.next;
        }

        current = current.next;
    }

    if (list1 != null) {
        current.next = list1;
    }

    if (list2 != null) {
        current.next = list2;
    }

    return dummy.next;
}
```

Complexity:

```text
Time:  O(n + m)
Space: O(1) auxiliary space
```

This implementation reuses the existing nodes rather than creating new data nodes.

---

# 12. Check Whether a Linked List Is a Palindrome

Problem:

```text
1 → 2 → 2 → 1 → null
```

This is a palindrome because the values are the same from both directions.

A common efficient approach:

1. Find the middle node.
2. Reverse the second half.
3. Compare the first and second halves.
4. Optionally restore the original list.

```java
static boolean isPalindrome(Node head) {

    if (head == null || head.next == null) {
        return true;
    }

    Node slow = head;
    Node fast = head;

    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    Node secondHalf = reverseNodes(slow.next);

    Node first = head;
    Node second = secondHalf;

    boolean result = true;

    while (second != null) {

        if (first.data != second.data) {
            result = false;
            break;
        }

        first = first.next;
        second = second.next;
    }

    // Restore the original list
    slow.next = reverseNodes(secondHalf);

    return result;
}

static Node reverseNodes(Node head) {

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

Complexity:

```text
Time:  O(n)
Space: O(1)
```

This version restores the list after comparison.

---

# 13. Important Edge Cases

Always test linked-list operations with:

### Empty list

```text
null
```

### Single node

```text
10 → null
```

### Two nodes

```text
10 → 20 → null
```

### Multiple nodes

```text
10 → 20 → 30 → 40 → null
```

### Duplicate values

```text
10 → 20 → 20 → 30 → null
```

### Value not found

```text
10 → 20 → 30
```

Search for `50`.

### Delete the only node

```text
10 → null
```

After deletion:

```text
head = null
tail = null
size = 0
```

These cases are especially important when maintaining both `head`, `tail`, and `size`.

---

# 14. Singly Linked List Complexity

| Operation | Complexity |
|---|---:|
| Access by index | O(n) |
| Search | O(n) |
| Traverse | O(n) |
| Insert at beginning | O(1) |
| Insert at end with tail | O(1) |
| Insert at end without tail | O(n) |
| Insert at position | O(n) |
| Delete beginning | O(1) |
| Delete end | O(n) |
| Delete by value | O(n) |
| Reverse | O(n) |
| Find middle | O(n) |
| Detect cycle | O(n) |
| Merge two sorted lists | O(n + m) |
| Find nth from end | O(n) |

---

# 15. Common Interview Questions

## Beginner

1. Implement a singly linked list.
2. Insert a node at the beginning.
3. Insert a node at the end.
4. Delete the first node.
5. Delete the last node.
6. Search for a value.
7. Find the length of a linked list.
8. Insert at a given position.

## Intermediate

1. Reverse a linked list.
2. Find the middle node.
3. Find the nth node from the end.
4. Detect a cycle.
5. Merge two sorted linked lists.
6. Remove duplicates from a sorted list.
7. Check whether a linked list is a palindrome.
8. Find the intersection of two linked lists.
9. Remove the nth node from the end.

## Advanced

1. Find the starting node of a cycle.
2. Reverse nodes in groups of K.
3. Sort a linked list using merge sort.
4. Merge K sorted linked lists.
5. Clone a linked list with random pointers.

---

# 16. Practice Problems

## Easy

- Create a singly linked list.
- Print all elements.
- Find the length.
- Search for an element.
- Insert at beginning.
- Insert at end.
- Delete from beginning.
- Delete from end.
- Find the maximum value.
- Find the minimum value.
- Find the middle node.

## Medium

- Reverse a linked list.
- Detect a cycle.
- Find the nth node from the end.
- Remove duplicates.
- Merge two sorted linked lists.
- Check whether a linked list is a palindrome.
- Find the intersection of two linked lists.
- Remove the nth node from the end.

## Hard

- Reverse nodes in K groups.
- Sort a linked list.
- Merge K sorted linked lists.
- Clone a linked list with random pointers.
- Find the starting point of a cycle.

---

# 17. Quick Revision Cheat Sheet

| Requirement | Technique |
|---|---|
| Traverse list | `current = current.next` |
| Insert at beginning | Update `head` |
| Insert at end | Update `tail` |
| Delete first node | `head = head.next` |
| Delete last node | Find previous node |
| Reverse list | Previous, Current, Next |
| Find middle | Slow and Fast |
| Detect cycle | Floyd's Algorithm |
| Find nth from end | Two pointers |
| Merge sorted lists | Dummy node |
| Simplify head operations | Dummy node |

---

# 18. Final Takeaway

A singly linked list is built using nodes connected through references.

The most important concepts are:

- Node structure
- Head and tail references
- Traversal
- Insertion and deletion
- Pointer manipulation
- Fast and slow pointers
- Dummy nodes
- Reversing a linked list

The most important reversal pattern to remember:

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

Practice implementing the operations yourself rather than only reading the code. Pointer manipulation becomes much easier with repetition.

---

# Checklist

- [ ] Understand the Node class.
- [ ] Implement a complete Singly Linked List.
- [ ] Maintain head, tail, and size.
- [ ] Insert at beginning.
- [ ] Insert at end.
- [ ] Insert at a given position.
- [ ] Delete from beginning.
- [ ] Delete from end.
- [ ] Delete by value.
- [ ] Search for an element.
- [ ] Find the length.
- [ ] Reverse the list.
- [ ] Find the middle node.
- [ ] Find nth node from the end.
- [ ] Detect a cycle.
- [ ] Merge two sorted lists.
- [ ] Check whether a list is a palindrome.
- [ ] Handle empty and single-node lists.
- [ ] Understand the time and space complexity of each operation.

---

# Next Topic

```text
04-linked-list/doubly-linked-list.md
```

Next, we will learn **Doubly Linked Lists**, where each node has both `prev` and `next` references, allowing traversal in both directions.