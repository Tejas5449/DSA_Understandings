# Binary Tree Traversals in Java

## 1. Introduction to Tree Traversal

Tree Traversal means visiting every node of a tree in a specific order.

Unlike arrays and linked lists, trees do not have a single sequential order. We need traversal algorithms to access all nodes.

Consider this Binary Tree:

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

The four commonly used traversal techniques are:

1. Inorder Traversal.
2. Preorder Traversal.
3. Postorder Traversal.
4. Level Order Traversal.

The first three are Depth First Search (DFS) traversals. Level Order Traversal is Breadth First Search (BFS).

---

## 2. Tree Node Structure in Java

We will use the following Node class for all examples.

```java
class Node {

    int data;

    Node left;
    Node right;

    Node(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}
```

### Create a sample Binary Tree

```java
Node root = new Node(1);

root.left = new Node(2);
root.right = new Node(3);

root.left.left = new Node(4);
root.left.right = new Node(5);

root.right.left = new Node(6);
root.right.right = new Node(7);
```

Tree structure:

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

---

## 3. Inorder Traversal

### Definition

Inorder Traversal follows this order:

1. Visit the Left Subtree.
2. Visit the Root Node.
3. Visit the Right Subtree.

Remember:

```text
Left → Root → Right
```

### Example

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

Inorder output:

```text
4 2 5 1 6 3 7
```

### Step-by-step explanation

- Start at node `1`.
- Move to its left subtree.
- Visit node `4`.
- Return to node `2`.
- Visit node `2`.
- Visit node `5`.
- Return to node `1`.
- Visit node `1`.
- Traverse the right subtree in the same manner.

### Recursive Java implementation

```java
static void inorder(Node root) {

    if (root == null) {
        return;
    }

    // Visit left subtree
    inorder(root.left);

    // Visit root
    System.out.print(root.data + " ");

    // Visit right subtree
    inorder(root.right);
}
```

### Dry run

For:

```text
        1
       / \
      2   3
     / \
    4   5
```

Execution order:

| Step | Operation | Output |
|---|---|---|
| 1 | Visit left of 1 | |
| 2 | Visit left of 2 | |
| 3 | Visit 4 | 4 |
| 4 | Visit 2 | 4 2 |
| 5 | Visit 5 | 4 2 5 |
| 6 | Visit 1 | 4 2 5 1 |
| 7 | Visit 3 | 4 2 5 1 3 |

### Iterative Inorder Traversal

We can implement inorder traversal without recursion using a Stack.

**Why Stack?**

Recursion internally uses the call stack. When implementing traversal iteratively, we manage that stack ourselves.

```java
import java.util.*;

static void inorderIterative(Node root) {

    Stack<Node> stack = new Stack<>();

    Node current = root;

    while (current != null || !stack.isEmpty()) {

        // Move to the leftmost node
        while (current != null) {

            stack.push(current);

            current = current.left;
        }

        // Process the node
        current = stack.pop();

        System.out.print(current.data + " ");

        // Move to the right subtree
        current = current.right;
    }
}
```

### Complexity

- Time Complexity: O(n).
- Auxiliary Space: O(h), where h is the tree height.

---

## 4. Preorder Traversal

### Definition

Preorder Traversal follows this order:

1. Visit the Root Node.
2. Visit the Left Subtree.
3. Visit the Right Subtree.

Remember:

```text
Root → Left → Right
```

### Example

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

Output:

```text
1 2 4 5 3 6 7
```

### Recursive Java implementation

```java
static void preorder(Node root) {

    if (root == null) {
        return;
    }

    // Visit root
    System.out.print(root.data + " ");

    // Visit left subtree
    preorder(root.left);

    // Visit right subtree
    preorder(root.right);
}
```

### Dry run

For:

```text
        1
       / \
      2   3
     / \
    4   5
```

Execution order:

| Step | Operation | Output |
|---|---|---|
| 1 | Visit 1 | 1 |
| 2 | Visit 2 | 1 2 |
| 3 | Visit 4 | 1 2 4 |
| 4 | Return and visit 5 | 1 2 4 5 |
| 5 | Return and visit 3 | 1 2 4 5 3 |

### Iterative Preorder Traversal

We use a Stack.

```java
import java.util.*;

static void preorderIterative(Node root) {

    if (root == null) {
        return;
    }

    Stack<Node> stack = new Stack<>();

    stack.push(root);

    while (!stack.isEmpty()) {

        Node current = stack.pop();

        System.out.print(current.data + " ");

        // Push right first
        if (current.right != null) {
            stack.push(current.right);
        }

        // Push left second
        if (current.left != null) {
            stack.push(current.left);
        }
    }
}
```

