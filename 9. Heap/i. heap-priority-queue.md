# Heap and PriorityQueue in Java

## 1. Introduction to Heap

A Heap is a special type of complete Binary Tree that follows a specific ordering property called the Heap Property.

A Heap is commonly used when we need quick access to the minimum or maximum element.

### Real-world applications

- CPU task scheduling.
- Job scheduling systems.
- Priority-based processing.
- Finding the Kth largest or smallest element.
- Top K frequent elements.
- Dijkstra's shortest path algorithm.
- Heap Sort.

### Example

```text
              10
            /    \
           20     30
          /  \
         40   50
```

This is a Min Heap because every parent node is smaller than or equal to its children.

---

## 2. Types of Heap

There are two commonly used types of Heap.

### 2.1 Min Heap

In a Min Heap, the parent node is always smaller than or equal to its children.

The minimum element is always at the root.

Example:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

Properties:

- Root contains the minimum element.
- Parent value <= child values.
- Removing the minimum element requires restoring the Heap Property.

### 2.2 Max Heap

In a Max Heap, the parent node is always greater than or equal to its children.

The maximum element is always at the root.

Example:

```text
              50
            /    \
           40     30
          /  \
         20   10
```

Properties:

- Root contains the maximum element.
- Parent value >= child values.
- Removing the maximum element requires restoring the Heap Property.

### Min Heap vs Max Heap

| Feature | Min Heap | Max Heap |
|---|---|---|
| Root | Minimum element | Maximum element |
| Parent-child rule | Parent <= children | Parent >= children |
| Main use | Get smallest element quickly | Get largest element quickly |
| Java PriorityQueue default | Yes | No, custom comparator required |

---

## 3. Complete Binary Tree Property

A Heap must be a Complete Binary Tree.

A Complete Binary Tree has:
- Every level completely filled except possibly the last level.
- The last level filled from left to right.

Example:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

This is a valid complete Binary Tree.

The following is not complete:

```text
              10
            /    \
           20     30
             \
              50
```

There is a gap before the last node, so it violates the complete-tree property.

---

## 4. Array Representation of Heap

A Heap is commonly implemented using an array.

Consider this Min Heap:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

Array representation:

```text
[10, 20, 30, 40, 50]
```

For an element at index `i`, using zero-based indexing:

| Relationship | Formula |
|---|---|
| Parent | `(i - 1) / 2` |
| Left child | `2 * i + 1` |
| Right child | `2 * i + 2` |

### Example

Array:

```text
[10, 20, 30, 40, 50]
```

For index `1`, value = `20`.

- Parent index = `(1 - 1) / 2 = 0`
- Left child index = `2 * 1 + 1 = 3`
- Right child index = `2 * 1 + 2 = 4`

Therefore:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

---

## 5. What Is PriorityQueue in Java?

Java provides a built-in PriorityQueue implementation.

A PriorityQueue processes elements according to their priority rather than insertion order.

By default, Java PriorityQueue behaves as a Min Heap.

### Syntax

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

### Example

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(40);
        pq.add(10);
        pq.add(30);
        pq.add(20);

        System.out.println(pq.peek());

        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```

Output:

```text
10
10
20
30
40
```

The first `10` is from `peek()`. The remaining values are printed by repeatedly calling `poll()`.

### Important PriorityQueue methods

| Method | Description |
|---|---|
| `add(value)` | Inserts an element |
| `offer(value)` | Inserts an element |
| `peek()` | Returns the highest-priority element without removing it |
| `poll()` | Removes and returns the highest-priority element |
| `remove()` | Removes the head element; throws exception if empty |
| `size()` | Returns number of elements |
| `isEmpty()` | Checks whether the queue is empty |
| `contains(value)` | Checks whether an element exists |
| `clear()` | Removes all elements |

### Important difference

- `peek()` returns the head without removing it.
- `poll()` returns and removes the head.
- `element()` returns the head but throws an exception if empty.
- `remove()` removes the head but throws an exception if empty.

---

## 6. Implement Max Heap Using PriorityQueue

By default, PriorityQueue is a Min Heap.

To create a Max Heap, use `Collections.reverseOrder()`.

### Java example

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        PriorityQueue<Integer> pq =
                new PriorityQueue<>(Collections.reverseOrder());

        pq.add(10);
        pq.add(50);
        pq.add(30);
        pq.add(20);

        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```

Output:

```text
50
30
20
10
```

