# Merge Sort in Java

## 1. What Is Merge Sort?

**Merge Sort is a comparison-based sorting algorithm that uses the Divide and Conquer technique to sort an array.**

It divides an array into smaller subarrays, sorts those subarrays recursively, and then merges them to produce one sorted array.

Unlike Bubble Sort, which repeatedly compares adjacent elements, Merge Sort divides the problem into smaller parts.

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

Merge Sort follows three steps:

1. **Divide:** Divide the array into two halves.
2. **Conquer:** Recursively sort both halves.
3. **Combine:** Merge the two sorted halves into one sorted array.

This is called the **Divide and Conquer approach**.

## 2. Understanding Divide and Conquer

Divide and Conquer is an algorithmic technique that breaks a large problem into smaller subproblems.

Each subproblem is solved independently, and the results are combined to solve the original problem.

It has three main steps:

| Step | Description |
|---|---|
| Divide | Break the problem into smaller parts |
| Conquer | Solve each smaller problem |
| Combine | Merge the results |

Examples of algorithms using Divide and Conquer:

- Merge Sort
- Quick Sort
- Binary Search

## 3. How Merge Sort Works

Consider this array:

```text
[8, 3, 5, 4, 7, 6, 1, 2]
```

### Step 1: Divide the Array

Split the array into two halves:

```text
              [8, 3, 5, 4, 7, 6, 1, 2]
                         |
              ---------------------
              |                   |
          [8, 3, 5, 4]        [7, 6, 1, 2]
```

Continue dividing:

```text
       [8, 3, 5, 4]              [7, 6, 1, 2]
          /     \                  /     \
      [8, 3]   [5, 4]          [7, 6]   [1, 2]
       / \       / \             / \       / \
      [8][3]   [5][4]           [7][6]   [1][2]
```

Continue until every subarray contains only one element.

A single-element array is already sorted.

### Step 2: Merge Smaller Arrays

Merge the single-element arrays in sorted order.

```text
[8] + [3] -> [3, 8]

[5] + [4] -> [4, 5]

[7] + [6] -> [6, 7]

[1] + [2] -> [1, 2]
```

Now:

```text
[3, 8]   [4, 5]   [6, 7]   [1, 2]
```

### Step 3: Merge Again

```text
[3, 8] + [4, 5] -> [3, 4, 5, 8]

[6, 7] + [1, 2] -> [1, 2, 6, 7]
```

Now:

```text
[3, 4, 5, 8]     [1, 2, 6, 7]
```

### Step 4: Final Merge

Merge both sorted halves:

```text
[3, 4, 5, 8] + [1, 2, 6, 7]
```

Compare elements one by one:

```text
1 < 3  -> Take 1
2 < 3  -> Take 2
3 < 6  -> Take 3
4 < 6  -> Take 4
5 < 6  -> Take 5
6 < 8  -> Take 6
7 < 8  -> Take 7
Remaining element -> 8
```

Final sorted array:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

## 4. Merge Sort Recursion Tree

The recursion tree shows how the array is divided into smaller parts.

```text
                    [8,3,5,4,7,6,1,2]
                           |
                -----------------------
                |                     |
             [8,3,5,4]             [7,6,1,2]
                |                     |
             --------              --------
             |      |              |      |
           [8,3]  [5,4]           [7,6]  [1,2]
             |      |              |      |
           -----  -----          -----  -----
           |   |  |   |          |   |  |   |
          [8] [3][5] [4]         [7] [6][1] [2]
```

The splitting process creates approximately:

\[
\log_2(n)
\]

levels for an array of size `n`.

At each level, merging processes a total of `n` elements.

Therefore:

\[
O(n \log n)
\]

## 5. Merge Sort Algorithm

### Pseudocode

```text
mergeSort(arr, left, right):

    if left >= right:
        return

    mid = left + (right - left) / 2

    mergeSort(arr, left, mid)

    mergeSort(arr, mid + 1, right)

    merge(arr, left, mid, right)
```

The `merge()` method combines two sorted subarrays.

### Understanding the Indexes

Suppose:

```text
arr = [2, 5, 8, 1, 4, 7]

left = 0
right = 5
```

Calculate the middle:

```java
int mid = left + (right - left) / 2;
```

Result:

```text
mid = 2
```

The two halves are:

```text
Left half:  [2, 5, 8]
Right half: [1, 4, 7]
```

Their indexes are:

```text
Left:  left to mid

Right: mid + 1 to right
```

## 6. Java Implementation of Merge Sort

