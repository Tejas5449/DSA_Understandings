# Bubble Sort in Java

## 1. What Is Sorting?

**Sorting is the process of arranging elements in a particular order.**

The two most common sorting orders are:

- **Ascending order:** Smallest to largest.
- **Descending order:** Largest to smallest.

### Example

Unsorted array:

```text
[5, 2, 8, 1, 3]
```

Ascending order:

```text
[1, 2, 3, 5, 8]
```

Descending order:

```text
[8, 5, 3, 2, 1]
```

Sorting is an important concept in Data Structures and Algorithms because it helps with searching, organizing, and processing data efficiently.

### Common Sorting Algorithms

| Algorithm | Average Time Complexity |
|---|---|
| Bubble Sort | O(n²) |
| Selection Sort | O(n²) |
| Insertion Sort | O(n²) |
| Merge Sort | O(n log n) |
| Quick Sort | O(n log n) |
| Heap Sort | O(n log n) |

In this chapter, we'll focus on **Bubble Sort**.

---

## 2. What Is Bubble Sort?

**Bubble Sort is a simple comparison-based sorting algorithm that repeatedly compares adjacent elements and swaps them if they are in the wrong order.**

After each complete pass, the largest unsorted element moves to its correct position at the end of the array when sorting in ascending order.

This movement is called *bubbling up*.

### Example

Input:

```text
[5, 3, 8, 4, 2]
```

Expected output:

```text
[2, 3, 4, 5, 8]
```

### Main Idea

For ascending order:

1. Compare two adjacent elements.
2. If the left element is greater than the right element, swap them.
3. Move to the next pair.
4. Repeat until the end of the unsorted portion.
5. Continue with another pass until the array is sorted.

## 3. Visual Understanding of Bubble Sort

Consider:

```text
[5, 3, 8, 4, 2]
```

### Pass 1

**Comparison 1: Compare 5 and 3**

```text
[5, 3, 8, 4, 2]
 ^  ^

5 > 3 → Swap
```

Result:

```text
[3, 5, 8, 4, 2]
```

**Comparison 2: Compare 5 and 8**

```text
[3, 5, 8, 4, 2]
    ^  ^

5 < 8 → No swap
```

Result:

```text
[3, 5, 8, 4, 2]
```

**Comparison 3: Compare 8 and 4**

```text
[3, 5, 8, 4, 2]
       ^  ^

8 > 4 → Swap
```

Result:

```text
[3, 5, 4, 8, 2]
```

**Comparison 4: Compare 8 and 2**

```text
[3, 5, 4, 8, 2]
          ^  ^

8 > 2 → Swap
```

Result:

```text
[3, 5, 4, 2, 8]
```

After Pass 1:

```text
[3, 5, 4, 2, 8]
             ^
       Largest element
       is correctly placed
```

### Pass 2

Start:

```text
[3, 5, 4, 2, 8]
```

Comparisons:

```text
3 and 5 → No swap

5 and 4 → Swap

5 and 2 → Swap
```

Result:

```text
[3, 4, 2, 5, 8]
```

The second-largest element, `5`, is now correctly positioned.

### Pass 3

Start:

```text
[3, 4, 2, 5, 8]
```

Comparisons:

```text
3 and 4 → No swap

4 and 2 → Swap
```

Result:

```text
[3, 2, 4, 5, 8]
```

### Pass 4

Start:

```text
[3, 2, 4, 5, 8]
```

Comparison:

```text
3 and 2 → Swap
```

Result:

```text
[2, 3, 4, 5, 8]
```

The array is now sorted.

### Complete Dry Run Summary

| Pass | Array After Pass |
|---|---|
| Initial | `[5, 3, 8, 4, 2]` |
| Pass 1 | `[3, 5, 4, 2, 8]` |
| Pass 2 | `[3, 4, 2, 5, 8]` |
| Pass 3 | `[3, 2, 4, 5, 8]` |
| Pass 4 | `[2, 3, 4, 5, 8]` |

