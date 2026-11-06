# Merge Sorted Array

**Difficulty:** Easy  
**LeetCode:** [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)  
**Topics:** Arrays, Two Pointers, Sorting, In-place Algorithms

---

## 1. Problem Statement

You are given two integer arrays, `nums1` and `nums2`, sorted in non-decreasing order.

You are also given two integers:

- `m`: Number of valid elements in `nums1`.
- `n`: Number of elements in `nums2`.

Merge `nums2` into `nums1` as one sorted array.

The final sorted array should be stored inside `nums1`.

**Important:**
- `nums1` has a length of `m + n`.
- The first `m` positions contain valid elements.
- The remaining `n` positions are initialized with zero and should be ignored.
- `nums2` contains exactly `n` elements.

You must modify `nums1` directly instead of returning a new array.

### Example 1

**Input:**

```java
nums1 = [1, 2, 3, 0, 0, 0]
m = 3

nums2 = [2, 5, 6]
n = 3
```

**Output:**

```java
[1, 2, 2, 3, 5, 6]
```

**Explanation:**

The first three elements of `nums1` and all elements of `nums2` are merged in sorted order.

### Example 2

**Input:**

```java
nums1 = [1]
m = 1

nums2 = []
n = 0
```

**Output:**

```java
[1]
```

### Example 3

**Input:**

```java
nums1 = [0]
m = 0

nums2 = [1]
n = 1
```

**Output:**

```java
[1]
```

---

## 2. Understanding the Problem

We have two already sorted arrays.

Our task is to combine them into one sorted array.

For example:

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]

Valid elements in nums1 = [1, 2, 3]

Final array = [1, 2, 2, 3, 5, 6]
```

Notice that `nums1` already has enough space to store all elements.

We should use this available space instead of creating another array.

---

## 3. Approach 1: Using an Extra Array

The simplest approach is to create a new array and merge both sorted arrays into it.

### Logic

1. Create a new array of size `m + n`.
2. Use two pointers to compare elements from both arrays.
3. Copy the smaller element into the new array.
4. Copy any remaining elements.
5. Copy the merged result back into `nums1`.

### Java Code

```java
import java.util.Arrays;

public class MergeSortedArrayExtraSpace {

    public static void merge(int[] nums1, int m, int[] nums2, int n) {

        int[] result = new int[m + n];

        int i = 0;
        int j = 0;
        int k = 0;

        // Compare elements from both arrays
        while (i < m && j < n) {

            if (nums1[i] <= nums2[j]) {
                result[k++] = nums1[i++];
            } else {
                result[k++] = nums2[j++];
            }
        }

        // Copy remaining elements from nums1
        while (i < m) {
            result[k++] = nums1[i++];
        }

        // Copy remaining elements from nums2
        while (j < n) {
            result[k++] = nums2[j++];
        }

        // Copy result back into nums1
        for (int x = 0; x < result.length; x++) {
            nums1[x] = result[x];
        }
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 3, 0, 0, 0};
        int[] nums2 = {2, 5, 6};

        merge(nums1, 3, nums2, 3);

        System.out.println(Arrays.toString(nums1));
    }
}
```

**Output:**

```text
[1, 2, 2, 3, 5, 6]
```

### Complexity Analysis

- Time Complexity: O(m + n).
- Space Complexity: O(m + n), because we create another array.

This approach works, but it does not satisfy the optimal O(1) extra-space requirement.

---

## 4. Approach 2: Sorting After Copying

Another simple approach is to copy all valid elements of `nums2` into the empty positions of `nums1`, then sort `nums1`.

### Java Code

```java
import java.util.Arrays;

public class MergeSortedArraySorting {

    public static void merge(int[] nums1, int m, int[] nums2, int n) {

        for (int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }

        Arrays.sort(nums1);
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 3, 0, 0, 0};
        int[] nums2 = {2, 5, 6};

        merge(nums1, 3, nums2, 3);