### Custom comparator

You can also provide a comparator:

```java
PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>((a, b) -> Integer.compare(b, a));
```

Prefer `Integer.compare(b, a)` over `b - a` because subtraction can overflow for extreme integer values.

---

## 7. Heap Operations and Complexity

A Heap supports the following common operations:

| Operation | Time Complexity |
|---|---:|
| Get minimum in Min Heap | O(1) |
| Get maximum in Max Heap | O(1) |
| Insert | O(log n) |
| Remove root | O(log n) |
| Heapify one node | O(log n) |
| Build Heap from an array | O(n) |
| Search for an arbitrary element | O(n) |

The root is always the highest-priority element.

Searching for an arbitrary element is not O(log n), because a Heap is not fully sorted.

---

## 8. Insert an Element into Min Heap

### Approach

Suppose we insert `5` into this Min Heap:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

Steps:

1. Insert the new element at the next available position.
2. Compare it with its parent.
3. If it is smaller than its parent, swap them.
4. Continue until the Heap Property is restored.

After inserting `5`:

```text
              10
            /    \
           20     30
          /  \    /
         40   50  5
```

Now move `5` upward:

```text
              10
            /    \
            5     30
          /  \    /
         40   50  20
```

Then:

```text
               5
            /    \
           10     30
          /  \    /
         40   50  20
```

### Java implementation

```java
static void insertMinHeap(
        ArrayList<Integer> heap, int value) {

    heap.add(value);

    int index = heap.size() - 1;

    while (index > 0) {

        int parent = (index - 1) / 2;

        if (heap.get(index) < heap.get(parent)) {

            Collections.swap(heap, index, parent);

            index = parent;

        } else {
            break;
        }
    }
}
```

**Time Complexity:** O(log n).

---

## 9. Delete Minimum Element from Min Heap

In a Min Heap, the minimum element is at index `0`.

To remove it:

1. Save the root value.
2. Replace the root with the last element.
3. Remove the last array element.
4. Move the new root downward until the Heap Property is restored.

### Example

Before deletion:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

Replace root `10` with `50`:

```text
              50
            /    \
           20     30
          /
         40
```

Move `50` down by swapping with the smaller child:

```text
              20
            /    \
           50     30
          /
         40
```

Continue:

```text
              20
            /    \
           40     30
          /
         50
```

### Java implementation

```java
static int deleteMin(ArrayList<Integer> heap) {

    if (heap.isEmpty()) {
        throw new IllegalStateException("Heap is empty");
    }

    int minimum = heap.get(0);

    int last = heap.remove(heap.size() - 1);

    if (!heap.isEmpty()) {

        heap.set(0, last);

        heapifyDown(heap, 0);
    }

    return minimum;
}

static void heapifyDown(
        ArrayList<Integer> heap, int index) {

    int size = heap.size();

    while (true) {

        int smallest = index;

        int left = 2 * index + 1;
        int right = 2 * index + 2;

        if (left < size &&
                heap.get(left) < heap.get(smallest)) {

            smallest = left;
        }

        if (right < size &&
                heap.get(right) < heap.get(smallest)) {

            smallest = right;
        }

        if (smallest == index) {
            break;
        }

        Collections.swap(heap, index, smallest);

        index = smallest;
    }
}
```

**Time Complexity:** O(log n).

---

## 10. Heapify

Heapify is the process of restoring the Heap Property.

For a Min Heap, the parent should be smaller than or equal to its children.

### Example

Given:

```text
[50, 20, 30, 40, 10]
```

This array does not satisfy the Min Heap property.

After building a Min Heap:

```text
[10, 20, 30, 40, 50]
```

The corresponding tree is:

```text
              10
            /    \
           20     30
          /  \
         40   50
```

### Java implementation: Heapify down

```java
static void heapify(
        int[] arr, int size, int index) {

    int smallest = index;

    int left = 2 * index + 1;
    int right = 2 * index + 2;

    if (left < size &&
            arr[left] < arr[smallest]) {

        smallest = left;
    }

    if (right < size &&
            arr[right] < arr[smallest]) {

        smallest = right;
    }

    if (smallest != index) {

        int temp = arr[index];

        arr[index] = arr[smallest];

        arr[smallest] = temp;

        heapify(arr, size, smallest);
    }
}
```

### Build Min Heap

