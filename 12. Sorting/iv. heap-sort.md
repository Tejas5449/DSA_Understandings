# Heap Sort in Java

## 1. What is Heap Sort?

**Heap Sort** is a comparison-based sorting algorithm that uses a Binary Heap data structure to sort elements.

It follows two main steps:

1. Build a Max Heap from the input array.
2. Repeatedly move the largest element (root) to the end of the array and restore the heap.

Heap Sort is an in-place sorting algorithm with a time complexity of **O(n log n)**.

### Example

Input:

```java
int[] arr = {4, 10, 3, 5, 1};
```

Sorted output:

```text
[1, 3, 4, 5, 10]
```

---

## 2. What is a Binary Heap?

A **Binary Heap** is a complete binary tree that follows a specific ordering property.

There are two types:

### Max Heap

Every parent node is greater than or equal to its children.

Example:

```text
        10
       /  \
      5    3
     / \
    4   1
```

The largest element is always at the root.

### Min Heap

Every parent node is less than or equal to its children.

Example:

```text
        1
       / \
      4   3
     / \
    10  5
```

The smallest element is always at the root.

For ascending Heap Sort, we use a **Max Heap**.

---

## 3. How is a Heap Stored in an Array?

A binary heap is commonly represented using an array.

Consider this Max Heap:

```text
        10
       /  \
      5    3
     / \
    4   1
```

Array representation:

```java
int[] heap = {10, 5, 3, 4, 1};
```

For an element at index `i`, the relationships are:

| Node | Formula |
|---|---|
| Parent | `(i - 1) / 2` |
| Left child | `2 * i + 1` |
| Right child | `2 * i + 2` |

Example:

For index `1`, whose value is `5`:

- Parent index = `(1 - 1) / 2 = 0`
- Left child index = `2 * 1 + 1 = 3`
- Right child index = `2 * 1 + 2 = 4`

Therefore:

```text
        10
       /  \
      5    3
     / \
    4   1
```

---

## 4. How Heap Sort Works

Heap Sort has two major phases.

### Phase 1: Build a Max Heap

Convert the input array into a Max Heap.

Input:

```text
[4, 10, 3, 5, 1]
```

After building the Max Heap:

```text
        10
       /  \
      5    3
     / \
    4   1
```

Array:

```text
[10, 5, 3, 4, 1]
```

Now the largest element is at index `0`.

### Phase 2: Extract the Maximum Element

Repeat these steps:

1. Swap the root (largest element) with the last element in the unsorted heap.
2. Reduce the heap size by one.
3. Call `heapify()` to restore the Max Heap.
4. Repeat until only one element remains in the heap.

For example:

```text
Initial Max Heap:
[10, 5, 3, 4, 1]

Swap 10 with 1:
[1, 5, 3, 4, 10]

Heapify the remaining elements:
[5, 4, 3, 1, 10]

Swap 5 with 1:
[1, 4, 3, 5, 10]

Heapify:
[4, 1, 3, 5, 10]

Swap 4 with 3:
[3, 1, 4, 5, 10]

Heapify:
[3, 1, 4, 5, 10]

Swap 3 with 1:
[1, 3, 4, 5, 10]
```

Final sorted array:

```text
[1, 3, 4, 5, 10]
```

---

## 5. What is Heapify?

**Heapify** is the operation that restores the heap property when a node violates it.

For a Max Heap:

- Compare the current node with its left child.
- Compare it with its right child.
- Find the largest among them.
- If a child is larger, swap the current node with that child.
- Continue the process from the child's position.

### Java Heapify Method

```java
public static void heapify(int[] arr, int n, int i) {

    int largest = i;

    int left = 2 * i + 1;
    int right = 2 * i + 2;

    // Check whether the left child is larger
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }

    // Check whether the right child is larger
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    // If the largest element is not the current node
    if (largest != i) {

        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        // Continue heapifying the affected subtree
        heapify(arr, n, largest);
    }
}
```

### Understanding the Parameters