**Important observation:** After each pass, one more element reaches its final sorted position.

---

## 4. Bubble Sort Algorithm

### Pseudocode

```text
bubbleSort(arr):

    n = length of arr

    for i = 0 to n - 2:

        for j = 0 to n - i - 2:

            if arr[j] > arr[j + 1]:

                swap(arr[j], arr[j + 1])
```

### Why Do We Use `n - i - 1`?

After every pass, the largest remaining element is already placed at the end.

Therefore, we do not need to compare elements that are already sorted.

For an array of size `5`:

```text
Pass 1 → 4 comparisons
Pass 2 → 3 comparisons
Pass 3 → 2 comparisons
Pass 4 → 1 comparison
```

Total comparisons:

\[
4 + 3 + 2 + 1 = 10
\]

For an array of size `n`:

\[
\frac{n(n-1)}{2}
\]

This gives a time complexity of:

\[
O(n^2)
\]

## 5. Bubble Sort Java Implementation

```java
import java.util.Arrays;

public class BubbleSort {

    static void bubbleSort(int[] arr) {

        int n = arr.length;

        // Outer loop controls the number of passes
        for (int i = 0; i < n - 1; i++) {

            // Inner loop compares adjacent elements
            for (int j = 0; j < n - i - 1; j++) {

                if (arr[j] > arr[j + 1]) {

                    // Swap adjacent elements
                    int temp = arr[j];

                    arr[j] = arr[j + 1];

                    arr[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 8, 4, 2};

        System.out.println(
                "Before Sorting: " + Arrays.toString(arr)
        );

        bubbleSort(arr);

        System.out.println(
                "After Sorting: " + Arrays.toString(arr)
        );
    }
}
```

### Output

```text
Before Sorting: [5, 3, 8, 4, 2]
After Sorting: [2, 3, 4, 5, 8]
```

### Code Explanation

**Outer loop:**

```java
for (int i = 0; i < n - 1; i++)
```

Controls the number of passes.

For `n` elements, at most `n - 1` passes are required.

**Inner loop:**

```java
for (int j = 0; j < n - i - 1; j++)
```

Compares adjacent elements in the unsorted portion.

**Comparison:**

```java
if (arr[j] > arr[j + 1])
```

Checks whether the two adjacent elements are in the wrong order.

**Swapping:**

```java
int temp = arr[j];
arr[j] = arr[j + 1];
arr[j + 1] = temp;
```

Exchanges the positions of two elements.

## 6. Optimized Bubble Sort

The basic Bubble Sort implementation always performs all `n - 1` passes, even if the array becomes sorted early.

We can improve it by using a boolean variable called `swapped`.

### Optimization Idea

If no swaps happen during a complete pass, the array is already sorted.

Therefore, we can stop immediately.

### Java Implementation

```java
import java.util.Arrays;

public class OptimizedBubbleSort {

    static void bubbleSort(int[] arr) {

        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {

            boolean swapped = false;

            for (int j = 0; j < n - i - 1; j++) {

                if (arr[j] > arr[j + 1]) {

                    int temp = arr[j];

                    arr[j] = arr[j + 1];

                    arr[j + 1] = temp;

                    swapped = true;
                }
            }

            // Stop if no swapping occurred
            if (!swapped) {
                break;
            }
        }
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};

        bubbleSort(arr);

        System.out.println(Arrays.toString(arr));
    }
}
```

### Output

```text
[1, 2, 3, 4, 5]
```

### Why Is This Better?

Consider an already sorted array:

```text
[1, 2, 3, 4, 5]
```

During the first pass:

```text
1 < 2 → No swap
2 < 3 → No swap
3 < 4 → No swap
4 < 5 → No swap
```

Since no swaps occurred, the algorithm stops after the first pass.

The optimized version therefore takes only `O(n)` time for an already sorted array.

## 7. Bubble Sort in Descending Order

To sort elements from largest to smallest, reverse the comparison condition.