```java
import java.util.Arrays;

public class MergeSort {

    static void mergeSort(int[] arr, int left, int right) {

        // Base case
        if (left >= right) {
            return;
        }

        // Find middle index
        int mid = left + (right - left) / 2;

        // Sort left half
        mergeSort(arr, left, mid);

        // Sort right half
        mergeSort(arr, mid + 1, right);

        // Merge both sorted halves
        merge(arr, left, mid, right);
    }

    static void merge(
            int[] arr,
            int left,
            int mid,
            int right) {

        int n1 = mid - left + 1;

        int n2 = right - mid;

        int[] leftArray = new int[n1];

        int[] rightArray = new int[n2];

        // Copy left subarray
        for (int i = 0; i < n1; i++) {

            leftArray[i] = arr[left + i];
        }

        // Copy right subarray
        for (int j = 0; j < n2; j++) {

            rightArray[j] = arr[mid + 1 + j];
        }

        int i = 0;
        int j = 0;
        int k = left;

        // Compare and merge elements
        while (i < n1 && j < n2) {

            if (leftArray[i] <= rightArray[j]) {

                arr[k] = leftArray[i];

                i++;

            } else {

                arr[k] = rightArray[j];

                j++;
            }

            k++;
        }

        // Copy remaining elements from left array
        while (i < n1) {

            arr[k] = leftArray[i];

            i++;
            k++;
        }

        // Copy remaining elements from right array
        while (j < n2) {

            arr[k] = rightArray[j];

            j++;
            k++;
        }
    }

    public static void main(String[] args) {

        int[] arr = {8, 3, 5, 4, 7, 6, 1, 2};

        System.out.println(
                "Before Sorting: " + Arrays.toString(arr)
        );

        mergeSort(arr, 0, arr.length - 1);

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

## 7. Understanding the Merge Method

The `merge()` method is the most important part of Merge Sort.

It combines two already sorted subarrays.

Consider:

```text
Left array:  [2, 5, 8]

Right array: [1, 4, 7]
```

Initially:

```text
i = 0
j = 0
k = 0
```

Where:

- `i` points to the left array.
- `j` points to the right array.
- `k` points to the original array.

### Step-by-Step Merge

**Comparison 1**

```text
2 and 1

1 is smaller.
```

Insert `1` into the original array.

```text
[1, _, _, _, _, _]
```

**Comparison 2**

```text
2 and 4

2 is smaller.
```

Insert `2`.

```text
[1, 2, _, _, _, _]
```

**Comparison 3**

```text
5 and 4

4 is smaller.
```

Insert `4`.

```text
[1, 2, 4, _, _, _]
```

**Comparison 4**

```text
5 and 7

5 is smaller.
```

Insert `5`.

```text
[1, 2, 4, 5, _, _]
```

**Comparison 5**

```text
8 and 7

7 is smaller.
```

Insert `7`.

```text
[1, 2, 4, 5, 7, _]
```

The right array is now exhausted.

Copy the remaining left element:

```text
[1, 2, 4, 5, 7, 8]
```

The final merged array is sorted.

### Why Do We Need Two Additional Loops?

The main comparison loop stops when either the left or right subarray is exhausted.

But the other subarray may still contain elements.

Therefore, we copy the remaining elements:

```java
while (i < n1) {
    arr[k] = leftArray[i];
    i++;
    k++;
}
```

And:

```java
while (j < n2) {
    arr[k] = rightArray[j];
    j++;
    k++;
}
```

Without these loops, some elements would be missing from the merged result.

## 8. Complete Dry Run

Input:

```text
[5, 2, 8, 1]
```

### Division

```text
                 [5, 2, 8, 1]
                       |
               ----------------
               |              |
             [5, 2]         [8, 1]
               |              |
             ------         ------
             |    |         |    |
            [5]  [2]       [8]  [1]
```

### First Merge

```text
[5] + [2] -> [2, 5]