| Parameter | Meaning |
|---|---|
| `arr` | Input array |
| `n` | Current heap size |
| `i` | Index of the node to heapify |

**Important:** `n` represents the size of the active heap, not necessarily the full array length. During sorting, elements beyond `n - 1` are already sorted.

---

## 6. Build a Max Heap

To convert an array into a Max Heap, we start from the last non-leaf node and call `heapify()` moving backwards to the root.

The last non-leaf node is:

```java
int start = n / 2 - 1;
```

Why?

All nodes from index `n / 2` onward are leaf nodes. A leaf node has no children, so it already satisfies the heap property.

### Java Code

```java
public static void buildMaxHeap(int[] arr) {

    int n = arr.length;

    // Start from the last non-leaf node
    for (int i = n / 2 - 1; i >= 0; i--) {

        heapify(arr, n, i);
    }
}
```

### Example

Input:

```text
[4, 10, 3, 5, 1]
```

Array indexes:

```text
Index:  0   1   2   3   4
Value:  4  10   3   5   1
```

Array as a tree:

```text
        4
       / \
     10   3
     / \
    5   1
```

The last non-leaf node is:

```java
n / 2 - 1
= 5 / 2 - 1
= 1
```

First, heapify index `1`:

```text
        4
       / \
     10   3
     / \
    5   1
```

Node `10` is already greater than its children, so no swap is needed.

Next, heapify index `0`:

```text
        4
       / \
     10   3
```

The largest child is `10`, so swap `4` and `10`.

Result:

```text
        10
       /  \
      4    3
     / \
    5   1
```

The node `4` is now smaller than its child `5`, so swap them:

```text
        10
       /  \
      5    3
     / \
    4   1
```

Final Max Heap:

```text
[10, 5, 3, 4, 1]
```

---

## 7. Complete Heap Sort Implementation in Java

```java
import java.util.Arrays;

public class HeapSort {

    // Restores the Max Heap property
    public static void heapify(int[] arr, int n, int i) {

        int largest = i;

        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }

        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }

        if (largest != i) {

            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;

            heapify(arr, n, largest);
        }
    }

    // Converts the array into a Max Heap
    public static void buildMaxHeap(int[] arr) {

        int n = arr.length;

        for (int i = n / 2 - 1; i >= 0; i--) {

            heapify(arr, n, i);
        }
    }

    // Performs Heap Sort
    public static void heapSort(int[] arr) {

        int n = arr.length;

        // Step 1: Build Max Heap
        buildMaxHeap(arr);

        // Step 2: Extract maximum elements
        for (int i = n - 1; i > 0; i--) {

            // Move current maximum to the end
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // Restore heap for remaining elements
            heapify(arr, i, 0);
        }
    }

    public static void main(String[] args) {

        int[] arr = {4, 10, 3, 5, 1};

        System.out.println("Original Array: "
                + Arrays.toString(arr));

        heapSort(arr);

        System.out.println("Sorted Array: "
                + Arrays.toString(arr));
    }
}
```

Output:

```text
Original Array: [4, 10, 3, 5, 1]
Sorted Array: [1, 3, 4, 5, 10]
```

---

## 8. Complete Dry Run

Input:

```text
[4, 10, 3, 5, 1]
```

### Step 1: Build Max Heap

```text
Initial Array:
[4, 10, 3, 5, 1]

After buildMaxHeap():
[10, 5, 3, 4, 1]
```

### Step 2: Sorting Process

| Iteration | Operation | Array |
|---|---|---|
| Initial | Max Heap | `[10, 5, 3, 4, 1]` |
| 1 | Swap 10 and 1, heapify | `[5, 4, 3, 1, 10]` |
| 2 | Swap 5 and 1, heapify | `[4, 1, 3, 5, 10]` |
| 3 | Swap 4 and 3, heapify | `[3, 1, 4, 5, 10]` |
| 4 | Swap 3 and 1 | `[1, 3, 4, 5, 10]` |

Final output:

```text
[1, 3, 4, 5, 10]
```

### Important Observation

After every iteration:

- The largest remaining element is placed at the end.
- The sorted portion grows from right to left.
- The active heap becomes smaller.

---

## 9. Time Complexity of Heap Sort

Heap Sort has three important operations.

| Operation | Time Complexity |
|---|---|
| Heapify one node | O(log n) |
| Build Max Heap | O(n) |
| Extract all elements | O(n log n) |

### Why is Build Heap O(n), not O(n log n)?

A simple upper-bound calculation might suggest that building a heap takes O(n log n), because we call `heapify()` on multiple nodes.

However, most nodes are near the bottom of the tree and require very little work.

- About half the nodes are leaves and require no heapification.
- About one-quarter are one level above the leaves.
- About one-eighth are two levels above the leaves.

The total work across all nodes is O(n).

### Overall Complexity

| Case | Time Complexity |
|---|---|
| Best Case | O(n log n) |
| Average Case | O(n log n) |
| Worst Case | O(n log n) |

### Space Complexity

For the recursive `heapify()` implementation shown above:

- The recursive call stack can use O(log n) space.
- The sorting itself does not require another array.

If `heapify()` is implemented iteratively, Heap Sort uses **O(1) auxiliary space**.

---

## 10. Properties of Heap Sort

| Property | Heap Sort |
|---|---|
| Sorting technique | Comparison-based |
| Data structure | Binary Heap |
| Best time | O(n log n) |
| Average time | O(n log n) |
| Worst time | O(n log n) |
| Auxiliary space | O(1) with iterative heapify |
| In-place | Yes |
| Stable | No |
| Adaptive | No |

### Is Heap Sort Stable?

No.

A stable sorting algorithm preserves the relative order of equal elements.

Heap Sort can change the relative order of equal elements because it swaps elements during heap construction and extraction.

---

## 11. Heap Sort in Descending Order

For ascending order, we use a Max Heap.

For descending order, we can use a Min Heap.

The main difference is that we select the smallest element during heapification.

### Min Heapify

```java
public static void minHeapify(int[] arr, int n, int i) {

    int smallest = i;

    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] < arr[smallest]) {
        smallest = left;
    }

    if (right < n && arr[right] < arr[smallest]) {
        smallest = right;
    }

    if (smallest != i) {

        int temp = arr[i];
        arr[i] = arr[smallest];
        arr[smallest] = temp;

        minHeapify(arr, n, smallest);
    }
}
```

Build a Min Heap, then repeatedly swap its root with the last element of the active heap and heapify the reduced heap.

The result will be in descending order.

---

## 12. Heap Sort vs Quick Sort vs Merge Sort

| Feature | Heap Sort | Quick Sort | Merge Sort |
|---|---|---|---|
| Best Time | O(n log n) | O(n log n) | O(n log n) |
| Average Time | O(n log n) | O(n log n) | O(n log n) |
| Worst Time | O(n log n) | O(n²) | O(n log n) |
| Auxiliary Space | O(1) iterative | O(log n) average stack | O(n) |
| Stable | No | No | Yes |
| In-place | Yes | Generally yes | Generally no |

### When Should We Use Heap Sort?

Heap Sort is useful when:

- Guaranteed O(n log n) worst-case time is important.
- Extra array memory should be avoided.
- Stability is not required.

---

## 13. Common Mistakes in Heap Sort

### Mistake 1: Using the Wrong Child Index

Incorrect:

```java
int left = 2 * i;
int right = 2 * i + 1;
```

Correct for zero-based arrays:

```java
int left = 2 * i + 1;
int right = 2 * i + 2;
```

### Mistake 2: Starting Heap Construction at the Wrong Index

Correct:

```java
for (int i = n / 2 - 1; i >= 0; i--) {
    heapify(arr, n, i);
}
```

Starting at the last non-leaf node avoids unnecessary calls on leaf nodes.

### Mistake 3: Using the Full Array Size During Sorting

Incorrect:

```java
heapify(arr, arr.length, 0);
```

Correct:

```java
heapify(arr, i, 0);
```