**Why push right before left?**

Because Stack follows LIFO (Last In, First Out).

We want to process the left child first, so we push the right child first.

### Complexity

- Time Complexity: O(n).
- Auxiliary Space: O(h) for recursive traversal; O(n) worst case for the iterative stack.

---

## 5. Postorder Traversal

### Definition

Postorder Traversal follows this order:

1. Visit the Left Subtree.
2. Visit the Right Subtree.
3. Visit the Root Node.

Remember:

```text
Left → Right → Root
```

### Example

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

Output:

```text
4 5 2 6 7 3 1
```

### Recursive Java implementation

```java
static void postorder(Node root) {

    if (root == null) {
        return;
    }

    // Visit left subtree
    postorder(root.left);

    // Visit right subtree
    postorder(root.right);

    // Visit root
    System.out.print(root.data + " ");
}
```

### Dry run

For:

```text
        1
       / \
      2   3
     / \
    4   5
```

Execution order:

| Step | Operation | Output |
|---|---|---|
| 1 | Visit 4 | 4 |
| 2 | Visit 5 | 4 5 |
| 3 | Visit 2 | 4 5 2 |
| 4 | Visit 3 | 4 5 2 3 |
| 5 | Visit 1 | 4 5 2 3 1 |

### Iterative Postorder Traversal Using Two Stacks

```java
import java.util.*;

static void postorderIterative(Node root) {

    if (root == null) {
        return;
    }

    Stack<Node> stack1 = new Stack<>();
    Stack<Node> stack2 = new Stack<>();

    stack1.push(root);

    while (!stack1.isEmpty()) {

        Node current = stack1.pop();

        stack2.push(current);

        if (current.left != null) {
            stack1.push(current.left);
        }

        if (current.right != null) {
            stack1.push(current.right);
        }
    }

    while (!stack2.isEmpty()) {

        System.out.print(stack2.pop().data + " ");
    }
}
```

### Why do we use two stacks?

The first stack processes nodes in a particular order. The second stack reverses that order to produce:

```text
Left → Right → Root
```

### Complexity

- Time Complexity: O(n).
- Auxiliary Space: O(n) for the two-stack implementation.

---

## 6. Level Order Traversal (BFS)

### Definition

Level Order Traversal visits nodes level by level, from left to right.

It uses a Queue.

Remember:

```text
Level 1 → Level 2 → Level 3
```

### Example

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

Output:

```text
1 2 3 4 5 6 7
```

### Why do we use Queue?

A Queue follows FIFO (First In, First Out).

Nodes are processed in the same order in which they are discovered.

### Recursive Java implementation using Queue

```java
import java.util.*;

static void levelOrder(Node root) {

    if (root == null) {
        return;
    }

    Queue<Node> queue = new LinkedList<>();

    queue.offer(root);

    while (!queue.isEmpty()) {

        Node current = queue.poll();

        System.out.print(current.data + " ");

        if (current.left != null) {
            queue.offer(current.left);
        }

        if (current.right != null) {
            queue.offer(current.right);
        }
    }
}
```

### Dry run

Tree:

```text
        1
       / \
      2   3
     / \
    4   5
```

| Step | Removed node | Queue after adding children | Output |
|---|---:|---|---|
| 1 | 1 | [2, 3] | 1 |
| 2 | 2 | [3, 4, 5] | 1 2 |
| 3 | 3 | [4, 5] | 1 2 3 |
| 4 | 4 | [5] | 1 2 3 4 |
| 5 | 5 | [] | 1 2 3 4 5 |

### Level Order Traversal: Each Level Separately

Sometimes interview questions require returning a separate list for each level.

Expected output:

```text
[
  [1],
  [2, 3],
  [4, 5, 6, 7]
]
```

### Java implementation

```java
import java.util.*;

static List<List<Integer>> levelOrderByLevel(Node root) {

    List<List<Integer>> result = new ArrayList<>();

    if (root == null) {
        return result;
    }

    Queue<Node> queue = new LinkedList<>();

    queue.offer(root);

    while (!queue.isEmpty()) {

        int size = queue.size();

        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < size; i++) {

            Node current = queue.poll();

            level.add(current.data);

            if (current.left != null) {
                queue.offer(current.left);
            }

            if (current.right != null) {
                queue.offer(current.right);
            }
        }

        result.add(level);
    }

    return result;
}
```

### Important concept: Why capture queue.size()?

At the beginning of each loop, `queue.size()` tells us how many nodes belong to the current level.

We process exactly those nodes before moving to the next level.

### Complexity

- Time Complexity: O(n).
- Auxiliary Space: O(n) in the worst case for the queue.

---

## 7. Comparison of All Tree Traversals

