# Quick Sort in Java

## 1. What Is Quick Sort?

**Quick Sort is a comparison-based sorting algorithm that uses the Divide and Conquer technique.**

It selects an element called a **pivot**, partitions the array around that pivot, and recursively sorts the smaller partitions.

After partitioning:

- Elements smaller than or equal to the pivot are placed on its left.
- Elements greater than the pivot are placed on its right.
- The pivot reaches its correct sorted position.

### Example

Input:

```text
[8, 3, 5, 4, 7, 6, 1, 2]
```

Sorted output:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

### Main Idea

Quick Sort follows these steps:

1. Select a pivot element.
2. Partition the array around the pivot.
3. Place the pivot in its correct position.
4. Recursively sort the left partition.
5. Recursively sort the right partition.

Unlike Merge Sort, Quick Sort generally performs most of its work during partitioning rather than merging.

---

## 2. Understanding the Pivot

A **pivot** is the element used to divide an array into smaller partitions.

Consider:

```text
[7, 2, 9, 4, 1, 6, 3]
```

Suppose we select `4` as the pivot.

After partitioning, one valid result is:

```text
[2, 1, 3, 4, 9, 6, 7]
```

Here:

- Elements on the left are smaller than or equal to `4`.
- Elements on the right are greater than `4`.
- Pivot `4` is in its final sorted position.

The elements within each partition are not necessarily sorted yet.

Quick Sort will recursively sort them.

## 3. Pivot Selection Strategies

There are different ways to select a pivot.

| Strategy | Description |
|---|---|
| First element | Select the first element |
| Last element | Select the last element |
| Middle element | Select the middle-position element |
| Random pivot | Select a random element |
| Median of three | Select the median among first, middle, and last elements |

### Example

Array:

```text
[8, 3, 5, 4, 7, 6, 1]
```

Possible pivots:

```text
First element  = 8
Last element   = 1
Middle element = 4
```

### Why Does Pivot Selection Matter?

A good pivot divides the array into reasonably balanced partitions.

For example:

```text
Balanced partition:

[1, 2, 3, 4] [5, 6, 7, 8]
```

An unbalanced partition might look like:

```text
[1] [2, 3, 4, 5, 6, 7, 8]
```

Balanced partitions generally lead to better performance.

---

## 4. Quick Sort Algorithm

### Pseudocode

```text
quickSort(arr, low, high):

    if low < high:

        pivotIndex = partition(arr, low, high)

        quickSort(arr, low, pivotIndex - 1)

        quickSort(arr, pivotIndex + 1, high)
```

The `partition()` method rearranges the elements and returns the pivot's final position.

### Understanding the Indexes

Suppose:

```text
arr = [5, 2, 8, 1, 4, 7]

low = 0
high = 5
```

The current partition covers:

```text
Indexes: 0 to 5
```

After partitioning, suppose the pivot reaches index `2`.

Then Quick Sort recursively processes:

```text
Left partition:  0 to 1
Right partition: 3 to 5
```

The pivot itself is excluded because it is already in its final position.

---

## 5. Lomuto Partition Scheme

Lomuto partition is a commonly taught partitioning method.

In this implementation:

- The last element is selected as the pivot.
- `i` tracks the boundary of elements smaller than or equal to the pivot.
- `j` scans the remaining elements.
- Finally, the pivot is swapped into its correct position.

### Example

Input:

```text
[7, 2, 9, 4, 1, 6, 3]
```

Pivot:

```text
3
```

Initial indexes:

```text
low = 0
high = 6
i = low - 1 = -1
j = 0
```

We scan the array until `j` reaches the pivot.

### Step-by-Step Dry Run

| Step | Current element | Comparison with pivot | Action |
|---|---:|---|---|
| 1 | 7 | 7 > 3 | No swap |
| 2 | 2 | 2 <= 3 | Increase `i`, swap |
| 3 | 9 | 9 > 3 | No swap |
| 4 | 4 | 4 > 3 | No swap |
| 5 | 1 | 1 <= 3 | Increase `i`, swap |
| 6 | 6 | 6 > 3 | No swap |

After scanning:

```text
[2, 1, 9, 4, 7, 6, 3]
```

Now swap the pivot with the element at `i + 1`.

```text
[2, 1, 3, 4, 7, 6, 9]
```

Pivot `3` is now at index `2`.

### Lomuto Partition Java Implementation

