# Binary Search in Java

## 1. What is Binary Search?

**Binary Search** is an efficient searching algorithm that uses the Divide and Conquer technique to find an element in a sorted array.

Instead of checking every element one by one, Binary Search repeatedly divides the search range into two halves.

It compares the target with the middle element:

- If the middle element equals the target, the element is found.
- If the target is smaller, search the left half.
- If the target is larger, search the right half.

This process continues until the element is found or the search range becomes empty.

### Example

Consider the sorted array:

```java
int[] arr = {10, 20, 30, 40, 50, 60, 70};
```

Target:

```java
int target = 50;
```

Binary Search finds the target by repeatedly reducing the search range.

**Important:** Binary Search requires sorted data when used in its standard form.

---

## 2. How Binary Search Works

Binary Search uses three variables:

| Variable | Meaning |
|---|---|
| `low` | Starting index of the search range |
| `high` | Ending index of the search range |
| `mid` | Middle index of the search range |

Middle index:

```java
int mid = low + (high - low) / 2;
```

We use this formula instead of:

```java
int mid = (low + high) / 2;
```

because `low + high` could overflow for very large integer indexes.

### Search Logic

```text
If arr[mid] == target:
    Element found

If target < arr[mid]:
    Search left half
    high = mid - 1

If target > arr[mid]:
    Search right half
    low = mid + 1
```

---

## 3. Binary Search Dry Run

Input:

```java
int[] arr = {10, 20, 30, 40, 50, 60, 70};

int target = 50;
```

Initial values:

```text
low = 0
high = 6
```

### Iteration 1

Calculate middle:

```text
mid = low + (high - low) / 2
mid = 0 + (6 - 0) / 2
mid = 3
```

Element at index `3`:

```text
arr[3] = 40
```

Comparison:

```text
50 > 40
```

Therefore, search the right half.

Update:

```text
low = mid + 1
low = 4
```

### Iteration 2

Current values:

```text
low = 4
high = 6
```

Calculate middle:

```text
mid = 4 + (6 - 4) / 2
mid = 5
```

Element at index `5`:

```text
arr[5] = 60
```

Comparison:

```text
50 < 60
```

Therefore, search the left half.

Update:

```text
high = mid - 1
high = 4
```

### Iteration 3

Current values:

```text
low = 4
high = 4
```

Calculate middle:

```text
mid = 4 + (4 - 4) / 2
mid = 4
```

Element at index `4`:

```text
arr[4] = 50
```

Target found!

Output:

```text
Element found at index: 4
```

### Dry Run Summary