Consider:

```text
              1
            /   \
           2     3
          / \   / \
         4   5 6   7
```

| Traversal | Order | Output |
|---|---|---|
| Inorder | Left, Root, Right | 4 2 5 1 6 3 7 |
| Preorder | Root, Left, Right | 1 2 4 5 3 6 7 |
| Postorder | Left, Right, Root | 4 5 2 6 7 3 1 |
| Level Order | Level by Level | 1 2 3 4 5 6 7 |

### Easy way to remember

- Inorder: Root in the middle.
- Preorder: Root first.
- Postorder: Root last.
- Level Order: Visit level by level.

---

## 8. DFS vs BFS

Tree traversal algorithms are divided into two major categories.

### Depth First Search (DFS)

DFS explores as far as possible along one branch before backtracking.

DFS includes:

- Inorder.
- Preorder.
- Postorder.

DFS commonly uses recursion or a Stack.

### Breadth First Search (BFS)

BFS explores all nodes at the current level before moving to the next level.

BFS uses a Queue.

### Comparison

| Feature | DFS | BFS |
|---|---|---|
| Approach | Go deep first | Go level by level |
| Data structure | Stack or recursion | Queue |
| Traversals | Inorder, Preorder, Postorder | Level Order |
| Time complexity | O(n) | O(n) |
| Auxiliary space | O(h) recursive | O(w) queue |
| Common use | Tree structure analysis | Level-based processing |

Here:
- `h` = height of the tree.
- `w` = maximum width of the tree.

---

## 9. Common Applications of Tree Traversals

### Inorder Traversal

- Get sorted values from a BST.
- Find kth smallest value in a BST.
- Validate ordering through adjacent values.

### Preorder Traversal

- Copy or serialize a tree.
- Generate prefix expressions.
- Process parent nodes before children.

### Postorder Traversal

- Delete or free a tree.
- Calculate values from child nodes.
- Generate postfix expressions.

### Level Order Traversal

- Print nodes level by level.
- Find the minimum depth.
- Find the maximum width.
- Find the right-side view of a tree.
- Perform BFS-based shortest-level searches.

---

## 10. Complete Java Program for All Four Traversals

```java
import java.util.*;

public class Main {

    static class Node {

        int data;
        Node left;
        Node right;

        Node(int data) {
            this.data = data;
        }
    }

    // Inorder: Left -> Root -> Right
    static void inorder(Node root) {

        if (root == null) {
            return;
        }

        inorder(root.left);

        System.out.print(root.data + " ");

        inorder(root.right);
    }

    // Preorder: Root -> Left -> Right
    static void preorder(Node root) {

        if (root == null) {
            return;
        }

        System.out.print(root.data + " ");

        preorder(root.left);

        preorder(root.right);
    }

    // Postorder: Left -> Right -> Root
    static void postorder(Node root) {

        if (root == null) {
            return;
        }

        postorder(root.left);

        postorder(root.right);

        System.out.print(root.data + " ");
    }

    // Level Order: Level by Level
    static void levelOrder(Node root) {

        if (root == null) {
            return;
        }

        Queue<Node> queue = new LinkedList<>();

        queue.offer(root);

        while (!queue.isEmpty()) {

            Node current = queue.poll();

            System.out.print(current.data + " ");

            if (current.left != null) {
                queue.offer(current.left);
            }

            if (current.right != null) {
                queue.offer(current.right);
            }
        }
    }

    public static void main(String[] args) {

        Node root = new Node(1);

        root.left = new Node(2);
        root.right = new Node(3);

        root.left.left = new Node(4);
        root.left.right = new Node(5);

        root.right.left = new Node(6);
        root.right.right = new Node(7);

        System.out.print("Inorder: ");
        inorder(root);

        System.out.println();

        System.out.print("Preorder: ");
        preorder(root);

        System.out.println();

        System.out.print("Postorder: ");
        postorder(root);

        System.out.println();

        System.out.print("Level Order: ");
        levelOrder(root);
    }
}
```

### Output

```text
Inorder: 4 2 5 1 6 3 7
Preorder: 1 2 4 5 3 6 7
Postorder: 4 5 2 6 7 3 1
Level Order: 1 2 3 4 5 6 7
```

---

## 11. Time and Space Complexity

For a Binary Tree containing `n` nodes and height `h`:

| Traversal | Time Complexity | Auxiliary Space |
|---|---:|---:|
| Recursive Inorder | O(n) | O(h) |
| Iterative Inorder | O(n) | O(h) |
| Recursive Preorder | O(n) | O(h) |
| Iterative Preorder | O(n) | O(n) worst case |
| Recursive Postorder | O(n) | O(h) |
| Iterative Postorder (two stacks) | O(n) | O(n) |
| Level Order | O(n) | O(w) |