[8] + [1] -> [1, 8]
```

### Final Merge

```text
[2, 5] + [1, 8]
```

Comparisons:

| Comparison | Selected |
|---|---|
| 2 vs 1 | 1 |
| 2 vs 8 | 2 |
| 5 vs 8 | 5 |
| Remaining | 8 |

Final result:

```text
[1, 2, 5, 8]
```

## 9. Merge Sort in Descending Order

To sort in descending order, change the comparison condition in the merge method.

Ascending:

```java
if (leftArray[i] <= rightArray[j])
```

Descending:

```java
if (leftArray[i] >= rightArray[j])
```

### Java Implementation

```java
static void mergeDescending(
        int[] arr,
        int left,
        int mid,
        int right) {

    int n1 = mid - left + 1;

    int n2 = right - mid;

    int[] leftArray = new int[n1];

    int[] rightArray = new int[n2];

    for (int i = 0; i < n1; i++) {
        leftArray[i] = arr[left + i];
    }

    for (int j = 0; j < n2; j++) {
        rightArray[j] = arr[mid + 1 + j];
    }

    int i = 0;
    int j = 0;
    int k = left;

    while (i < n1 && j < n2) {

        if (leftArray[i] >= rightArray[j]) {

            arr[k++] = leftArray[i++];

        } else {

            arr[k++] = rightArray[j++];
        }
    }

    while (i < n1) {
        arr[k++] = leftArray[i++];
    }

    while (j < n2) {
        arr[k++] = rightArray[j++];
    }
}
```

Use this method instead of the ascending `merge()` method in the recursive Merge Sort implementation.

## 10. Time Complexity of Merge Sort

Merge Sort divides the array into two halves at every level.

The number of levels is approximately:

\[
\log_2(n)
\]

At each level, merging takes:

\[
O(n)
\]

Therefore:

\[
T(n) = 2T(n/2) + O(n)
\]

Solving this recurrence gives:

\[
O(n \log n)
\]

### Best Case

Even if the array is already sorted, Merge Sort continues dividing and merging.

\[
O(n \log n)
\]

### Average Case

\[
O(n \log n)
\]

### Worst Case

\[
O(n \log n)
\]

### Complexity Summary

| Case | Time Complexity |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n log n) |

Unlike Bubble Sort, Merge Sort maintains `O(n log n)` time complexity in all three cases.

## 11. Space Complexity

Merge Sort creates temporary arrays during merging.

For an array of size `n`, the temporary arrays require:

\[
O(n)
\]

The recursive call stack requires:

\[
O(\log n)
\]

Therefore, the total auxiliary space complexity is:

\[
O(n)
\]

**Important:** Standard array-based Merge Sort is not an in-place sorting algorithm because it requires additional temporary storage.

## 12. Is Merge Sort Stable?

Yes. Standard Merge Sort can be stable.

A stable sorting algorithm preserves the original relative order of elements with equal sorting keys.

In our implementation, we use:

```java
if (leftArray[i] <= rightArray[j])
```

When both values are equal, we take the element from the left subarray first.

This preserves the original order of equal elements because the left subarray contains earlier elements from the original array.

### Example

Input:

```text
[(4, A), (2, B), (4, C)]
```

After sorting by numeric value:

```text
[(2, B), (4, A), (4, C)]
```

The relative order of `A` and `C` remains unchanged.

## 13. Important Properties of Merge Sort

| Property | Merge Sort |
|---|---|
| Algorithm type | Comparison-based |
| Approach | Divide and Conquer |
| Best time | O(n log n) |
| Average time | O(n log n) |
| Worst time | O(n log n) |
| Auxiliary space | O(n) |
| In-place | No, for standard array implementation |
| Stable | Yes |
| Recursive | Yes, in the standard implementation |

## 14. Merge Sort vs Bubble Sort

| Feature | Bubble Sort | Merge Sort |
|---|---|---|
| Technique | Adjacent comparison and swapping | Divide and Conquer |
| Best time | O(n) optimized | O(n log n) |
| Average time | O(n²) | O(n log n) |
| Worst time | O(n²) | O(n log n) |
| Extra space | O(1) | O(n) |
| Stable | Yes | Yes |
| In-place | Yes | No, standard array implementation |

### Which One Is Better?

For large arrays, Merge Sort is generally much faster.

Bubble Sort is useful for learning basic sorting concepts, but Merge Sort is more suitable for larger datasets when its additional memory requirement is acceptable.

## 15. Merge Sort Applications

Merge Sort is useful in several situations:

1. Sorting large datasets.
2. Sorting linked lists.
3. External sorting when data does not fit into memory.
4. Counting inversions in an array.
5. Merging multiple sorted sequences.
6. Stable sorting when preserving the order of equal elements is important.

### Why Is Merge Sort Good for Linked Lists?

Linked lists do not support efficient random access.

Merge Sort works well because:

- A linked list can be split using slow and fast pointers.
- Two sorted linked lists can be merged by adjusting pointers.
- It does not require copying the entire list into temporary arrays.

## 16. Common Merge Sort Mistakes

### Mistake 1: Incorrect Base Case

Incorrect:

```java
if (left == right) {
    return;
}
```

This works for valid non-empty ranges, but a safer general condition is:

```java
if (left >= right) {
    return;
}
```

It also handles an empty range.

### Mistake 2: Incorrect Middle Calculation

Avoid:

```java
int mid = (left + right) / 2;
```

For very large indexes, the addition could overflow.

Prefer:

```java
int mid = left + (right - left) / 2;
```

### Mistake 3: Incorrect Right Subarray Start

Incorrect:

```java
mergeSort(arr, mid, right);
```

Correct:

```java
mergeSort(arr, mid + 1, right);
```

Using `mid` again may cause infinite recursion.

### Mistake 4: Forgetting to Copy Remaining Elements

After the main merge loop, always copy the remaining elements from either subarray.

### Mistake 5: Incorrect Temporary Array Sizes

Correct:

```java
int n1 = mid - left + 1;