| Iteration | Low | High | Mid | arr[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 6 | 3 | 40 | Search right |
| 2 | 4 | 6 | 5 | 60 | Search left |
| 3 | 4 | 4 | 4 | 50 | Found |

---

## 4. Iterative Binary Search in Java

Iterative Binary Search uses a `while` loop.

### Java Implementation

```java
public class BinarySearch {

    public static int binarySearch(int[] arr, int target) {

        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                return mid;
            }

            if (target < arr[mid]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50, 60, 70};

        int target = 50;

        int result = binarySearch(arr, target);

        if (result != -1) {
            System.out.println("Element found at index: " + result);
        } else {
            System.out.println("Element not found");
        }
    }
}
```

Output:

```text
Element found at index: 4
```

### Understanding the Code

**1. Initialize the search range**

```java
int low = 0;
int high = arr.length - 1;
```

Initially, the entire array is the search range.

**2. Continue while the search range is valid**

```java
while (low <= high)
```

The condition includes `low == high` because a single remaining element must still be checked.

**3. Calculate the middle index**

```java
int mid = low + (high - low) / 2;
```

**4. Compare the middle element**

```java
if (arr[mid] == target) {
    return mid;
}
```

**5. Search the left half**

```java
if (target < arr[mid]) {
    high = mid - 1;
}
```

**6. Search the right half**

```java
else {
    low = mid + 1;
}
```

**7. Target not found**

```java
return -1;
```

---

## 5. Binary Search When the Element Is Not Found

Consider:

```java
int[] arr = {10, 20, 30, 40, 50, 60, 70};

int target = 45;
```

Search process:

| Iteration | Low | High | Mid | arr[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 6 | 3 | 40 | Search right |
| 2 | 4 | 6 | 5 | 60 | Search left |
| 3 | 4 | 4 | 4 | 50 | Search left |

After the third iteration:

```text
low = 4
high = 3
```

Now:

```text
low > high
```

The search range is empty, so the loop stops.

Output:

```text
Element not found
```

---

## 6. Recursive Binary Search in Java

Binary Search can also be implemented using recursion.

Instead of using a loop, the method calls itself with a smaller search range.

### Java Implementation

```java
public class RecursiveBinarySearch {

    public static int binarySearch(
            int[] arr,
            int low,
            int high,
            int target) {

        // Base condition: element not found
        if (low > high) {
            return -1;
        }

        int mid = low + (high - low) / 2;

        // Target found
        if (arr[mid] == target) {
            return mid;
        }

        // Search left half
        if (target < arr[mid]) {
            return binarySearch(
                    arr, low, mid - 1, target
            );
        }

        // Search right half
        return binarySearch(
                arr, mid + 1, high, target
        );
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50, 60, 70};

        int target = 50;

        int result = binarySearch(
                arr, 0, arr.length - 1, target
        );

        System.out.println("Result: " + result);
    }
}
```

Output:

```text
Result: 4
```

### Understanding Recursive Binary Search

There are two important parts:

**Base condition**

```java
if (low > high) {
    return -1;
}
```

Stops recursion when no elements remain.

**Recursive calls**

Left half:

```java
return binarySearch(arr, low, mid - 1, target);
```

Right half:

```java
return binarySearch(arr, mid + 1, high, target);
```

Each recursive call reduces the search range by approximately half.

### Recursion Flow

For target `50`:

```text
binarySearch(0, 6)
       |
       mid = 3
       |
       50 > 40
       |
binarySearch(4, 6)
       |
       mid = 5
       |
       50 < 60
       |
binarySearch(4, 4)
       |
       mid = 4
       |
       50 == 50
       |
      Found
```

---

## 7. Iterative vs Recursive Binary Search

| Feature | Iterative | Recursive |
|---|---|---|
| Technique | Loop | Recursion |
| Time Complexity | O(log n) | O(log n) |
| Auxiliary Space | O(1) | O(log n) |
| Function calls | One method call | Multiple recursive calls |
| Stack usage | Constant | Recursive call stack |
| Performance | Usually more memory-efficient | Has function-call overhead |

**Recommendation:** Prefer iterative Binary Search when you need a straightforward, memory-efficient implementation.

---

## 8. Binary Search Time Complexity

Binary Search divides the search range into half after every iteration.

Suppose the array contains `n` elements.

After each iteration:

```text
n
n / 2
n / 4
n / 8
n / 16
...
```

The number of divisions required is approximately:

\[
\log_2(n)
\]

Therefore, the time complexity is:

\[
O(\log n)
\]

### Best Case: O(1)

The target is found at the middle index in the first iteration.

### Average Case: O(log n)

The search range is repeatedly divided until the target is found.

### Worst Case: O(log n)

The target is found near the end of the search process or does not exist.

### Complexity Summary

| Case | Time Complexity |
|---|---|
| Best Case | O(1) |
| Average Case | O(log n) |
| Worst Case | O(log n) |

### Space Complexity

| Implementation | Auxiliary Space |
|---|---|
| Iterative | O(1) |
| Recursive | O(log n) |

---

## 9. Find First Occurrence of an Element

Consider an array with duplicate elements:

```java
int[] arr = {10, 20, 20, 20, 30, 40, 50};

int target = 20;
```

Normal Binary Search may return any matching index.

But sometimes we need the first occurrence.

Expected output:

```text
First occurrence: 1
```

### Approach

When the target is found:

1. Store the current index as the answer.
2. Continue searching in the left half.
3. Return the earliest matching index.

### Java Implementation

```java
public static int firstOccurrence(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length - 1;

    int answer = -1;

    while (low <= high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] == target) {

            answer = mid;

            // Search for an earlier occurrence
            high = mid - 1;

        } else if (target < arr[mid]) {

            high = mid - 1;

        } else {

            low = mid + 1;
        }
    }

    return answer;
}
```

### Dry Run

Input:

```text
[10, 20, 20, 20, 30, 40, 50]
```

Target:

```text
20
```

| Low | High | Mid | Value | Action |
|---:|---:|---:|---:|---|
| 0 | 6 | 3 | 20 | Store 3, search left |
| 0 | 2 | 1 | 20 | Store 1, search left |
| 0 | 0 | 0 | 10 | Search right |

The loop ends.

Answer:

```text
1
```

---

## 10. Find Last Occurrence of an Element

Now consider:

```java
int[] arr = {10, 20, 20, 20, 30, 40, 50};

int target = 20;
```

Expected output:

```text
Last occurrence: 3
```

### Approach

When the target is found:

1. Store the current index.
2. Continue searching in the right half.
3. Return the last matching index.

### Java Implementation

```java
public static int lastOccurrence(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length - 1;

    int answer = -1;

    while (low <= high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] == target) {

            answer = mid;

            // Search for a later occurrence
            low = mid + 1;

        } else if (target < arr[mid]) {

            high = mid - 1;

        } else {

            low = mid + 1;
        }
    }

    return answer;
}
```

### Dry Run

| Low | High | Mid | Value | Action |
|---:|---:|---:|---:|---|
| 0 | 6 | 3 | 20 | Store 3, search right |
| 4 | 6 | 5 | 40 | Search left |
| 4 | 4 | 4 | 30 | Search left |

Answer:

```text
3
```

---

## 11. Count Occurrences of an Element

We can use the first and last occurrence to calculate how many times an element appears.

Formula:

\[
\text{Count} = \text{Last Index} - \text{First Index} + 1
\]

### Java Implementation

```java
public static int countOccurrences(
        int[] arr, int target) {

    int first = firstOccurrence(arr, target);

    if (first == -1) {
        return 0;
    }

    int last = lastOccurrence(arr, target);

    return last - first + 1;
}
```

Example:

```java
int[] arr = {10, 20, 20, 20, 30, 40, 50};

System.out.println(countOccurrences(arr, 20));
```

Output:

```text
3
```

**Important:** This formula works because equal elements are grouped together in a sorted array.

---

## 12. Lower Bound

Lower Bound is a common Binary Search concept.

**Lower Bound returns the index of the first element that is greater than or equal to the target.**

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int target = 30;
```

Output:

```text
2
```

Because `arr[2] = 30`, which is the first element greater than or equal to `30`.

### Another Example

```java
int[] arr = {10, 20, 30, 40, 50};

int target = 35;
```

The first element greater than or equal to `35` is `40`.

Output:

```text
3
```

### Java Implementation

```java
public static int lowerBound(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length;

    while (low < high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] >= target) {

            high = mid;

        } else {

            low = mid + 1;
        }
    }

    return low;
}
```

### Important Points

- Lower Bound searches for the first element `>= target`.
- It may return `arr.length` if no element satisfies the condition.
- It works with duplicate values.
- The search range uses `high = arr.length` as an exclusive boundary.

Example:

```text
Array:  [10, 20, 30, 40, 50]
Target: 60

