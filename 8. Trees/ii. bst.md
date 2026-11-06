# Binary Search Tree (BST) in Java

## 1. Introduction to Binary Search Tree

A Binary Search Tree (BST) is a special type of Binary Tree that follows a specific ordering rule.

For every node:

- All values in the left subtree are smaller than the node's value.
- All values in the right subtree are greater than the node's value.
- Both left and right subtrees must also follow the BST property.

### Example

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

In this example:

- Values smaller than `50` are on the left.
- Values greater than `50` are on the right.
- The same rule applies to every node.

### Why do we use BST?

A normal Binary Tree does not follow any ordering rule.

A BST maintains sorted relationships between nodes, making searching, insertion, and deletion efficient when the tree is reasonably balanced.

---

## 2. BST Properties

Consider:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

Important properties:

1. Left subtree values are smaller than the current node.
2. Right subtree values are greater than the current node.
3. The left subtree must also be a valid BST.
4. The right subtree must also be a valid BST.
5. Inorder traversal of a BST produces values in sorted order.

### Important note about duplicates

In this chapter, we use the following policy:

- Duplicate values are not inserted.
- Every value is unique.

Some implementations allow duplicates by consistently placing them on one side. Always follow the duplicate policy specified by the problem.

---

## 3. BST Node Implementation in Java

A BST node contains:

- Data.
- Reference to the left child.
- Reference to the right child.

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

### Creating a BST manually

```java
public class Main {

    public static void main(String[] args) {

        Node root = new Node(50);

        root.left = new Node(30);
        root.right = new Node(70);

        root.left.left = new Node(20);
        root.left.right = new Node(40);

        root.right.left = new Node(60);
        root.right.right = new Node(80);
    }
}
```

---

## 4. Search an Element in BST

### Problem

Given a BST and a target value, determine whether the value exists.

Input:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80

Target = 40
```

Output:

```text
true
```

### Approach

Compare the target with the current node:

1. If the current node is null, the value does not exist.
2. If the target equals the current node, return true.
3. If the target is smaller, search the left subtree.
4. If the target is greater, search the right subtree.

### Java solution using recursion

```java
static boolean search(Node root, int target) {

    if (root == null) {
        return false;
    }

    if (root.data == target) {
        return true;
    }

    if (target < root.data) {
        return search(root.left, target);
    }

    return search(root.right, target);
}
```

### Dry run

Target = `40`

```text
              50
             /
            30
              \
               40
```

| Step | Current node | Comparison | Action |
|---|---:|---|---|
| 1 | 50 | 40 < 50 | Move left |
| 2 | 30 | 40 > 30 | Move right |
| 3 | 40 | 40 == 40 | Found |

Output:

```text
true
```

**Time Complexity:**
- Average: O(log n) for a reasonably balanced BST.
- Worst: O(n) for a skewed BST.

**Space Complexity:** O(h) due to recursion.

### Iterative approach

```java
static boolean searchIterative(Node root, int target) {

    while (root != null) {

        if (root.data == target) {
            return true;
        }

        if (target < root.data) {
            root = root.left;
        } else {
            root = root.right;
        }
    }

    return false;
}
```

The iterative approach uses O(1) auxiliary space.

---

## 5. Insert a Node into BST

### Problem

Insert a new value into an existing BST while maintaining the BST property.

Input:

```text
              50
            /    \
           30     70
```

Insert:

```text
40
```

Output:

```text
              50
            /    \
           30     70
             \
              40
```

### Approach

1. If the current node is null, create a new node.
2. If the new value is smaller, insert into the left subtree.
3. If the new value is greater, insert into the right subtree.
4. If the value already exists, do nothing according to our duplicate policy.

### Java solution

```java
static Node insert(Node root, int value) {

    if (root == null) {
        return new Node(value);
    }

    if (value < root.data) {

        root.left = insert(root.left, value);

    } else if (value > root.data) {

        root.right = insert(root.right, value);
    }

    return root;
}
```

### Dry run

Insert values:

```text
50, 30, 70, 20, 40
```

Insertion steps:

- Insert `50`: It becomes the root.
- Insert `30`: Smaller than `50`, so it goes left.
- Insert `70`: Greater than `50`, so it goes right.
- Insert `20`: Smaller than `50` and `30`, so it goes left of `30`.
- Insert `40`: Smaller than `50`, but greater than `30`, so it goes right of `30`.

Final tree:

```text
              50
            /    \
           30     70
          /  \
         20   40
```

**Time Complexity:**
- Average: O(log n).
- Worst: O(n).

**Space Complexity:** O(h) due to recursion.

---

## 6. Find Minimum Value in BST

### Concept

The minimum value in a BST is always present in the leftmost node.

Example:

```text
              50
            /    \
           30     70
          /
         20
        /
       10