int n2 = right - mid;
```

The `+1` is required because the left range includes both `left` and `mid`.

### Mistake 6: Using the Wrong Comparison for Stability

Prefer:

```java
if (leftArray[i] <= rightArray[j])
```

This selects the left element first when values are equal.

## 17. Important Merge Sort Interview Questions

### Beginner Level

1. What is Merge Sort?
2. What is Divide and Conquer?
3. How does Merge Sort work?
4. What is the base case of Merge Sort?
5. Why do we divide an array into two halves?
6. What is the purpose of the merge method?

### Intermediate Level

1. Explain Merge Sort with a dry run.
2. What is the time complexity of Merge Sort?
3. What is its space complexity?
4. Is Merge Sort stable?
5. Is Merge Sort an in-place algorithm?
6. Why is Merge Sort better than Bubble Sort for large arrays?
7. Why do we use two temporary arrays?

### Advanced Level

1. Explain the recurrence relation of Merge Sort.
2. How does Merge Sort work on linked lists?
3. How can Merge Sort count inversions?
4. Can Merge Sort be implemented iteratively?
5. Why does Merge Sort have `O(n log n)` worst-case time complexity?
6. How does external Merge Sort work?

## 18. Practice Problems

### Easy

1. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
2. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

### Medium

1. [Sort an Array](https://leetcode.com/problems/sort-an-array/)
2. [Sort List](https://leetcode.com/problems/sort-list/)
3. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

### Hard

1. [Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)
2. [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/)

## 19. Quick Revision

| Concept | Remember |
|---|---|
| Merge Sort | Divide and Conquer |
| Divide | Split array into two halves |
| Conquer | Recursively sort both halves |
| Combine | Merge sorted halves |
| Base case | `left >= right` |
| Middle | `left + (right - left) / 2` |
| Best time | O(n log n) |
| Average time | O(n log n) |
| Worst time | O(n log n) |
| Auxiliary space | O(n) |
| Stable | Yes |
| In-place | No, standard array implementation |

### Merge Sort Template

```java
static void mergeSort(int[] arr, int left, int right) {

    if (left >= right) {
        return;
    }

    int mid = left + (right - left) / 2;

    mergeSort(arr, left, mid);

    mergeSort(arr, mid + 1, right);

    merge(arr, left, mid, right);
}
```

### Merge Template

```java
static void merge(
        int[] arr,
        int left,
        int mid,
        int right) {

    int[] temp = new int[right - left + 1];

    int i = left;
    int j = mid + 1;
    int k = 0;

    while (i <= mid && j <= right) {

        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }

    while (i <= mid) {
        temp[k++] = arr[i++];
    }

    while (j <= right) {
        temp[k++] = arr[j++];
    }

    for (int x = 0; x < temp.length; x++) {
        arr[left + x] = temp[x];
    }
}
```

## 20. Learning Checklist

- [ ] Understand Divide and Conquer.
- [ ] Understand how Merge Sort divides an array.
- [ ] Understand the recursion tree.
- [ ] Implement Merge Sort in Java.
- [ ] Understand the merge method.
- [ ] Perform a manual dry run.
- [ ] Implement descending-order Merge Sort.
- [ ] Understand the recurrence relation.
- [ ] Understand best, average, and worst-case complexity.
- [ ] Understand why Merge Sort requires extra memory.
- [ ] Understand why Merge Sort is stable.
- [ ] Practice merging two sorted arrays.
- [ ] Solve at least three Merge Sort-related problems.

---

## Conclusion

Merge Sort is an efficient sorting algorithm based on Divide and Conquer.

The key idea is:

**Divide the array, sort the smaller parts, and merge them into one sorted array.**

Remember these important points:

1. Merge Sort always has `O(n log n)` time complexity.
2. It requires `O(n)` auxiliary space for standard array sorting.
3. It is stable when equal elements are merged in the correct order.
4. It is useful for sorting large datasets and linked lists.

The next sorting algorithm in this repository is **Quick Sort**, which also uses Divide and Conquer but partitions the array around a pivot.