```java
static int partition(int[] arr, int low, int high) {

    // Select last element as pivot
    int pivot = arr[high];

    // Boundary of smaller elements
    int i = low - 1;

    for (int j = low; j < high; j++) {

        if (arr[j] <= pivot) {

            i++;

            // Swap arr[i] and arr[j]
            int temp = arr[i];

            arr[i] = arr[j];

            arr[j] = temp;
        }
    }

    // Place pivot in its correct position
    int temp = arr[i + 1];

    arr[i + 1] = arr[high];

    arr[high] = temp;

    return i + 1;
}
```

### Important Understanding

The condition:

```java
if (arr[j] <= pivot)
```

ensures that elements smaller than or equal to the pivot are moved toward the left side.

The returned index represents the pivot's final position.

---

## 6. Complete Quick Sort Java Implementation

We can now combine the Lomuto partition method with recursive Quick Sort.

```java
import java.util.Arrays;

public class QuickSort {

    static void quickSort(int[] arr, int low, int high) {

        if (low < high) {

            // Partition the array
            int pivotIndex = partition(arr, low, high);

            // Sort left partition
            quickSort(arr, low, pivotIndex - 1);

            // Sort right partition
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    static int partition(int[] arr, int low, int high) {

        int pivot = arr[high];

        int i = low - 1;

        for (int j = low; j < high; j++) {

            if (arr[j] <= pivot) {

                i++;

                int temp = arr[i];

                arr[i] = arr[j];

                arr[j] = temp;
            }
        }

        int temp = arr[i + 1];

        arr[i + 1] = arr[high];

        arr[high] = temp;

        return i + 1;
    }

    public static void main(String[] args) {

        int[] arr = {8, 3, 5, 4, 7, 6, 1, 2};

        System.out.println(
                "Before Sorting: " + Arrays.toString(arr)
        );

        quickSort(arr, 0, arr.length - 1);

        System.out.println(
                "After Sorting: " + Arrays.toString(arr)
        );
    }
}
```

### Output

```text
Before Sorting: [8, 3, 5, 4, 7, 6, 1, 2]
After Sorting: [1, 2, 3, 4, 5, 6, 7, 8]
```

---

## 7. Complete Quick Sort Dry Run

Input:

```text
[5, 2, 8, 1, 4]
```

We use Lomuto partition with the last element as pivot.

### First Partition

```text
Array: [5, 2, 8, 1, 4]

Pivot = 4
```

Scan the elements:

```text
5 > 4 → No swap

2 <= 4 → Move to left

8 > 4 → No swap

1 <= 4 → Move to left
```

Before placing the pivot:

```text
[2, 1, 8, 5, 4]
```

Place the pivot:

```text
[2, 1, 4, 5, 8]
```

Pivot index:

```text
2
```

Now the partitions are:

```text
Left:  [2, 1]

Pivot: 4

Right: [5, 8]
```

### Second Partition: Left Side

```text
[2, 1]
```

Pivot:

```text
1
```

After partitioning:

```text
[1, 2]
```

### Third Partition: Right Side

```text
[5, 8]
```

Pivot:

```text
8
```

The array remains:

```text
[5, 8]
```

### Final Sorted Array

```text
[1, 2, 4, 5, 8]
```

### Recursion Tree

```text
                  [5, 2, 8, 1, 4]
                         |
                      Pivot 4
                     /       \
                 [2, 1]      [5, 8]
                   |            |
                Pivot 1      Pivot 8
                 /   \        /   \
               []   [2]      [5]  []
```

---

## 8. Hoare Partition Scheme

Another popular partitioning technique is the **Hoare partition scheme**.

Unlike Lomuto partition:

- It commonly selects the first element as the pivot.
- It uses two pointers moving from opposite ends.
- The left pointer searches for an element greater than or equal to the pivot.
- The right pointer searches for an element smaller than or equal to the pivot.
- The elements are swapped when the pointers have not crossed.

### Example

Input:

```text
[8, 3, 1, 7, 0, 10, 2]
```

Pivot:

```text
8
```

The two pointers move toward each other.

The partition process places smaller elements toward the left and larger elements toward the right.

Unlike Lomuto, Hoare partition does not necessarily place the pivot in its final sorted position.

It returns a partition boundary instead.

### Java Implementation

```java
static int hoarePartition(int[] arr, int low, int high) {

    int pivot = arr[low];

    int i = low - 1;

    int j = high + 1;

    while (true) {

        // Move left pointer
        do {
            i++;
        } while (arr[i] < pivot);

        // Move right pointer
        do {
            j--;
        } while (arr[j] > pivot);

        // Pointers crossed
        if (i >= j) {
            return j;
        }

        // Swap elements
        int temp = arr[i];

        arr[i] = arr[j];

        arr[j] = temp;
    }
}
```