        System.out.println(Arrays.toString(nums1));
    }
}
```

**Output:**

```text
[1, 2, 2, 3, 5, 6]
```

### Complexity Analysis

- Copying elements: O(n).
- Sorting: O((m + n) log(m + n)).
- Extra space: Depends on the sorting implementation.

This solution is easy to write, but we can do better by using the fact that both arrays are already sorted.

---

## 5. Approach 3: Optimal Solution Using Three Pointers

This is the recommended interview solution.

### Main Idea

Instead of merging from the beginning, we merge from the end.

Why?

Because `nums1` already contains empty positions at the end.

If we start from the beginning, we may overwrite elements in `nums1` that we still need to compare.

By starting from the end, we place the largest element in the last available position.

### Three Pointers

We maintain three pointers:

```java
int i = m - 1;
int j = n - 1;
int k = m + n - 1;
```

- `i`: Points to the last valid element of `nums1`.
- `j`: Points to the last element of `nums2`.
- `k`: Points to the last available position in `nums1`.

### Algorithm

1. Compare `nums1[i]` and `nums2[j]`.
2. Place the larger element at `nums1[k]`.
3. Move the corresponding pointer backward.
4. Move `k` backward.
5. Repeat until all elements of `nums2` have been copied.

### Java Code

```java
import java.util.Arrays;

public class MergeSortedArray {

    public static void merge(int[] nums1, int m, int[] nums2, int n) {

        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while (j >= 0) {

            if (i >= 0 && nums1[i] > nums2[j]) {

                nums1[k] = nums1[i];
                i--;

            } else {

                nums1[k] = nums2[j];
                j--;
            }

            k--;
        }
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 3, 0, 0, 0};
        int[] nums2 = {2, 5, 6};

        merge(nums1, 3, nums2, 3);

        System.out.println(Arrays.toString(nums1));
    }
}
```

**Output:**

```text
[1, 2, 2, 3, 5, 6]
```

---

## 6. Detailed Dry Run

Let's understand the optimal approach step by step.

**Input:**

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]

m = 3
n = 3
```

### Initial Pointer Positions

```text
i = m - 1 = 2
j = n - 1 = 2
k = m + n - 1 = 5
```

Initial arrays:

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]
```

### Iteration 1

Compare:

```text
nums1[i] = 3
nums2[j] = 6
```

Since `6 > 3`, place `6` at index `5`.

```text
nums1 = [1, 2, 3, 0, 0, 6]
```

Move `j` and `k` backward.

```text
i = 2
j = 1
k = 4
```

### Iteration 2

Compare:

```text
nums1[i] = 3
nums2[j] = 5
```

Since `5 > 3`, place `5` at index `4`.

```text
nums1 = [1, 2, 3, 0, 5, 6]
```

Move `j` and `k` backward.

```text
i = 2
j = 0
k = 3
```

### Iteration 3

Compare:

```text
nums1[i] = 3
nums2[j] = 2
```

Since `3 > 2`, place `3` at index `3`.

```text
nums1 = [1, 2, 3, 3, 5, 6]
```

Move `i` and `k` backward.

```text
i = 1
j = 0
k = 2
```

### Iteration 4

Compare:

```text
nums1[i] = 2
nums2[j] = 2
```

Both values are equal.

Our code uses `>` rather than `>=`, so it places the element from `nums2`.

```text
nums1 = [1, 2, 2, 3, 5, 6]
```

Move `j` and `k` backward.

```text
i = 1
j = -1
k = 1
```

Now `j < 0`, so the loop stops.

### Final Result

```text
nums1 = [1, 2, 2, 3, 5, 6]
```

---

## 7. Dry Run Table

| Iteration | nums1[i] | nums2[j] | Placed Value | Updated nums1 |
|---|---:|---:|---:|---|
| Initial | 3 | 6 | - | `[1,2,3,0,0,0]` |
| 1 | 3 | 6 | 6 | `[1,2,3,0,0,6]` |
| 2 | 3 | 5 | 5 | `[1,2,3,0,5,6]` |
| 3 | 3 | 2 | 3 | `[1,2,3,3,5,6]` |
| 4 | 2 | 2 | 2 | `[1,2,2,3,5,6]` |

Final output:

```text
[1, 2, 2, 3, 5, 6]
```

---

## 8. Why Do We Start From the End?

This is the most important concept in this problem.

Suppose we start merging from the beginning:

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]
```

If we place `2` at index `0`, we overwrite the original `1`.

We would lose data that we still need.