```java
static void buildMinHeap(int[] arr) {

    int n = arr.length;

    // Start from the last non-leaf node
    for (int i = n / 2 - 1; i >= 0; i--) {

        heapify(arr, n, i);
    }
}
```

### Why start from `n / 2 - 1`?

In an array-based Heap, all nodes from index `n / 2` to `n - 1` are leaf nodes.

Leaf nodes already satisfy the Heap Property because they have no children.

Therefore, we start heapifying from the last non-leaf node.

**Time Complexity of building a Heap:** O(n), not O(n log n).

---

## 11. Heap Sort

Heap Sort is a comparison-based sorting algorithm that uses a Heap.

It can sort an array in ascending order by using a Max Heap.

### Algorithm

1. Build a Max Heap.
2. The maximum element is at index `0`.
3. Swap the root with the last element.
4. Reduce the effective Heap size.
5. Heapify the root.
6. Repeat until the array is sorted.

### Java implementation

```java
import java.util.*;

public class Main {

    static void heapify(
            int[] arr, int size, int index) {

        int largest = index;

        int left = 2 * index + 1;
        int right = 2 * index + 2;

        if (left < size &&
                arr[left] > arr[largest]) {

            largest = left;
        }

        if (right < size &&
                arr[right] > arr[largest]) {

            largest = right;
        }

        if (largest != index) {

            int temp = arr[index];

            arr[index] = arr[largest];

            arr[largest] = temp;

            heapify(arr, size, largest);
        }
    }

    static void heapSort(int[] arr) {

        int n = arr.length;

        // Build Max Heap
        for (int i = n / 2 - 1; i >= 0; i--) {

            heapify(arr, n, i);
        }

        // Extract maximum elements
        for (int i = n - 1; i > 0; i--) {

            int temp = arr[0];

            arr[0] = arr[i];

            arr[i] = temp;

            heapify(arr, i, 0);
        }
    }

    public static void main(String[] args) {

        int[] arr = {40, 10, 30, 50, 20};

        heapSort(arr);

        System.out.println(Arrays.toString(arr));
    }
}
```

Output:

```text
[10, 20, 30, 40, 50]
```

### Complexity

- Time Complexity: O(n log n).
- Auxiliary Space: O(1) for the iterative-array version of Heap Sort.
- Stability: Not stable.

The recursive `heapify` helper uses recursion stack space O(log n) in the worst case. An iterative heapify implementation can keep auxiliary space O(1).

---

## 12. Important Problem: Kth Largest Element

### Problem

Given an integer array and an integer `k`, find the kth largest element.

Input:

```text
nums = [3, 2, 1, 5, 6, 4]
k = 2
```

Output:

```text
5
```

Sorted array:

```text
[1, 2, 3, 4, 5, 6]
```

The second largest element is `5`.

### Approach using Min Heap

Maintain a Min Heap containing only the largest `k` elements.

Steps:

1. Insert each element into the Min Heap.
2. If the heap size becomes greater than `k`, remove the smallest element.
3. After processing all elements, the root is the kth largest element.

### Java solution

```java
import java.util.*;

static int findKthLargest(int[] nums, int k) {

    if (k < 1 || k > nums.length) {
        throw new IllegalArgumentException("Invalid k");
    }

    PriorityQueue<Integer> minHeap = new PriorityQueue<>();

    for (int num : nums) {

        minHeap.offer(num);

        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }

    return minHeap.peek();
}
```

### Dry run

Input:

```text
nums = [3, 2, 1, 5, 6, 4]
k = 2
```

| Number | Heap after processing |
|---:|---|
| 3 | [3] |
| 2 | [2, 3] |
| 1 | [2, 3] |
| 5 | [3, 5] |
| 6 | [5, 6] |
| 4 | [5, 6] |

Final heap:

```text
[5, 6]
```

Root = `5`.

**Time Complexity:** O(n log k).

**Space Complexity:** O(k).

---

## 13. Important Problem: Top K Frequent Elements

### Problem

Given an integer array, return the `k` most frequent elements.

Input:

```text
nums = [1, 1, 1, 2, 2, 3]
k = 2
```

Output:

```text
[1, 2]
```

Explanation:

- `1` appears 3 times.
- `2` appears 2 times.
- `3` appears once.

### Approach

1. Count the frequency of each element using HashMap.
2. Maintain a Min Heap of size `k`.
3. Compare elements by frequency.
4. Remove the least frequent element whenever heap size exceeds `k`.

### Java solution