For a skewed tree, recursive stack space can become O(n).

For a balanced tree, recursive stack space is O(log n).

---

## 12. Common Mistakes

1. Confusing Inorder, Preorder, and Postorder.
2. Forgetting the base condition `root == null`.
3. Pushing the left child before the right child in iterative preorder.
4. Using a Stack instead of a Queue for normal Level Order Traversal.
5. Forgetting to capture the queue size when processing one level at a time.
6. Processing the root at the wrong position in recursive traversals.
7. Assuming Inorder traversal of every Binary Tree gives sorted values. It gives sorted values only when the tree is a valid BST.
8. Forgetting that every traversal visits all nodes, so its time complexity is O(n).

---

## 13. Common Interview Questions

**Q1. What are the four main Binary Tree traversals?**

Inorder, Preorder, Postorder, and Level Order.

**Q2. Which traversal uses Left → Root → Right?**

Inorder Traversal.

**Q3. Which traversal uses Root → Left → Right?**

Preorder Traversal.

**Q4. Which traversal uses Left → Right → Root?**

Postorder Traversal.

**Q5. Which traversal uses a Queue?**

Level Order Traversal.

**Q6. Which traversals are DFS?**

Inorder, Preorder, and Postorder.

**Q7. Which traversal of a BST produces sorted output?**

Inorder Traversal.

**Q8. What is the time complexity of tree traversal?**

O(n), because every node is visited once.

**Q9. Why do we use Stack in iterative DFS?**

A Stack helps track nodes and supports the depth-first processing order.

**Q10. Why do we use Queue in BFS?**

A Queue ensures nodes are processed in the order they are discovered.

**Q11. What is the difference between DFS and BFS?**

DFS explores deeper nodes first, whereas BFS explores nodes level by level.

**Q12. Can we implement DFS without recursion?**

Yes. We can use an explicit Stack.

**Q13. Can we implement BFS without a Queue?**

It is possible using other techniques, but a Queue is the standard and most natural implementation.

**Q14. What is the space complexity of recursive traversal?**

O(h), where h is the height of the tree.

---

## 14. Practice Problems

### Easy

1. Binary Tree Inorder Traversal.
2. Binary Tree Preorder Traversal.
3. Binary Tree Postorder Traversal.
4. Binary Tree Level Order Traversal.
5. Maximum Depth of Binary Tree.
6. Same Tree.
7. Invert Binary Tree.

### Medium

1. Binary Tree Zigzag Level Order Traversal.
2. Binary Tree Right Side View.
3. Binary Tree Level Order Traversal II.
4. Average of Levels in Binary Tree.
5. Minimum Depth of Binary Tree.
6. Construct Binary Tree from Preorder and Inorder Traversal.
7. Populating Next Right Pointers in Each Node.

### Hard

1. Binary Tree Maximum Path Sum.
2. Serialize and Deserialize Binary Tree.
3. Vertical Order Traversal of a Binary Tree.
4. Binary Tree Cameras.

---

## 15. Quick Revision Cheat Sheet

| Traversal | Sequence | Data Structure |
|---|---|---|
| Inorder | Left → Root → Right | Recursion / Stack |
| Preorder | Root → Left → Right | Recursion / Stack |
| Postorder | Left → Right → Root | Recursion / Stack |
| Level Order | Level by Level | Queue |

### Remember these patterns

```java
// Inorder
inorder(root.left);
process(root);
inorder(root.right);

// Preorder
process(root);
preorder(root.left);
preorder(root.right);

// Postorder
postorder(root.left);
postorder(root.right);
process(root);
```

---

## 16. Chapter Completion Checklist

- [ ] Understand what tree traversal means.
- [ ] Implement recursive Inorder Traversal.
- [ ] Implement iterative Inorder Traversal.
- [ ] Implement recursive Preorder Traversal.
- [ ] Implement iterative Preorder Traversal.
- [ ] Implement recursive Postorder Traversal.
- [ ] Implement iterative Postorder Traversal.
- [ ] Implement Level Order Traversal using Queue.
- [ ] Understand DFS and BFS.
- [ ] Understand traversal time and space complexity.
- [ ] Solve at least five traversal problems.

---

## Next Topic

**09-heap/heap-priority-queue.md**

In the next chapter, we will learn:

- What is a Heap?
- Min Heap and Max Heap.
- Complete Binary Tree property.
- Heap implementation.
- Java PriorityQueue.
- Insert and remove operations.
- Heapify.
- Heap Sort.
- Top K Frequent Elements.
- Kth Largest Element.
- Interview questions and practice problems.