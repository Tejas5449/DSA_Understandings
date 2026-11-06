# Binary Tree in Java

## 1. Introduction to Tree Data Structure

A Tree is a non-linear data structure used to store data in a hierarchical format.

Unlike arrays and linked lists, where elements are arranged sequentially, a tree contains nodes connected through edges.

### Real-world examples

- Folder and file structures.
- Organization hierarchies.
- HTML DOM structure.
- Database indexing.
- Decision-making systems.
- File systems.

### Example of a Tree

```text
             10
           /    \
          20     30
         /  \      \
        40   50     60
```

In this example:
- `10` is the root node.
- `20` and `30` are children of `10`.
- `40`, `50`, and `60` are leaf nodes.

---

## 2. Important Tree Terminology

Understanding tree terminology is important before implementing a Binary Tree.

Consider this tree:

```text
             10
           /    \
          20     30
         /  \    /
        40   50 60
```

### 2.1 Node

A node is an individual element in a tree.

Example: `10`, `20`, `30`, `40`, `50`, and `60` are nodes.

### 2.2 Root

The topmost node of a tree is called the root.

Example: `10`.

A tree has exactly one root when it is non-empty.

### 2.3 Parent

A node that has one or more child nodes is called a parent.

Example:
- `10` is the parent of `20` and `30`.
- `20` is the parent of `40` and `50`.

### 2.4 Child

A node directly connected below another node is called its child.

Example: `20` and `30` are children of `10`.

### 2.5 Siblings

Nodes that share the same parent are called siblings.

Example: `20` and `30` are siblings.

### 2.6 Leaf Node

A node that has no children is called a leaf node.

Example: `40`, `50`, and `60`.

### 2.7 Edge

The connection between two nodes is called an edge.

A tree containing `n` nodes has exactly `n - 1` edges.

### 2.8 Depth

Depth of a node is the number of edges from the root to that node.

Example:
- Depth of `10` = 0.
- Depth of `20` = 1.
- Depth of `40` = 2.

### 2.9 Height

Height of a node is the number of edges in the longest downward path from that node to a leaf.

The height of a tree is the height of its root.

For the example above, the tree height is `2`.

> Convention used in this chapter: a leaf has height 0, and an empty tree has height -1.

### 2.10 Subtree

A subtree is a tree formed by a node and all its descendants.

Example: The subtree rooted at `20` is:

```text
       20
      /  \
     40   50
```

---

## 3. What Is a Binary Tree?

A Binary Tree is a tree in which each node can have at most two children.

These children are called:

- Left child
- Right child

Example:

```text
             10
           /    \
          20     30
         /  \
        40   50
```

Each node can have:
- No children.
- Only a left child.
- Only a right child.
- Both left and right children.

### Important properties

1. Every node has at most two children.
2. A binary tree can be empty.
3. A binary tree with `n` nodes has `n - 1` edges when non-empty.
4. The maximum number of nodes at level `L`, where the root is level 0, is \(2^L\).
5. The maximum number of nodes in a binary tree of height `h` is \(2^{h+1}-1\).

---

## 4. Types of Binary Trees

### 4.1 Full Binary Tree

A Full Binary Tree is a tree in which every node has either zero or two children.

```text
             10
           /    \
          20     30
         /  \
        40   50
```

Every node has either two children or no children.

### 4.2 Complete Binary Tree

A Complete Binary Tree has all levels completely filled except possibly the last level.

The last level must be filled from left to right.

```text
             10
           /    \
          20     30
         /  \
        40   50
```

### 4.3 Perfect Binary Tree

A Perfect Binary Tree has:
- Every internal node with exactly two children.
- All leaf nodes at the same level.

```text
             10
           /    \
          20     30
         /  \   /  \
        40  50 60  70
```

### 4.4 Skewed Binary Tree

A Skewed Binary Tree is a tree in which each node has only one child.

Left-skewed:

```text
        10
       /
      20
     /
    30
   /
  40
```

Right-skewed:

```text
    10
      \
       20
         \
          30
            \
             40
```