```java
import java.util.*;

static int[] topKFrequent(int[] nums, int k) {

    Map<Integer, Integer> frequency = new HashMap<>();

    for (int num : nums) {

        frequency.put(
            num,
            frequency.getOrDefault(num, 0) + 1
        );
    }

    PriorityQueue<Integer> minHeap =
            new PriorityQueue<>(
                (a, b) -> Integer.compare(
                    frequency.get(a),
                    frequency.get(b)
                )
            );

    for (int num : frequency.keySet()) {

        minHeap.offer(num);

        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }

    int[] result = new int[k];

    for (int i = 0; i < k; i++) {

        result[i] = minHeap.poll();
    }

    return result;
}
```

**Time Complexity:** O(n + m log k), where `m` is the number of distinct elements.

**Space Complexity:** O(m + k).

When multiple elements have equal frequencies, their relative order is not guaranteed by this implementation.

---

## 14. Important Problem: Kth Smallest Element

### Problem

Find the kth smallest element in an array.

Input:

```text
nums = [7, 10, 4, 3, 20, 15]
k = 3
```

Output:

```text
7
```

### Approach using Max Heap

Maintain a Max Heap of size `k`.

1. Insert each element.
2. If heap size exceeds `k`, remove the maximum.
3. The root is the kth smallest element.

### Java solution

```java
import java.util.*;

static int kthSmallest(int[] nums, int k) {

    if (k < 1 || k > nums.length) {
        throw new IllegalArgumentException("Invalid k");
    }

    PriorityQueue<Integer> maxHeap =
            new PriorityQueue<>(Collections.reverseOrder());

    for (int num : nums) {

        maxHeap.offer(num);

        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }

    return maxHeap.peek();
}
```

**Time Complexity:** O(n log k).

**Space Complexity:** O(k).

---

## 15. Java PriorityQueue with Custom Objects

PriorityQueue can also store custom Java objects.

### Example: Student priority

Suppose we have students with names and marks.

We want students with higher marks to be processed first.

```java
import java.util.*;

class Student {

    String name;
    int marks;

    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }
}

public class Main {

    public static void main(String[] args) {

        PriorityQueue<Student> pq =
                new PriorityQueue<>(
                    (a, b) -> Integer.compare(b.marks, a.marks)
                );

        pq.offer(new Student("Rahul", 80));
        pq.offer(new Student("Amit", 95));
        pq.offer(new Student("Priya", 90));

        while (!pq.isEmpty()) {

            Student student = pq.poll();

            System.out.println(
                student.name + " : " + student.marks
            );
        }
    }
}
```

Output:

```text
Amit : 95
Priya : 90
Rahul : 80
```

This is useful in task scheduling, ranking systems, and priority-based processing.

---

## 16. Important Java PriorityQueue Notes

1. Java PriorityQueue is a Min Heap by default.
2. Use `Collections.reverseOrder()` for a Max Heap.
3. `peek()` retrieves the head without removing it.
4. `poll()` retrieves and removes the head.
5. PriorityQueue does not guarantee sorted iteration order.
6. Repeatedly calling `poll()` retrieves elements in priority order.
7. PriorityQueue allows duplicate elements.
8. Null elements are not allowed.
9. Adding an element takes O(log n) time.
10. Removing the head takes O(log n) time.
11. Searching for an arbitrary element using `contains()` takes O(n).
12. If an object's priority changes after insertion, the queue does not automatically rebuild itself. Remove and reinsert the object to restore the intended ordering.

---

## 17. Heap vs BST vs PriorityQueue

| Feature | Heap | BST | PriorityQueue |
|---|---|---|---|
| Structure | Complete Binary Tree | Binary Tree with ordering | Java collection implementation of a heap |
| Main purpose | Access min/max efficiently | Ordered searching | Priority-based processing |
| Root | Min or max | Depends on inserted values | Highest-priority element |
| Search arbitrary element | O(n) | O(log n) average if balanced | O(n) |
| Insert | O(log n) | O(log n) average if balanced | O(log n) |
| Remove root/head | O(log n) | Depends on operation | O(log n) |

---

## 18. Common Mistakes

1. Assuming Java PriorityQueue is a Max Heap by default.
2. Assuming iterating over PriorityQueue returns sorted values.
3. Using `poll()` when you only need to inspect the root.
4. Forgetting to limit heap size to `k` in Top K problems.
5. Confusing Kth largest with Kth smallest.
6. Assuming arbitrary search in a Heap is O(log n).
7. Forgetting to restore the Heap Property after deletion.
8. Assuming building a Heap takes O(n log n); bottom-up heap construction takes O(n).
9. Using subtraction in a comparator when integer overflow is possible.
10. Forgetting to handle invalid values of `k`.