For ascending order:

```java
if (arr[j] > arr[j + 1])
```

For descending order:

```java
if (arr[j] < arr[j + 1])
```

### Java Implementation

```java
import java.util.Arrays;

public class BubbleSortDescending {

    static void bubbleSortDescending(int[] arr) {

        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {

            for (int j = 0; j < n - i - 1; j++) {

                if (arr[j] < arr[j + 1]) {

                    int temp = arr[j];

                    arr[j] = arr[j + 1];

                    arr[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 8, 4, 2};

        bubbleSortDescending(arr);

        System.out.println(Arrays.toString(arr));
    }
}
```

**Output:**

```text
[8, 5, 4, 3, 2]
```

## 8. Bubble Sort with Pass-by-Pass Output

Printing the array after each pass is useful for understanding the algorithm and debugging.

```java
import java.util.Arrays;

public class BubbleSortDryRun {

    static void bubbleSort(int[] arr) {

        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {

            for (int j = 0; j < n - i - 1; j++) {

                if (arr[j] > arr[j + 1]) {

                    int temp = arr[j];

                    arr[j] = arr[j + 1];

                    arr[j + 1] = temp;
                }
            }

            System.out.println(
                    "Pass " + (i + 1) + ": " +
                    Arrays.toString(arr)
            );
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 8, 4, 2};

        bubbleSort(arr);
    }
}
```

### Output

```text
Pass 1: [3, 5, 4, 2, 8]
Pass 2: [3, 4, 2, 5, 8]
Pass 3: [3, 2, 4, 5, 8]
Pass 4: [2, 3, 4, 5, 8]
```

## 9. Time Complexity of Bubble Sort

Time complexity measures how the execution time grows as the input size increases.

### Best Case: O(n)

The best case occurs when the array is already sorted and we use the optimized version.

Example:

```text
[1, 2, 3, 4, 5]
```

Only one pass is required to confirm that the array is sorted.

**Best-case complexity of optimized Bubble Sort:**

\[
O(n)
\]

The basic unoptimized version still performs `O(n²)` comparisons.

### Average Case: O(n²)

For a randomly ordered array, Bubble Sort generally requires multiple passes and comparisons.

\[
O(n^2)
\]

### Worst Case: O(n²)

The worst case occurs when the array is sorted in the opposite order.

Example:

```text
[5, 4, 3, 2, 1]
```

Many swaps are required to arrange the elements correctly.

\[
O(n^2)
\]

### Complexity Summary

| Case | Basic Bubble Sort | Optimized Bubble Sort |
|---|---|---|
| Best | O(n²) | O(n) |
| Average | O(n²) | O(n²) |
| Worst | O(n²) | O(n²) |

## 10. Space Complexity

Bubble Sort sorts the array directly without creating another array.

It only requires a temporary variable for swapping.

```java
int temp = arr[j];
arr[j] = arr[j + 1];
arr[j + 1] = temp;
```

Therefore:

\[
O(1)
\]

**Auxiliary Space Complexity: O(1)**

Bubble Sort is an **in-place sorting algorithm**.

## 11. Is Bubble Sort Stable?

Yes. Standard Bubble Sort is a stable sorting algorithm when swaps are performed only for strictly out-of-order elements.

A stable sorting algorithm preserves the original relative order of elements having equal values.

### Example

Suppose we have:

```text
[(4, A), (2, B), (4, C)]
```

Here, `A`, `B`, and `C` are identifiers.

After sorting by numeric value:

```text
[(2, B), (4, A), (4, C)]
```

Notice that `(4, A)` still appears before `(4, C)`.

Their original relative order is preserved.

### Why Is Bubble Sort Stable?

The ascending implementation swaps only when:

```java
arr[j] > arr[j + 1]
```

Equal elements are not swapped.

Changing the comparison to `>=` can destroy stability.

## 12. Important Properties of Bubble Sort