Lower Bound = 5
```

Index `5` means no element is greater than or equal to `60`.

---

## 13. Upper Bound

**Upper Bound returns the index of the first element that is strictly greater than the target.**

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int target = 30;
```

Output:

```text
3
```

Because `40` is the first element greater than `30`.

### Java Implementation

```java
public static int upperBound(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length;

    while (low < high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] > target) {

            high = mid;

        } else {

            low = mid + 1;
        }
    }

    return low;
}
```

### Lower Bound vs Upper Bound

| Concept | Condition | Meaning |
|---|---|---|
| Lower Bound | `arr[mid] >= target` | First element greater than or equal to target |
| Upper Bound | `arr[mid] > target` | First element strictly greater than target |

Example:

```text
Array: [10, 20, 20, 20, 30, 40]

Target: 20

Lower Bound = 1
Upper Bound = 4
```

The number of occurrences can also be calculated using:

```java
int count = upperBound(arr, target)
          - lowerBound(arr, target);
```

---

## 14. Search Insert Position

This is a common interview problem.

Given a sorted array, find the index where the target exists.

If the target does not exist, return the index where it should be inserted to maintain sorted order.

Example 1:

```text
Array: [10, 20, 30, 40, 50]
Target: 30

Output: 2
```

Example 2:

```text
Array: [10, 20, 30, 40, 50]
Target: 35

Output: 3
```