### Quick Sort Using Hoare Partition

```java
static void quickSortHoare(int[] arr, int low, int high) {

    if (low < high) {

        int partitionIndex =
                hoarePartition(arr, low, high);

        quickSortHoare(
                arr,
                low,
                partitionIndex
        );

        quickSortHoare(
                arr,
                partitionIndex + 1,
                high
        );
    }
}
```

### Important Difference

For Lomuto:

```java
quickSort(arr, low, pivotIndex - 1);

quickSort(arr, pivotIndex + 1, high);
```

For Hoare:

```java
quickSortHoare(arr, low, partitionIndex);

quickSortHoare(arr, partitionIndex + 1, high);
```

Do not mix the recursive boundaries of these two partition schemes.

---

## 9. Lomuto vs Hoare Partition

| Feature | Lomuto | Hoare |
|---|---|---|
| Common pivot | Last element | First element |
| Pointers | One scanning pointer and one boundary | Two pointers |
| Swapping | Moves smaller elements left | Swaps misplaced elements |
| Return value | Final pivot index | Partition boundary |
| Recursive ranges | Exclude pivot index | Split around boundary |
| Typical swaps | Often more | Often fewer |
| Implementation | Simpler to understand | Slightly more complex |

Both schemes can correctly partition an array when their respective recursion boundaries are used.

---

## 10. Quick Sort in Descending Order

To sort in descending order using Lomuto partition, change the comparison condition.

Ascending:

```java
if (arr[j] <= pivot)
```

Descending:

```java
if (arr[j] >= pivot)
```

### Java Implementation

```java
static int partitionDescending(
        int[] arr,
        int low,
        int high) {

    int pivot = arr[high];

    int i = low - 1;

    for (int j = low; j < high; j++) {

        if (arr[j] >= pivot) {

            i++;

            int temp = arr[i];

            arr[i] = arr[j];

            arr[j] = temp;
        }
    }

    int temp = arr[i + 1];

    arr[i + 1] = arr[high];

    arr[high] = temp;

    return i + 1;
}

static void quickSortDescending(
        int[] arr,
        int low,
        int high) {

    if (low < high) {

        int pivotIndex =
                partitionDescending(arr, low, high);

        quickSortDescending(
                arr,
                low,
                pivotIndex - 1
        );

        quickSortDescending(
                arr,
                pivotIndex + 1,
                high
        );
    }
}
```

Example:

```text
Input:  [5, 2, 8, 1, 4]

Output: [8, 5, 4, 2, 1]
```

---

## 11. Time Complexity of Quick Sort

Quick Sort's performance depends on how balanced the partitions are.

### Best Case: O(n log n)

The best case occurs when the pivot divides the array into two nearly equal parts.

Example:

```text
                [1, 2, 3, 4, 5, 6, 7, 8]
                         |
                 -----------------
                 |               |
             [1, 2, 3, 4]   [5, 6, 7, 8]
```

The recursion depth is approximately:

\[
\log_2(n)
\]

Each level performs approximately `O(n)` partitioning work.

Therefore:

\[
O(n \log n)
\]

### Average Case: O(n log n)

For reasonably balanced partitions, Quick Sort performs:

\[
O(n \log n)
\]

This is why Quick Sort is generally efficient in practice.

### Worst Case: O(n²)

The worst case occurs when each partition is extremely unbalanced.

For example:

```text
[1, 2, 3, 4, 5]
```

If the last element is always selected as the pivot:

```text
First partition:  [1, 2, 3, 4] [5]

Second partition: [1, 2, 3] [4]

Third partition:  [1, 2] [3]

Fourth partition: [1] [2]
```

The recursion depth becomes approximately `n`.

Total partitioning work becomes:

\[
O(n^2)
\]

### Complexity Summary

| Case | Time Complexity |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n²) |

---

## 12. Space Complexity of Quick Sort

Quick Sort sorts the array in place, so it does not require another array of size `n`.

However, recursive method calls consume stack space.

| Case | Auxiliary Stack Space |
|---|---|
| Best | O(log n) |
| Average | O(log n) |
| Worst | O(n) |

**Important:** Quick Sort is usually described as an in-place sorting algorithm, but its recursive implementation still uses additional stack memory.

---

## 13. Is Quick Sort Stable?

Standard Quick Sort implementations using in-place partitioning are generally **not stable**.

A stable sorting algorithm preserves the relative order of elements having equal keys.

During partitioning, elements may be swapped across each other, changing the order of equal elements.