Instead, we start from the last available position.

```text
nums1 = [1, 2, 3, 0, 0, 0]
                         ↑
                         k
```

We place the largest element first.

This ensures that we never overwrite an unprocessed element in `nums1`.

**Remember:** When merging into an array that has empty space at the end, consider using pointers from the end.

---

## 9. Why Does the Loop Check Only `j >= 0`?

Our loop condition is:

```java
while (j >= 0)
```

We do not need to check `i >= 0` in the loop condition.

Why?

There are two possible situations:

**Case 1: `nums1` still has elements**

We compare both arrays and place the larger element.

**Case 2: `nums1` is completely processed**

If `i < 0`, we copy the remaining elements from `nums2` into `nums1`.

The code handles this using:

```java
if (i >= 0 && nums1[i] > nums2[j])
```

If `i < 0`, the condition is false, so the `else` block copies from `nums2`.

If `nums2` is completely processed first, the remaining elements in `nums1` are already in their correct positions. No further work is necessary.

---

## 10. Complexity Analysis

Let:

- `m` = Number of valid elements in `nums1`.
- `n` = Number of elements in `nums2`.

| Complexity | Value | Explanation |
|---|---|---|
| Time | O(m + n) | Each element is processed at most once. |
| Extra Space | O(1) | We use only three pointers. |

This is the optimal solution for the problem.

---

## 11. Edge Cases

| nums1 | m | nums2 | n | Final Result |
|---|---:|---|---:|---|
| `[1,2,3,0,0,0]` | 3 | `[2,5,6]` | 3 | `[1,2,2,3,5,6]` |
| `[1]` | 1 | `[]` | 0 | `[1]` |
| `[0]` | 0 | `[1]` | 1 | `[1]` |
| `[4,5,6,0,0,0]` | 3 | `[1,2,3]` | 3 | `[1,2,3,4,5,6]` |
| `[1,2,3,0,0,0]` | 3 | `[4,5,6]` | 3 | `[1,2,3,4,5,6]` |

---

## 12. Interview Questions

### Q1. Why do we use three pointers?

We need to track:

- The last valid element of `nums1`.
- The last element of `nums2`.
- The last available position in `nums1`.

### Q2. Why do we merge from the end?

Because `nums1` has empty positions at the end. Merging backward prevents us from overwriting unprocessed elements.

### Q3. What is the time complexity?

O(m + n), because each element is processed at most once.

### Q4. What is the space complexity?

O(1), because we do not create another array.

### Q5. Why does the loop continue while `j >= 0`?

If `nums2` still contains elements, they must be placed into `nums1`. If `nums2` is empty, any remaining elements in `nums1` are already correctly positioned.

### Q6. What happens when both elements are equal?

Our implementation uses:

```java
nums1[i] > nums2[j]
```

When the values are equal, it places the element from `nums2` first. The final array remains sorted.

### Q7. Does this algorithm modify the original array?

Yes. The final merged array is stored directly inside `nums1`.

---

## 13. Practice Problems

Try these problems to improve your understanding of two pointers and array manipulation.

1. [Merge Two Sorted Lists – LeetCode 21](https://leetcode.com/problems/merge-two-sorted-lists/)
2. [Remove Duplicates from Sorted Array – LeetCode 26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
3. [Move Zeroes – LeetCode 283](https://leetcode.com/problems/move-zeroes/)
4. [Squares of a Sorted Array – LeetCode 977](https://leetcode.com/problems/squares-of-a-sorted-array/)

---

## 14. Final Summary

We learned three approaches:

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Extra Array | O(m + n) | O(m + n) |
| Copy and Sort | O((m+n) log(m+n)) | Depends on sorting |
| Three Pointers | O(m + n) | O(1) |

### Recommended Interview Solution

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {

    int i = m - 1;
    int j = n - 1;
    int k = m + n - 1;

    while (j >= 0) {

        if (i >= 0 && nums1[i] > nums2[j]) {
            nums1[k] = nums1[i];
            i--;
        } else {
            nums1[k] = nums2[j];
            j--;
        }

        k--;
    }
}
```

**Important takeaway:** When two sorted arrays need to be merged into an array with extra capacity at the end, use three pointers and start from the back.