### Java Implementation

```java
public static int searchInsert(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length - 1;

    while (low <= high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] == target) {
            return mid;
        }

        if (target < arr[mid]) {

            high = mid - 1;

        } else {

            low = mid + 1;
        }
    }

    return low;
}
```

Why return `low`?

When the search finishes, `low` points to the position where the target should be inserted.

This is also equivalent to finding the Lower Bound.

---

## 15. Binary Search on a Descending Sorted Array

Standard Binary Search assumes ascending order.

For a descending array, the comparison logic must be reversed.

Example:

```java
int[] arr = {90, 80, 70, 60, 50, 40, 30};

int target = 60;
```

### Java Implementation

```java
public static int binarySearchDescending(
        int[] arr, int target) {

    int low = 0;
    int high = arr.length - 1;

    while (low <= high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] == target) {
            return mid;
        }

        // In descending order, smaller values are on the right
        if (target > arr[mid]) {

            high = mid - 1;

        } else {

            low = mid + 1;
        }
    }

    return -1;
}
```

Output:

```text
Element found at index: 3
```

---

## 16. Binary Search Using Java Arrays

Java provides a built-in method:

```java
Arrays.binarySearch()
```

Example:

```java
import java.util.Arrays;

public class BuiltInBinarySearch {

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50};

        int result = Arrays.binarySearch(arr, 30);

        System.out.println(result);
    }
}
```

Output:

```text
2
```

If the target is not found, Java returns a negative value.

The return value is:

```text
-(insertion point) - 1
```

For example:

```java
int[] arr = {10, 20, 30, 40, 50};

int result = Arrays.binarySearch(arr, 35);

System.out.println(result);
```

The insertion point is `3`, so the returned value is:

```text
-4
```

**Important:**

- The array must be sorted for meaningful Binary Search results.
- If duplicates exist, `Arrays.binarySearch()` does not guarantee that the first or last occurrence will be returned.
- Use your own implementation when a specific occurrence is required.

---

## 17. Common Mistakes in Binary Search

### Mistake 1: Using Binary Search on an Unsorted Array

Binary Search relies on the ordering of elements.

Incorrect:

```java
int[] arr = {40, 10, 50, 20, 30};
```

Correct:

```java
int[] arr = {10, 20, 30, 40, 50};
```

Sort the array first if Binary Search is required.

### Mistake 2: Incorrect While Loop Condition

Incorrect:

```java
while (low < high)
```

For standard element-search Binary Search, this can skip checking the final remaining element.

Correct:

```java
while (low <= high)
```

Some variations, such as Lower Bound, intentionally use `low < high`.

### Mistake 3: Incorrect Middle Index

Less safe:

```java
int mid = (low + high) / 2;
```

Preferred:

```java
int mid = low + (high - low) / 2;
```

### Mistake 4: Forgetting to Update the Search Range

Incorrect:

```java
if (target < arr[mid]) {
    high = mid;
}
```

This can cause an infinite loop when `mid == low`.

Correct:

```java
if (target < arr[mid]) {
    high = mid - 1;
}
```

### Mistake 5: Searching the Wrong Half

For ascending arrays:

```java
if (target < arr[mid]) {
    high = mid - 1;
} else {
    low = mid + 1;
}
```

Remember:

- Smaller target → left half.
- Larger target → right half.

### Mistake 6: Confusing Lower Bound and Upper Bound

- Lower Bound finds the first element `>= target`.
- Upper Bound finds the first element `> target`.

These are different when duplicates exist.

---

## 18. Frequently Asked Interview Questions

### Q1. What is Binary Search?

Binary Search is an efficient searching algorithm that repeatedly divides a sorted search range into two halves.

### Q2. What is the time complexity of Binary Search?

O(log n) in the average and worst cases.

### Q3. What is the best-case time complexity?

O(1), when the target is found at the middle index in the first comparison.

### Q4. Why does Binary Search require sorted data?

Because it decides which half to discard based on the ordering of elements.

Without sorted data, it cannot safely eliminate half of the search range.