```

Minimum value = `10`.

### Java solution

```java
static int findMinimum(Node root) {

    if (root == null) {
        throw new IllegalArgumentException("Tree is empty");
    }

    while (root.left != null) {
        root = root.left;
    }

    return root.data;
}
```

**Time Complexity:**
- Average: O(log n) for a balanced BST.
- Worst: O(n).

**Space Complexity:** O(1).

---

## 7. Find Maximum Value in BST

### Concept

The maximum value in a BST is always present in the rightmost node.

Example:

```text
              50
            /    \
           30     70
                     \
                      80
                        \
                         90
```

Maximum value = `90`.

### Java solution

```java
static int findMaximum(Node root) {

    if (root == null) {
        throw new IllegalArgumentException("Tree is empty");
    }

    while (root.right != null) {
        root = root.right;
    }

    return root.data;
}
```

**Time Complexity:**
- Average: O(log n) for a balanced BST.
- Worst: O(n).

**Space Complexity:** O(1).

---

## 8. Delete a Node from BST

Deletion is one of the most important BST interview questions.

There are three cases to consider.

### Case 1: Node has no children

This is a leaf node.

Example: Delete `20`.

Before:

```text
              50
            /    \
           30     70
          /
         20
```

After:

```text
              50
            /    \
           30     70
```

Simply remove the node by returning null from its recursive call.

### Case 2: Node has one child

Example: Delete `30`.

Before:

```text
              50
            /    \
           30     70
             \
              40
```

After:

```text
              50
            /    \
           40     70
```

Replace the deleted node with its only child.

### Case 3: Node has two children

Example: Delete `50`.

Before:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

We can replace the deleted node with its inorder successor.

The inorder successor is the smallest value in the right subtree.

Here, the inorder successor is `60`.

After:

```text
              60
            /    \
           30     70
          / \      \
         20  40     80
```

### Java solution

```java
static Node deleteNode(Node root, int key) {

    if (root == null) {
        return null;
    }

    if (key < root.data) {

        root.left = deleteNode(root.left, key);

    } else if (key > root.data) {

        root.right = deleteNode(root.right, key);

    } else {

        // Case 1: No child
        if (root.left == null && root.right == null) {
            return null;
        }

        // Case 2: Only right child
        if (root.left == null) {
            return root.right;
        }

        // Case 2: Only left child
        if (root.right == null) {
            return root.left;
        }

        // Case 3: Two children
        int successor = findMinimum(root.right);

        root.data = successor;

        root.right = deleteNode(root.right, successor);
    }

    return root;
}
```

### Complexity

**Time Complexity:**
- Average: O(log n) for a balanced BST.
- Worst: O(n).

**Space Complexity:** O(h) due to recursion.

---

## 9. Validate Whether a Binary Tree Is a BST

### Problem

Given a Binary Tree, determine whether it follows the BST property.

Valid BST:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

Invalid BST:

```text
              50
            /    \
           30     70
          / \
         20  60