| Property | Bubble Sort |
|---|---|
| Algorithm type | Comparison-based |
| Sorting approach | Adjacent swaps |
| Best time (optimized) | O(n) |
| Average time | O(n²) |
| Worst time | O(n²) |
| Auxiliary space | O(1) |
| In-place | Yes |
| Stable | Yes, with strict comparisons |
| Adaptive | Optimized version can stop early |
| Suitable for large datasets | Generally no |

## 13. Bubble Sort vs Other Sorting Algorithms

| Feature | Bubble Sort | Merge Sort | Quick Sort |
|---|---|---|---|
| Average time | O(n²) | O(n log n) | O(n log n) |
| Worst time | O(n²) | O(n log n) | O(n²) |
| Extra space | O(1) | O(n) for arrays | O(log n) average recursion stack |
| Stable | Yes | Yes, in standard implementation | Usually no |
| Approach | Adjacent swaps | Divide and conquer | Partitioning |

Bubble Sort is easy to understand but inefficient for large arrays.

Merge Sort and Quick Sort are generally more suitable for large datasets.

## 14. Common Bubble Sort Mistakes

### Mistake 1: Incorrect Inner Loop Condition

Incorrect:

```java
for (int j = 0; j < n; j++)
```

This can cause an `ArrayIndexOutOfBoundsException` when accessing:

```java
arr[j + 1]
```

Correct:

```java
for (int j = 0; j < n - i - 1; j++)
```

### Mistake 2: Forgetting to Swap

Incorrect:

```java
if (arr[j] > arr[j + 1]) {
    // No swapping logic
}
```

Correct:

```java
if (arr[j] > arr[j + 1]) {

    int temp = arr[j];

    arr[j] = arr[j + 1];

    arr[j + 1] = temp;
}
```

### Mistake 3: Not Resetting the Swapped Flag

Incorrect:

```java
boolean swapped = false;

for (int i = 0; i < n - 1; i++) {
    // ...
}
```

For the optimized algorithm, reset the flag at the beginning of every pass.

Correct:

```java
for (int i = 0; i < n - 1; i++) {

    boolean swapped = false;

    // Inner loop
}
```

### Mistake 4: Incorrect Comparison for Descending Order

Ascending:

```java
if (arr[j] > arr[j + 1])
```

Descending:

```java
if (arr[j] < arr[j + 1])
```

### Mistake 5: Assuming Bubble Sort Is Efficient for Large Arrays

Bubble Sort has quadratic average and worst-case time complexity.

For large datasets, prefer more efficient sorting algorithms.

## 15. Bubble Sort Interview Questions

### Beginner Level

1. What is Bubble Sort?
2. How does Bubble Sort work?
3. Why is it called Bubble Sort?
4. What is the time complexity of Bubble Sort?
5. What is its space complexity?
6. Is Bubble Sort an in-place algorithm?
7. Is Bubble Sort stable?

### Intermediate Level

1. How can Bubble Sort be optimized?
2. Why do we use `n - i - 1` in the inner loop?
3. How many comparisons are required in the worst case?
4. What happens when the array is already sorted?
5. How do you implement Bubble Sort in descending order?
6. What is the difference between Bubble Sort and Selection Sort?

### Advanced Level

1. Can Bubble Sort achieve O(n) time complexity?
2. Why is Bubble Sort inefficient for large arrays?
3. What is the maximum number of swaps required?
4. How is Bubble Sort related to inversions in an array?
5. What happens to stability if we swap equal elements?

## 16. Important Interview Concept: Number of Swaps

Consider:

```text
[3, 2, 1]
```

To sort the array:

```text
[3, 2, 1]
   ↓
[2, 3, 1]
   ↓
[2, 1, 3]
   ↓
[1, 2, 3]
```

Total swaps:

```text
3
```

The maximum number of swaps for `n` distinct elements occurs when the array is in reverse order.

Formula:

\[
\frac{n(n-1)}{2}
\]

For `n = 5`:

\[
\frac{5(5-1)}{2} = 10
\]