---

## 19. Common Interview Questions

**Q1. What is a Heap?**

A Heap is a complete Binary Tree that follows the Heap Property.

**Q2. What is a Min Heap?**

A Heap where every parent is smaller than or equal to its children.

**Q3. What is a Max Heap?**

A Heap where every parent is greater than or equal to its children.

**Q4. What is the default behavior of Java PriorityQueue?**

It behaves as a Min Heap.

**Q5. How do you implement a Max Heap in Java?**

Use:

```java
PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>(Collections.reverseOrder());
```

**Q6. What is heapify?**

Heapify restores the Heap Property after insertion, deletion, or during Heap construction.

**Q7. What is the time complexity of insertion in a Heap?**

O(log n).

**Q8. What is the time complexity of removing the root?**

O(log n).

**Q9. What is the time complexity of building a Heap?**

O(n) using bottom-up heap construction.

**Q10. What is Heap Sort complexity?**

O(n log n) time and O(1) auxiliary space for the iterative-array implementation.

**Q11. Is Heap Sort stable?**

No, Heap Sort is not stable.

**Q12. Why is a Heap represented using an array?**

Because a Heap is a complete Binary Tree, so its nodes can be stored compactly without requiring explicit left and right references.

**Q13. What is the difference between Heap and BST?**

A Heap only maintains parent-child ordering. A BST maintains ordering across the entire left and right subtrees.

**Q14. How can we find the kth largest element efficiently?**

Maintain a Min Heap of size `k`.

**Q15. How can we find the kth smallest element efficiently?**

Maintain a Max Heap of size `k`.

---

## 20. Practice Problems

### Easy

1. Last Stone Weight.
2. Kth Largest Element in a Stream.
3. Minimum Cost to Connect Sticks.
4. Take Gifts From the Richest Pile.

### Medium

1. Kth Largest Element in an Array.
2. Top K Frequent Elements.
3. K Closest Points to Origin.
4. Find K Pairs with Smallest Sums.
5. Task Scheduler.
6. Sort Characters by Frequency.
7. Reorganize String.

### Hard

1. Find Median from Data Stream.
2. Merge K Sorted Lists.
3. Smallest Range Covering Elements from K Lists.
4. IPO.

---

## 21. Quick Revision Cheat Sheet

| Requirement | Approach |
|---|---|
| Find minimum quickly | Min Heap |
| Find maximum quickly | Max Heap |
| Kth largest element | Min Heap of size k |
| Kth smallest element | Max Heap of size k |
| Top K frequent elements | HashMap + Min Heap |
| Sort using Heap | Heap Sort |
| Priority-based processing | PriorityQueue |
| Build a Heap | Bottom-up heapify |

### Important formulas

For zero-based array indexing:

- Parent = `(i - 1) / 2`
- Left child = `2 * i + 1`
- Right child = `2 * i + 2`

### Important complexities

- Peek root: O(1).
- Insert: O(log n).
- Remove root: O(log n).
- Build Heap: O(n).
- Heap Sort: O(n log n).
- Search arbitrary element: O(n).

---

## 22. Chapter Completion Checklist

- [ ] Understand Heap data structure.
- [ ] Understand Min Heap and Max Heap.
- [ ] Understand Complete Binary Tree property.
- [ ] Learn array representation of Heap.
- [ ] Use Java PriorityQueue.
- [ ] Implement a Min Heap.
- [ ] Implement insertion and deletion.
- [ ] Understand Heapify.
- [ ] Implement Heap Sort.
- [ ] Solve Kth Largest Element.
- [ ] Solve Kth Smallest Element.
- [ ] Solve Top K Frequent Elements.
- [ ] Solve at least five practice problems.

---

## Next Topic

**10-graphs/graph-basics.md**

In the next chapter, we will learn:

- What is a Graph?
- Vertices and Edges.
- Directed and Undirected Graphs.
- Weighted and Unweighted Graphs.
- Cyclic and Acyclic Graphs.
- Graph representation using adjacency matrix.
- Graph representation using adjacency list.
- Java implementation.
- BFS and DFS introduction.
- Interview questions and practice problems.