```

Why is the second tree invalid?

Because `60` is in the left subtree of `50`, but it is greater than `50`.

Checking only whether each node is greater than its immediate left child and smaller than its immediate right child is not sufficient.

### Correct approach: Range validation

Each node must fall within a valid minimum and maximum range.

```java
static boolean isValidBST(Node root) {

    return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

static boolean validate(Node root, long min, long max) {

    if (root == null) {
        return true;
    }

    if (root.data <= min || root.data >= max) {
        return false;
    }

    return validate(root.left, min, root.data)
            && validate(root.right, root.data, max);
}
```

### Explanation

For the root `50`:

- Left subtree must contain values between negative infinity and `50`.
- Right subtree must contain values between `50` and positive infinity.

These restrictions are passed down to every descendant.

**Time Complexity:** O(n).

**Space Complexity:** O(h).

---

## 10. Lowest Common Ancestor (LCA) in BST

### What is LCA?

The Lowest Common Ancestor of two nodes is the lowest node in the tree that has both nodes as descendants.

A node can be an ancestor of itself.

Example:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

Find LCA of `20` and `40`.

Output:

```text
30
```

Find LCA of `20` and `80`.

Output:

```text
50
```

### Approach

For a BST:

1. If both values are smaller than the current node, move left.
2. If both values are greater than the current node, move right.
3. Otherwise, the current node is the split point and is the LCA.

### Java solution

```java
static Node lowestCommonAncestor(
        Node root, int p, int q) {

    if (root == null) {
        return null;
    }

    if (p < root.data && q < root.data) {
        return lowestCommonAncestor(root.left, p, q);
    }

    if (p > root.data && q > root.data) {
        return lowestCommonAncestor(root.right, p, q);
    }

    return root;
}
```

**Important:** This implementation assumes both target values exist in the BST. If the problem does not guarantee that, verify their presence before returning an LCA.

**Time Complexity:**
- Average: O(log n) for a balanced BST.
- Worst: O(n).

**Space Complexity:** O(h).

---

## 11. Inorder Traversal of BST

Inorder traversal follows this order:

1. Left subtree.
2. Current node.
3. Right subtree.

### Java solution

```java
static void inorder(Node root) {

    if (root == null) {
        return;
    }

    inorder(root.left);

    System.out.print(root.data + " ");

    inorder(root.right);
}
```

For this BST:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

Output:

```text
20 30 40 50 60 70 80
```

**Important:** Inorder traversal of a BST produces sorted values in ascending order when the tree follows the strict ordering policy used in this chapter.

---

## 12. Find the Kth Smallest Element in BST

### Problem

Find the kth smallest value in a BST.

Example:

```text
              50
            /    \
           30     70
          / \    / \
         20  40  60  80
```

For `k = 3`, output:

```text
40
```

Because inorder traversal gives:

```text
20 30 40 50 60 70 80
```

The third smallest element is `40`.

### Java solution

```java
static int kthSmallest(Node root, int k) {

    Stack<Node> stack = new Stack<>();

    Node current = root;

    while (current != null || !stack.isEmpty()) {

        while (current != null) {
            stack.push(current);
            current = current.left;
        }

        current = stack.pop();

        k--;

        if (k == 0) {
            return current.data;
        }

        current = current.right;
    }

    return -1;
}
```

**Time Complexity:** O(h + k) in the usual analysis, bounded by O(n).

**Space Complexity:** O(h).

The method returns `-1` if k is invalid or larger than the number of nodes. In production code, a different error-handling approach may be preferable if `-1` is a valid tree value.

---

## 13. Complete BST Java Implementation

This program combines insertion, searching, deletion, minimum, maximum, and inorder traversal.

```java
public class Main {

    static class Node {

        int data;
        Node left;
        Node right;

        Node(int data) {
            this.data = data;
        }
    }

    static Node insert(Node root, int value) {

        if (root == null) {
            return new Node(value);
        }

        if (value < root.data) {
            root.left = insert(root.left, value);
        } else if (value > root.data) {
            root.right = insert(root.right, value);
        }

        return root;
    }

    static boolean search(Node root, int target) {

        if (root == null) {
            return false;
        }

        if (root.data == target) {
            return true;
        }

        if (target < root.data) {
            return search(root.left, target);
        }

        return search(root.right, target);
    }

    static int findMinimum(Node root) {

        if (root == null) {
            throw new IllegalArgumentException("Tree is empty");
        }

        while (root.left != null) {
            root = root.left;
        }

        return root.data;
    }

    static int findMaximum(Node root) {

        if (root == null) {
            throw new IllegalArgumentException("Tree is empty");
        }

        while (root.right != null) {
            root = root.right;
        }

        return root.data;
    }

    static Node deleteNode(Node root, int key) {

        if (root == null) {
            return null;
        }

        if (key < root.data) {
            root.left = deleteNode(root.left, key);
        } else if (key > root.data) {
            root.right = deleteNode(root.right, key);
        } else {

            if (root.left == null) {
                return root.right;
            }

            if (root.right == null) {
                return root.left;
            }

            root.data = findMinimum(root.right);

            root.right = deleteNode(root.right, root.data);
        }

        return root;
    }

    static void inorder(Node root) {

        if (root == null) {
            return;
        }

        inorder(root.left);

        System.out.print(root.data + " ");

        inorder(root.right);
    }

    public static void main(String[] args) {

        Node root = null;

        int[] values = {50, 30, 70, 20, 40, 60, 80};

        for (int value : values) {
            root = insert(root, value);
        }

        System.out.print("Inorder: ");
        inorder(root);

        System.out.println();

        System.out.println(
            "Search 40: " + search(root, 40)
        );

        System.out.println(
            "Minimum: " + findMinimum(root)
        );

        System.out.println(
            "Maximum: " + findMaximum(root)
        );

        root = deleteNode(root, 50);

        System.out.print("After deletion: ");
        inorder(root);
    }
}
```

Output:

```text
Inorder: 20 30 40 50 60 70 80
Search 40: true
Minimum: 20
Maximum: 80
After deletion: 20 30 40 60 70 80
```

---

## 14. BST Time and Space Complexity

Let:
- `n` = total number of nodes.
- `h` = height of the BST.

| Operation | Balanced BST | Worst Case |
|---|---:|---:|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Find minimum | O(log n) | O(n) |
| Find maximum | O(log n) | O(n) |
| Inorder traversal | O(n) | O(n) |
| Validate BST | O(n) | O(n) |

Recursive operations use O(h) auxiliary stack space.

A BST is not automatically balanced. If values are inserted in sorted order, it can become skewed:

```text
10
  \
   20
     \
      30
        \
         40
```

In this case, searching may take O(n).

Self-balancing trees such as AVL Trees and Red-Black Trees help maintain logarithmic height.

---

## 15. Common Mistakes

1. Assuming every Binary Tree is a BST.
2. Checking only immediate parent-child relationships while validating a BST.
3. Forgetting to assign the returned root after insertion or deletion.
4. Forgetting the three deletion cases.
5. Using an incorrect inorder traversal order.
6. Assuming BST operations always take O(log n).
7. Ignoring the duplicate-value policy.
8. Forgetting to handle an empty tree.
9. Assuming the LCA implementation verifies that both nodes exist.

---

## 16. Common Interview Questions

**Q1. What is a Binary Search Tree?**

A Binary Search Tree is a Binary Tree in which values in the left subtree are smaller and values in the right subtree are greater than the current node, under the strict ordering convention used here.

**Q2. What is the time complexity of searching in BST?**

O(log n) on average for a reasonably balanced BST and O(n) in the worst case.

**Q3. Why is inorder traversal important in BST?**

It returns values in sorted ascending order.

**Q4. How do you find the minimum value in BST?**

Move repeatedly to the left child until no left child exists.

**Q5. How do you find the maximum value in BST?**

Move repeatedly to the right child until no right child exists.

**Q6. What are the three cases of BST deletion?**

- Node has no children.
- Node has one child.
- Node has two children.

**Q7. What is the inorder successor?**

The smallest value in the right subtree of a node.

**Q8. Can a BST become skewed?**

Yes. Inserting sorted values into an ordinary unbalanced BST can produce a skewed tree.

**Q9. What is the height of a balanced BST?**

Its height is O(log n).

**Q10. How can we validate a BST?**

Use minimum and maximum bounds and check that every node falls within its permitted range.

**Q11. What is the Lowest Common Ancestor?**

The lowest node that is an ancestor of both target nodes.

**Q12. What is the difference between a BST and a balanced BST?**

A BST follows ordering rules. A balanced BST additionally maintains its height within a logarithmic bound.

---

## 17. Practice Problems

### Easy

1. Search in a Binary Search Tree.
2. Insert into a Binary Search Tree.
3. Minimum Absolute Difference in BST.
4. Range Sum of BST.
5. Two Sum IV - Input is a BST.

### Medium

1. Delete Node in a BST.
2. Validate Binary Search Tree.
3. Kth Smallest Element in a BST.
4. Lowest Common Ancestor of a BST.
5. Convert Sorted Array to Binary Search Tree.
6. Inorder Successor in BST.

### Hard

1. Recover Binary Search Tree.
2. Serialize and Deserialize BST.
3. Count of Range Sum.
4. Largest BST Subtree.

---

## 18. Quick Revision Cheat Sheet

| Requirement | Approach |
|---|---|
| Search | Compare and move left or right |
| Insert | Find the correct empty position |
| Delete leaf | Remove the node |
| Delete one-child node | Replace with its child |
| Delete two-child node | Replace with inorder successor or predecessor |
| Minimum | Leftmost node |
| Maximum | Rightmost node |
| Sorted output | Inorder traversal |
| Validate BST | Minimum and maximum bounds |
| Kth smallest | Inorder traversal |
| LCA | Find the split point |

### Important formulas

- Balanced BST height: O(log n).
- Skewed BST height: O(n).
- Search average: O(log n).
- Search worst case: O(n).
- Inorder traversal: O(n).

---

## 19. Chapter Completion Checklist

- [ ] Understand BST ordering properties.
- [ ] Implement a BST in Java.
- [ ] Search for a value.
- [ ] Insert a value.
- [ ] Find minimum and maximum.
- [ ] Delete a node in all three cases.
- [ ] Validate a BST using range boundaries.
- [ ] Find the Lowest Common Ancestor.
- [ ] Find the kth smallest element.
- [ ] Understand balanced and skewed BSTs.
- [ ] Solve at least five practice problems.

---

## Next Topic

**08-trees/traversals.md**

In the next chapter, we will study Binary Tree Traversals in detail:

- Inorder Traversal.
- Preorder Traversal.
- Postorder Traversal.
- Level Order Traversal (BFS).
- Recursive and iterative implementations.
- Stack and Queue usage.
- Dry runs and complexity analysis.
- Interview questions and practice problems.