A skewed tree can behave like a linked list.

---

## 5. Binary Tree Implementation in Java

A Binary Tree is commonly implemented using a class called `Node`.

Each node contains:
- Data.
- Reference to the left child.
- Reference to the right child.

### Node class

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

### Creating a Binary Tree manually

```java
public class Main {

    public static void main(String[] args) {

        Node root = new Node(10);

        root.left = new Node(20);
        root.right = new Node(30);

        root.left.left = new Node(40);
        root.left.right = new Node(50);

        root.right.left = new Node(60);
    }
}
```

The resulting tree:

```text
             10
           /    \
          20     30
         /  \    /
        40   50 60
```

### Understanding the code

```java
Node root = new Node(10);
```

Creates the root node.

```java
root.left = new Node(20);
```

Creates the left child of the root.

```java
root.right = new Node(30);
```

Creates the right child of the root.

```java
root.left.left = new Node(40);
```

Creates the left child of node `20`.

---

## 6. Basic Binary Tree Operations

Let's implement some common operations.

### 6.1 Count Total Nodes

**Problem:** Count the total number of nodes in a Binary Tree.

### Approach

For every node:
1. Count the current node.
2. Recursively count nodes in the left subtree.
3. Recursively count nodes in the right subtree.
4. Add all three results.

### Java solution

```java
static int countNodes(Node root) {

    if (root == null) {
        return 0;
    }

    int leftCount = countNodes(root.left);

    int rightCount = countNodes(root.right);

    return 1 + leftCount + rightCount;
}
```

### Dry run

```text
        10
       /  \
      20   30
```

- Node `20` contributes 1.
- Node `30` contributes 1.
- Node `10` contributes 1 + 1 + 1.

Output:

```text
3
```

**Time Complexity:** O(n).

**Space Complexity:** O(h) auxiliary recursion stack, where h is tree height.

---

### 6.2 Find the Sum of All Nodes

**Problem:** Calculate the sum of all node values.

### Java solution

```java
static int sumOfNodes(Node root) {

    if (root == null) {
        return 0;
    }

    return root.data
            + sumOfNodes(root.left)
            + sumOfNodes(root.right);
}
```

Example:

```text
        10
       /  \
      20   30
```

Output:

```text
60
```

**Time Complexity:** O(n).

**Space Complexity:** O(h).

---

### 6.3 Find the Maximum Value

**Problem:** Find the maximum value present in the Binary Tree.

### Java solution

```java
static int findMaximum(Node root) {

    if (root == null) {
        return Integer.MIN_VALUE;
    }

    int leftMax = findMaximum(root.left);

    int rightMax = findMaximum(root.right);

    return Math.max(
        root.data,
        Math.max(leftMax, rightMax)
    );
}
```

Example:

```text
        10
       /  \
      50   30
```

Output:

```text
50
```

**Time Complexity:** O(n).

**Space Complexity:** O(h).

---

### 6.4 Find the Height of a Binary Tree

**Problem:** Find the height of a Binary Tree.

Using the convention from this chapter:
- Empty tree height = -1.
- Leaf node height = 0.

### Java solution

```java
static int height(Node root) {

    if (root == null) {
        return -1;
    }

    int leftHeight = height(root.left);

    int rightHeight = height(root.right);

    return 1 + Math.max(leftHeight, rightHeight);
}
```

### Dry run

```text
          10
         /  \
        20   30
       /
      40
```

- Height of `40` = 0.
- Height of `20` = 1.
- Height of `30` = 0.
- Height of `10` = 2.

Output:

```text
2
```

**Time Complexity:** O(n).

**Space Complexity:** O(h).

**Note:** Some problems define the height of an empty tree as 0 and count nodes rather than edges. Always check the convention used in the question.

---

### 6.5 Search for an Element

**Problem:** Check whether a given value exists in the Binary Tree.

### Java solution

```java
static boolean search(Node root, int target) {

    if (root == null) {
        return false;
    }

    if (root.data == target) {
        return true;
    }

    return search(root.left, target)
            || search(root.right, target);
}
```