### Example

Suppose the input is:

```text
[(4, A), (2, B), (4, C)]
```

Here, `A` and `C` have equal sorting keys.

A stable sort must preserve:

```text
(4, A) before (4, C)
```

Standard in-place Quick Sort does not guarantee this.

---

## 14. Optimizing Quick Sort

There are several ways to improve Quick Sort's performance.

### 1. Random Pivot Selection

Instead of always selecting the first or last element, choose a random pivot.

This reduces the chance of consistently unbalanced partitions on already ordered or specially arranged inputs.

Example:

```java
import java.util.Random;

static Random random = new Random();

static int randomizedPartition(
        int[] arr,
        int low,
        int high) {

    int randomIndex =
            low + random.nextInt(high - low + 1);

    int temp = arr[randomIndex];

    arr[randomIndex] = arr[high];

    arr[high] = temp;

    return partition(arr, low, high);
}
```

Use `randomizedPartition()` in the Quick Sort implementation instead of `partition()`.

Randomization improves expected performance, but the worst-case time complexity remains `O(n²)`.

### 2. Median-of-Three Pivot

Select the median value among:

- First element.
- Middle element.
- Last element.

This can improve pivot quality on many inputs.

It is a practical heuristic, not a guarantee of balanced partitions.

### 3. Three-Way Partitioning

Three-way partitioning divides the array into three sections:

```text
Elements < pivot

Elements = pivot

Elements > pivot
```

This is especially useful when the array contains many duplicate values.

### Java Implementation: Three-Way Quick Sort

```java
static void quickSortThreeWay(
        int[] arr,
        int low,
        int high) {

    if (low >= high) {
        return;
    }

    int pivot = arr[low];

    int lt = low;

    int i = low + 1;

    int gt = high;

    while (i <= gt) {

        if (arr[i] < pivot) {

            int temp = arr[lt];

            arr[lt] = arr[i];

            arr[i] = temp;

            lt++;
            i++;

        } else if (arr[i] > pivot) {

            int temp = arr[i];

            arr[i] = arr[gt];

            arr[gt] = temp;

            gt--;

        } else {

            i++;
        }
    }

    // Sort elements smaller than pivot
    quickSortThreeWay(arr, low, lt - 1);

    // Sort elements greater than pivot
    quickSortThreeWay(arr, gt + 1, high);
}
```

The middle section containing values equal to the pivot does not need further sorting.

Three-way partitioning can be very efficient when many values are equal.

---

## 15. Quick Sort vs Merge Sort

Both algorithms use Divide and Conquer, but their approaches are different.

| Feature | Quick Sort | Merge Sort |
|---|---|---|
| Technique | Partition around pivot | Divide into halves |
| Best time | O(n log n) | O(n log n) |
| Average time | O(n log n) | O(n log n) |
| Worst time | O(n²) | O(n log n) |
| Auxiliary space | O(log n) average stack | O(n) |
| Stable | Usually no | Yes |
| In-place | Usually considered yes, excluding stack | No, standard array implementation |
| Main operation | Partitioning | Merging |

### When Should You Use Quick Sort?

Quick Sort is useful when:

- You want efficient average-case sorting.
- You want an in-place partitioning approach.
- Additional array-sized memory is undesirable.
- The pivot strategy is chosen carefully.

### When Should You Use Merge Sort?

Merge Sort is useful when:

- Stable sorting is required.
- Guaranteed `O(n log n)` worst-case time is important.
- You are sorting linked lists.
- Additional memory is acceptable.

---

## 16. Common Quick Sort Mistakes

### Mistake 1: Incorrect Base Condition

Correct:

```java
if (low < high) {
    // Partition and recursive calls
}
```

When `low >= high`, the partition contains zero or one element and is already sorted.

### Mistake 2: Incorrect Recursive Boundaries

For Lomuto partition:

```java
quickSort(arr, low, pivotIndex - 1);

quickSort(arr, pivotIndex + 1, high);
```

For Hoare partition:

```java
quickSortHoare(arr, low, partitionIndex);

quickSortHoare(arr, partitionIndex + 1, high);
```

### Mistake 3: Infinite Recursion

Using incorrect partition boundaries may cause the same partition to be processed repeatedly.

Always verify that each recursive call works on a smaller range.

### Mistake 4: Incorrect Pivot Placement

In Lomuto partition, the pivot must be swapped into its final position before returning its index.

### Mistake 5: Assuming Quick Sort Is Always O(n log n)

Quick Sort can take `O(n²)` time with highly unbalanced partitions.