Maximum swaps:

```text
10
```

### Why?

Each swap of adjacent out-of-order elements removes exactly one inversion.

An inversion is a pair of indices `(i, j)` such that:

```text
i < j and arr[i] > arr[j]
```

Standard Bubble Sort performs exactly as many swaps as the number of inversions in the input array.

## 17. Practice Problems

### Beginner

1. Implement Bubble Sort in ascending order.
2. Implement Bubble Sort in descending order.
3. Print the array after every pass.
4. Count the total number of swaps.
5. Count the total number of comparisons.

### Intermediate

1. Implement optimized Bubble Sort.
2. Sort an array containing duplicate elements.
3. Determine whether an array is already sorted.
4. Find the number of inversions using Bubble Sort.
5. Sort an array of objects while preserving the order of equal keys.

### Coding Practice

- [Sort an Array — LeetCode 912](https://leetcode.com/problems/sort-an-array/)
- [Sort Colors — LeetCode 75](https://leetcode.com/problems/sort-colors/)
- [Minimum Number of Moves to Sort an Array — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/minimum-number-swaps-required-sort-array/)

**Note:** These problems are useful for sorting practice, but Bubble Sort is not necessarily the optimal algorithm for them. LeetCode 912, for example, requires a more efficient sorting approach for large inputs.

## 18. Quick Revision

| Concept | Remember |
|---|---|
| Bubble Sort | Compares adjacent elements |
| Swap condition | `arr[j] > arr[j + 1]` |
| Outer loop | Controls passes |
| Inner loop | Compares unsorted adjacent elements |
| Optimization | Stop when no swaps occur |
| Best time (optimized) | O(n) |
| Average time | O(n²) |
| Worst time | O(n²) |
| Auxiliary space | O(1) |
| Stable | Yes |
| In-place | Yes |

### Basic Bubble Sort Template

```java
static void bubbleSort(int[] arr) {

    int n = arr.length;

    for (int i = 0; i < n - 1; i++) {

        for (int j = 0; j < n - i - 1; j++) {

            if (arr[j] > arr[j + 1]) {

                int temp = arr[j];

                arr[j] = arr[j + 1];

                arr[j + 1] = temp;
            }
        }
    }
}
```

### Optimized Bubble Sort Template

```java
static void bubbleSort(int[] arr) {

    int n = arr.length;

    for (int i = 0; i < n - 1; i++) {

        boolean swapped = false;

        for (int j = 0; j < n - i - 1; j++) {

            if (arr[j] > arr[j + 1]) {

                int temp = arr[j];

                arr[j] = arr[j + 1];

                arr[j + 1] = temp;

                swapped = true;
            }
        }

        if (!swapped) {
            break;
        }
    }
}
```

## 19. Learning Checklist

- [ ] Understand what sorting means.
- [ ] Understand how Bubble Sort works.
- [ ] Perform a manual dry run.
- [ ] Implement Bubble Sort in Java.
- [ ] Understand the outer and inner loops.
- [ ] Understand why the inner loop uses `n - i - 1`.
- [ ] Implement optimized Bubble Sort.
- [ ] Implement descending-order Bubble Sort.
- [ ] Understand best, average, and worst-case complexity.
- [ ] Understand why Bubble Sort is stable.
- [ ] Understand why Bubble Sort is in-place.
- [ ] Count swaps and comparisons.
- [ ] Solve at least three sorting practice problems.

---

## Conclusion

Bubble Sort is one of the simplest sorting algorithms and an excellent starting point for learning sorting techniques.

The key idea is:

**Compare adjacent elements, swap them if necessary, and repeat until the array is sorted.**

Although Bubble Sort is not efficient for large datasets, understanding it helps build a strong foundation in array manipulation, nested loops, comparisons, and algorithm complexity.

The next sorting algorithm in this repository is **Merge Sort**, which introduces the Divide and Conquer technique and improves the time complexity to `O(n log n)`.