After swapping the root with index `i`, the heap contains only the first `i` elements.

### Mistake 4: Forgetting to Heapify After Swapping

After moving the maximum element to the sorted portion, the root may violate the Max Heap property.

Always call:

```java
heapify(arr, i, 0);
```

### Mistake 5: Using a Min Heap for Ascending Heap Sort

For ascending order, use a Max Heap so the largest elements are placed at the end first.

---

## 14. Frequently Asked Interview Questions

### Q1. What is Heap Sort?

Heap Sort is a comparison-based sorting algorithm that uses a binary heap to sort an array.

### Q2. What is the time complexity of Heap Sort?

O(n log n) in the best, average, and worst cases.

### Q3. What is the space complexity of Heap Sort?

O(1) auxiliary space with iterative heapify. A recursive heapify implementation uses O(log n) stack space.

### Q4. Why do we use a Max Heap for ascending sorting?

A Max Heap keeps the largest element at the root. Moving it to the end repeatedly produces ascending order.

### Q5. Why does Heap Sort start from `n / 2 - 1`?

Because all indexes from `n / 2` to `n - 1` are leaf nodes. The last non-leaf node is at `n / 2 - 1`.

### Q6. Is Heap Sort stable?

No. Swapping elements can change the relative order of equal values.

### Q7. Is Heap Sort in-place?

Yes. It rearranges elements within the original array and does not require an additional array proportional to the input size.

### Q8. What is the time complexity of building a heap?

O(n), using the bottom-up heap construction method.

### Q9. What is the difference between Heap Sort and Priority Queue?

A Priority Queue is an abstract data structure that provides access to the highest- or lowest-priority element.

Heap Sort uses a heap to sort all elements in an array.

### Q10. Can Heap Sort have O(n²) worst-case time complexity?

No. Its worst-case time complexity is O(n log n).

---

## 15. Practice Problems

Try implementing Heap Sort yourself before checking the solution.

### Beginner

1. Implement Max Heap construction.
2. Implement Min Heap construction.
3. Implement Heapify for a Max Heap.
4. Sort an array in ascending order using Heap Sort.
5. Sort an array in descending order using Heap Sort.

### Intermediate

1. Find the Kth Largest Element in an Array.
2. Find the Kth Smallest Element in an Array.
3. Find the Top K Frequent Elements.
4. Merge K Sorted Arrays.
5. Find the K Closest Elements.

### Recommended LeetCode Problems

| Problem | Difficulty |
|---|---|
| [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Medium |
| [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | Medium |
| [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium |
| [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/) | Medium |
| [Sort an Array](https://leetcode.com/problems/sort-an-array/) | Medium |

---

## 16. Quick Revision

- Heap Sort uses a Binary Heap.
- A Max Heap is used for ascending sorting.
- A Min Heap can be used for descending sorting.
- Parent index: `(i - 1) / 2`
- Left child: `2 * i + 1`
- Right child: `2 * i + 2`
- Last non-leaf node: `n / 2 - 1`
- `heapify()` restores the heap property.
- Building a heap takes O(n).
- Heap Sort takes O(n log n) in all cases.
- Heap Sort is in-place.
- Heap Sort is not stable.
- Iterative heapify allows O(1) auxiliary space.

---

## 17. Self-Assessment Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain the difference between Max Heap and Min Heap.
- [ ] Convert an array into a Max Heap.
- [ ] Explain the `heapify()` operation.
- [ ] Implement Heapify in Java.
- [ ] Implement Build Max Heap in Java.
- [ ] Implement complete Heap Sort in Java.
- [ ] Dry run Heap Sort without looking at the code.
- [ ] Explain why Build Heap takes O(n).
- [ ] Explain Heap Sort's time and space complexity.
- [ ] Solve at least two heap-related coding problems.

---

## 18. What's Next?

Next, we will start the **Searching** section.

Topic: **Linear Search**

File: `13-searching/linear-search.md`

We will cover linear search, its Java implementation, dry runs, complexity analysis, and interview questions.