### Mistake 6: Forgetting Duplicate Values

Arrays with many duplicate values can lead to unnecessary recursive work with basic two-way partitioning.

Three-way partitioning can improve performance for such inputs.

### Mistake 7: Confusing Lomuto and Hoare Partition

Their return values have different meanings.

Do not use the recursive boundaries of one partition scheme with the other.

---

## 17. Important Quick Sort Interview Questions

### Beginner Level

1. What is Quick Sort?
2. What is a pivot?
3. How does partitioning work?
4. What is Divide and Conquer?
5. What is the time complexity of Quick Sort?
6. Is Quick Sort stable?
7. Is Quick Sort an in-place algorithm?

### Intermediate Level

1. Explain Quick Sort with a dry run.
2. What is the difference between Lomuto and Hoare partition?
3. Why does pivot selection matter?
4. What causes the worst case in Quick Sort?
5. Why is Quick Sort often fast in practice?
6. How can Quick Sort be optimized?
7. What is randomized Quick Sort?

### Advanced Level

1. Explain three-way partitioning.
2. How does Quick Sort handle duplicate values?
3. Why does randomized pivot selection improve expected performance?
4. What is the average recursion depth of Quick Sort?
5. How can Quick Sort be implemented iteratively?
6. Compare Quick Sort with Merge Sort.
7. How can tail-recursion techniques reduce stack usage?

---

## 18. Practice Problems

### Easy

1. [Sort an Array](https://leetcode.com/problems/sort-an-array/)
2. [Sort Colors](https://leetcode.com/problems/sort-colors/)

### Medium

1. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
3. [Sort List](https://leetcode.com/problems/sort-list/)

### Advanced

1. [Wiggle Sort II](https://leetcode.com/problems/wiggle-sort-ii/)
2. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

These problems develop partitioning and selection skills. Not all of them require Quick Sort specifically.

---

## 19. Quick Revision

| Concept | Remember |
|---|---|
| Quick Sort | Divide and Conquer |
| Pivot | Element used to partition the array |
| Partitioning | Places smaller and larger elements on different sides |
| Lomuto | Commonly uses last element as pivot |
| Hoare | Commonly uses first element as pivot |
| Best time | O(n log n) |
| Average time | O(n log n) |
| Worst time | O(n²) |
| Average stack space | O(log n) |
| Worst stack space | O(n) |
| Stable | Usually no |
| In-place | Yes, excluding recursion stack |

### Quick Sort Template: Lomuto

```java
static void quickSort(int[] arr, int low, int high) {

    if (low < high) {

        int pivotIndex = partition(arr, low, high);

        quickSort(arr, low, pivotIndex - 1);

        quickSort(arr, pivotIndex + 1, high);
    }
}
```

### Partition Template: Lomuto

```java
static int partition(int[] arr, int low, int high) {

    int pivot = arr[high];

    int i = low - 1;

    for (int j = low; j < high; j++) {

        if (arr[j] <= pivot) {

            i++;

            int temp = arr[i];

            arr[i] = arr[j];

            arr[j] = temp;
        }
    }

    int temp = arr[i + 1];

    arr[i + 1] = arr[high];

    arr[high] = temp;

    return i + 1;
}
```

---

## 20. Learning Checklist

- [ ] Understand Quick Sort.
- [ ] Understand pivot selection.
- [ ] Understand partitioning.
- [ ] Implement Lomuto partition in Java.
- [ ] Implement recursive Quick Sort.
- [ ] Perform a manual dry run.
- [ ] Understand Hoare partition.
- [ ] Understand the difference between Lomuto and Hoare.
- [ ] Implement descending-order Quick Sort.
- [ ] Understand best, average, and worst-case complexity.
- [ ] Understand recursion stack space.
- [ ] Understand randomized pivot selection.
- [ ] Understand three-way partitioning.
- [ ] Compare Quick Sort with Merge Sort.
- [ ] Solve at least three sorting-related problems.

---

## Conclusion

Quick Sort is an important sorting algorithm that uses Divide and Conquer.

The key idea is:

**Choose a pivot, partition the array, and recursively sort the partitions.**

Remember these important points:

1. Pivot selection affects performance.
2. Lomuto and Hoare use different partitioning techniques.
3. Average time complexity is `O(n log n)`.
4. Worst-case time complexity is `O(n²)`.
5. Standard in-place Quick Sort is generally not stable.

The next sorting algorithm in this repository is **Heap Sort**, which uses a Binary Heap to sort elements efficiently with `O(n log n)` worst-case time complexity.