### Q5. What is the space complexity of iterative Binary Search?

O(1), because it uses only a fixed number of variables.

### Q6. What is the space complexity of recursive Binary Search?

O(log n), because recursive calls use the call stack.

### Q7. What is the difference between Linear Search and Binary Search?

Linear Search checks elements one by one and takes O(n) time in the worst case.

Binary Search repeatedly divides a sorted search range in half and takes O(log n) time.

### Q8. Can Binary Search work on descending arrays?

Yes, but the comparison logic must be reversed.

### Q9. Can Binary Search find duplicate elements?

Yes. Modified Binary Search can find the first occurrence, last occurrence, and count of duplicate elements.

### Q10. What is Lower Bound?

The index of the first element greater than or equal to the target.

### Q11. What is Upper Bound?

The index of the first element strictly greater than the target.

### Q12. Why do we use `low + (high - low) / 2`?

To avoid possible integer overflow when calculating the middle index.

---

## 19. Practice Problems

### Beginner

1. Implement iterative Binary Search.
2. Implement recursive Binary Search.
3. Search for an element in a sorted array.
4. Find the first occurrence of an element.
5. Find the last occurrence of an element.
6. Count occurrences of an element.
7. Find the insertion position of a target.
8. Implement Lower Bound.
9. Implement Upper Bound.

### Intermediate

1. Find the first and last position of an element in a sorted array.
2. Search in a rotated sorted array.
3. Find the minimum element in a rotated sorted array.
4. Find the peak element.
5. Find the single element in a sorted array.
6. Find the square root of an integer using Binary Search.
7. Find the search range of a target.
8. Search in a 2D matrix.

### Advanced

1. Median of Two Sorted Arrays.
2. Kth Smallest Element in a Sorted Matrix.
3. Split Array Largest Sum.
4. Capacity to Ship Packages Within D Days.
5. Aggressive Cows.
6. Allocate Minimum Number of Pages.

### Recommended LeetCode Problems

| Problem | Difficulty |
|---|---|
| [Binary Search](https://leetcode.com/problems/binary-search/) | Easy |
| [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | Easy |
| [First Bad Version](https://leetcode.com/problems/first-bad-version/) | Easy |
| [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | Medium |
| [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | Medium |
| [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | Medium |
| [Find Peak Element](https://leetcode.com/problems/find-peak-element/) | Medium |
| [Sqrt(x)](https://leetcode.com/problems/sqrtx/) | Easy |

---

## 20. Quick Revision

- Binary Search uses the Divide and Conquer technique.
- It requires sorted data in its standard form.
- `low` represents the starting index.
- `high` represents the ending index.
- `mid` represents the middle index.
- Middle formula: `low + (high - low) / 2`.
- Smaller target → search left half.
- Larger target → search right half.
- Iterative Binary Search uses O(1) auxiliary space.
- Recursive Binary Search uses O(log n) auxiliary stack space.
- Best-case time: O(1).
- Average-case time: O(log n).
- Worst-case time: O(log n).
- Lower Bound finds the first element `>= target`.
- Upper Bound finds the first element `> target`.

---

## 21. Self-Assessment Checklist

Before moving to the next section, make sure you can:

- [ ] Explain Binary Search.
- [ ] Explain why sorted data is required.
- [ ] Implement iterative Binary Search in Java.
- [ ] Implement recursive Binary Search in Java.
- [ ] Perform a dry run manually.
- [ ] Find the first occurrence of an element.
- [ ] Find the last occurrence of an element.
- [ ] Count duplicate elements.
- [ ] Implement Lower Bound and Upper Bound.
- [ ] Solve Search Insert Position.
- [ ] Explain time and space complexity.
- [ ] Solve at least three Binary Search interview problems.

---

## 22. What's Next?

You have completed the **Searching** section:

- Linear Search
- Binary Search

Next, we will begin the **Dynamic Programming** section.

First topic: **Dynamic Programming Basics**

File: `14-dynamic-programming/dp-basics.md`

We will cover:

- What Dynamic Programming is.
- Overlapping subproblems.
- Optimal substructure.
- Recursion vs Memoization vs Tabulation.
- Fibonacci using different approaches.
- How to identify Dynamic Programming problems.
- Common interview patterns.