Example:

```text
        10
       /  \
      20   30
```

Target:

```text
20
```

Output:

```text
true
```

**Time Complexity:** O(n).

**Space Complexity:** O(h).

A normal Binary Tree has no ordering rule, so we may need to visit every node in the worst case.

---

## 7. Level of a Node

The level of a node is commonly defined as its depth plus one.

If the root is at level 1:

```text
             10       Level 1
           /    \
          20     30   Level 2
         /  \
        40   50       Level 3
```

Remember that some problems use root level 0 instead. Follow the question's convention.

---

## 8. Recursive Structure of a Binary Tree

A Binary Tree is naturally recursive.

Every node contains:
- A value.
- A left subtree.
- A right subtree.

Each subtree is itself a Binary Tree.

This is why many Binary Tree problems use recursion.

General pattern:

```java
static void solve(Node root) {

    if (root == null) {
        return;
    }

    // Process current node

    solve(root.left);

    solve(root.right);
}
```

This pattern is the foundation of tree traversal algorithms.

---

## 9. Complete Java Example

The following program combines the Node class and basic Binary Tree operations.

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

    static int countNodes(Node root) {

        if (root == null) {
            return 0;
        }

        return 1
                + countNodes(root.left)
                + countNodes(root.right);
    }

    static int sumOfNodes(Node root) {

        if (root == null) {
            return 0;
        }

        return root.data
                + sumOfNodes(root.left)
                + sumOfNodes(root.right);
    }

    static int height(Node root) {

        if (root == null) {
            return -1;
        }

        return 1 + Math.max(
            height(root.left),
            height(root.right)
        );
    }

    static boolean search(Node root, int target) {

        if (root == null) {
            return false;
        }

        if (root.data == target) {
            return true;
        }

        return search(root.left, target)
                || search(root.right, target);
    }

    public static void main(String[] args) {

        Node root = new Node(10);

        root.left = new Node(20);
        root.right = new Node(30);

        root.left.left = new Node(40);
        root.left.right = new Node(50);

        System.out.println(
            "Total Nodes: " + countNodes(root)
        );

        System.out.println(
            "Sum: " + sumOfNodes(root)
        );

        System.out.println(
            "Height: " + height(root)
        );

        System.out.println(
            "Search 50: " + search(root, 50)
        );
    }
}
```

Output:

```text
Total Nodes: 5
Sum: 150
Height: 2
Search 50: true
```

---

## 10. Binary Tree vs Binary Search Tree

These two terms are often confused in interviews.

| Feature | Binary Tree | Binary Search Tree |
|---|---|---|
| Children | At most two | At most two |
| Ordering | No specific ordering | Ordered according to its comparison rule |
| Search | O(n) | O(log n) average for a balanced tree |
| Insertion | Depends on implementation | Follows BST ordering |
| Duplicate values | Depends on implementation | Depends on the chosen duplicate policy |

A Binary Search Tree (BST) follows this ordering rule:

- Values in the left subtree are smaller than the node.
- Values in the right subtree are greater than the node.

This chapter covers general Binary Trees. BST will be covered separately.

---

## 11. Time and Space Complexity

For the basic operations implemented above:

| Operation | Time Complexity | Auxiliary Space |
|---|---:|---:|
| Count nodes | O(n) | O(h) |
| Sum of nodes | O(n) | O(h) |
| Find maximum | O(n) | O(h) |
| Calculate height | O(n) | O(h) |
| Search | O(n) | O(h) |

Where:
- `n` = total number of nodes.
- `h` = height of the tree.

For a balanced tree, height is approximately O(log n).

For a skewed tree, height can be O(n).

---

## 12. Common Mistakes

1. Forgetting the base case `root == null`.
2. Confusing Binary Tree with Binary Search Tree.
3. Assuming the left child is always smaller than the root in a normal Binary Tree.
4. Forgetting that a leaf node has height 0 under the edge-based convention.
5. Not considering a completely empty tree.
6. Ignoring recursion stack space.
7. Assuming a Binary Tree is always balanced.

---

## 13. Common Interview Questions

**Q1. What is a Tree data structure?**

A Tree is a non-linear hierarchical data structure consisting of nodes connected by edges.

**Q2. What is a Binary Tree?**

A Binary Tree is a tree in which each node can have at most two children.

**Q3. What is the root node?**

The topmost node of a tree.

**Q4. What is a leaf node?**

A node that does not have any children.

**Q5. How many edges are present in a tree with n nodes?**

A non-empty tree has exactly n - 1 edges.

**Q6. What is the maximum number of nodes at level L?**

If the root is at level 0, the maximum is \(2^L\).

**Q7. What is the maximum number of nodes in a Binary Tree of height h?**

\(2^{h+1}-1\), when height is measured in edges.

**Q8. What is a skewed Binary Tree?**

A tree in which each node has at most one child, forming a chain.

**Q9. What is the difference between a Binary Tree and a BST?**

A Binary Tree has no mandatory ordering rule. A BST follows an ordering rule for its left and right subtrees.

**Q10. What is the time complexity of searching in a Binary Tree?**

O(n) in the worst case.

**Q11. What is the height of an empty tree?**

It depends on the convention. In this chapter, it is -1 because height is measured in edges.

**Q12. Why is recursion commonly used in Binary Trees?**

Because each node contains left and right subtrees, which are themselves Binary Trees.

---

## 14. Practice Problems

### Easy

1. Count the number of nodes in a Binary Tree.
2. Find the sum of all nodes.
3. Find the maximum value in a Binary Tree.
4. Find the minimum value in a Binary Tree.
5. Calculate the height of a Binary Tree.
6. Search for a value in a Binary Tree.
7. Count the number of leaf nodes.
8. Count the number of nodes with two children.

### Medium

1. Maximum Depth of Binary Tree.
2. Invert Binary Tree.
3. Same Tree.
4. Symmetric Tree.
5. Diameter of Binary Tree.
6. Balanced Binary Tree.
7. Path Sum.

### Hard

1. Binary Tree Maximum Path Sum.
2. Serialize and Deserialize Binary Tree.
3. Recover a Tree from Traversal Sequences.

---

## 15. Quick Revision Cheat Sheet

| Concept | Meaning |
|---|---|
| Root | Topmost node |
| Parent | Node with child nodes |
| Child | Node connected below a parent |
| Sibling | Nodes with the same parent |
| Leaf | Node without children |
| Edge | Connection between two nodes |
| Depth | Distance from root |
| Height | Longest downward path to a leaf |
| Subtree | A node and its descendants |
| Binary Tree | At most two children per node |
| Full Tree | Every node has 0 or 2 children |
| Complete Tree | Last level filled from left to right |
| Perfect Tree | All internal nodes have two children and all leaves share a level |
| Skewed Tree | Nodes form a one-sided chain |

### Important formulas

When the root is at level 0 and height is measured in edges:

- Maximum nodes at level L = \(2^L\)
- Maximum nodes for height h = \(2^{h+1}-1\)
- Edges in a non-empty tree = `n - 1`
- Height of a leaf = `0`
- Height of an empty tree = `-1` (chapter convention)

---

## 16. Chapter Completion Checklist

- [ ] Understand Tree and Binary Tree.
- [ ] Learn all important tree terminology.
- [ ] Understand Full, Complete, Perfect, and Skewed Trees.
- [ ] Implement a Binary Tree in Java.
- [ ] Count total nodes.
- [ ] Calculate the sum of nodes.
- [ ] Find the maximum value.
- [ ] Calculate tree height.
- [ ] Search for an element.
- [ ] Understand recursion in tree problems.
- [ ] Solve at least five practice problems.

---

## Next Topic

**08-trees/bst.md**

In the next chapter, we will learn:
- What is a Binary Search Tree?
- BST ordering properties.
- Insert a node.
- Search for a value.
- Find minimum and maximum values.
- Delete a node.
- Validate a BST.
- Find the Lowest Common Ancestor.
- Interview questions and practice problems.