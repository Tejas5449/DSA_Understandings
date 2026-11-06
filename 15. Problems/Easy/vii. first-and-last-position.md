# Find First and Last Position of Element in Sorted Array

**Difficulty:** Medium  
**Topics:** Array, Binary Search  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given target value.

If the target is not found in the array, return `[-1, -1]`.

You must write an algorithm with O(log n) time complexity.

### Example 1

**Input:**
```java
nums = [5, 7, 7, 8, 8, 10]
target = 8
```

**Output:**
```java
[3, 4]
```

**Explanation:**

The target `8` appears at indices `3` and `4`.

### Example 2

**Input:**
```java
nums = [5, 7, 7, 8, 8, 10]
target = 6
```

**Output:**
```java
[-1, -1]
```

**Explanation:**

The target `6` does not exist.

### Example 3

**Input:**
```java
nums = []
target = 0
```

**Output:**
```java
[-1, -1]
```

---

## 2. Understanding the Problem

In normal Binary Search, when we find the target, we immediately return its index.

But here, the target can appear multiple times.

For example:

```text
Index:  0  1  2  3  4  5
Array: [5, 7, 7, 8, 8, 10]
```

For target `8`:

- First occurrence = index `3`
- Last occurrence = index `4`

We need to find both positions.

**Important observation:** Since the array is sorted, all occurrences of a target appear next to each other.

We can solve this using two separate Binary Searches.

---

## 3. Approach 1: Linear Search

### Concept

Traverse the array and find:

- The first index where the target appears.
- The last index where the target appears.

### Java Code

```java
public class FirstLastLinearSearch {

    public static int[] searchRange(int[] nums, int target) {

        int first = -1;
        int last = -1;

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] == target) {

                if (first == -1) {
                    first = i;
                }

                last = i;
            }
        }

        return new int[]{first, last};
    }

    public static void main(String[] args) {

        int[] nums = {5, 7, 7, 8, 8, 10};

        int target = 8;

        int[] result = searchRange(nums, target);

        System.out.println(
            result[0] + ", " + result[1]
        );
    }
}
```

### Dry Run

Input:

```java
nums = [5, 7, 7, 8, 8, 10]
target = 8
```

| Index | Element | First | Last |
|---|---:|---:|---:|
| 0 | 5 | -1 | -1 |
| 1 | 7 | -1 | -1 |
| 2 | 7 | -1 | -1 |
| 3 | 8 | 3 | 3 |
| 4 | 8 | 3 | 4 |
| 5 | 10 | 3 | 4 |

Output:

```java
[3, 4]
```

### Complexity Analysis

- Time Complexity: O(n)
- Space Complexity: O(1), excluding the returned array.

This approach works, but it does not satisfy the required O(log n) time complexity.

---

## 4. Approach 2: Two Binary Searches

### Concept

We will perform Binary Search twice.

**First Binary Search:**

Find the first occurrence of the target.

When we find the target, do not return immediately. Store its index and continue searching the left half.

**Second Binary Search:**

Find the last occurrence of the target.

When we find the target, store its index and continue searching the right half.

### First Occurrence Logic

When:

```java
nums[mid] == target
```

Store the current index:

```java
first = mid;
```

Continue searching left:

```java
right = mid - 1;
```

### Last Occurrence Logic

When:

```java
nums[mid] == target
```

Store the current index:

```java
last = mid;
```

Continue searching right:

```java
left = mid + 1;
```

---

## 5. Java Code – Optimized Solution

```java
public class FirstLastBinarySearch {

    public static int[] searchRange(int[] nums, int target) {

        int first = findFirst(nums, target);

        int last = findLast(nums, target);

        return new int[]{first, last};
    }

    private static int findFirst(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        int first = -1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {

                first = mid;

                right = mid - 1;

            } else if (nums[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return first;
    }

    private static int findLast(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        int last = -1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {

                last = mid;

                left = mid + 1;

            } else if (nums[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return last;
    }

    public static void main(String[] args) {

        int[] nums = {5, 7, 7, 8, 8, 10};

        int target = 8;

        int[] result = searchRange(nums, target);

        System.out.println(
            "[" + result[0] + ", " + result[1] + "]"
        );
    }
}
```

**Output:**

```text
[3, 4]
```

---

## 6. Detailed Dry Run – First Occurrence

Input:

```java
nums = [5, 7, 7, 8, 8, 10]
target = 8
```

Initial values:

```java
left = 0;
right = 5;
first = -1;
```

| Iteration | Left | Right | Mid | nums[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 5 | 2 | 7 | Search right |
| 2 | 3 | 5 | 4 | 8 | Store 4, search left |
| 3 | 3 | 3 | 3 | 8 | Store 3, search left |

At the end:

```java
first = 3;
```

The next search range is empty, so the loop terminates.

**First occurrence = 3**

---

## 7. Detailed Dry Run – Last Occurrence

Initial values:

```java
left = 0;
right = 5;
last = -1;
```

| Iteration | Left | Right | Mid | nums[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 5 | 2 | 7 | Search right |
| 2 | 3 | 5 | 4 | 8 | Store 4, search right |
| 3 | 5 | 5 | 5 | 10 | Search left |

At the end:

```java
last = 4;
```

**Last occurrence = 4**

Final result:

```java
[3, 4]
```

---

## 8. Understanding the Difference Between Both Searches

| Condition | First Occurrence | Last Occurrence |
|---|---|---|
| Target found | Store index | Store index |
| Next action | `right = mid - 1` | `left = mid + 1` |
| Search direction | Left | Right |
| Result | Smallest matching index | Largest matching index |

This is the most important concept in this problem.

---

## 9. Approach 3: Using One Helper Method

We can reduce duplicate code by creating one reusable Binary Search method.

Instead of writing two separate methods, pass a boolean indicating whether we want the first or last occurrence.

### Java Code

```java
public class FirstLastHelperSearch {

    public static int[] searchRange(int[] nums, int target) {

        return new int[]{
            findOccurrence(nums, target, true),
            findOccurrence(nums, target, false)
        };
    }

    private static int findOccurrence(
            int[] nums,
            int target,
            boolean findFirst) {

        int left = 0;
        int right = nums.length - 1;

        int answer = -1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {

                answer = mid;

                if (findFirst) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }

            } else if (nums[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] nums = {5, 7, 7, 8, 8, 10};

        int[] result = searchRange(nums, 8);

        System.out.println(
            "[" + result[0] + ", " + result[1] + "]"
        );
    }
}
```

This version is useful when you want to avoid repeating Binary Search logic.

---

## 10. Complexity Analysis

Let `n` be the number of elements in the array.

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Linear Search | O(n) | O(1) |
| Two Binary Searches | O(log n) | O(1) |
| Helper Method | O(log n) | O(1) |

Why is the optimized solution O(log n)?

We perform two Binary Searches:

- First occurrence: O(log n)
- Last occurrence: O(log n)

Therefore:

\[
O(\log n) + O(\log n) = O(\log n)
\]

The returned array has two elements and requires constant space.

---

## 11. Edge Cases

| Input | Target | Expected Output |
|---|---:|---|
| `[5, 7, 7, 8, 8, 10]` | 8 | `[3, 4]` |
| `[5, 7, 7, 8, 8, 10]` | 6 | `[-1, -1]` |
| `[1, 1, 1, 1]` | 1 | `[0, 3]` |
| `[1, 2, 3, 4]` | 4 | `[3, 3]` |
| `[1, 2, 3, 4]` | 1 | `[0, 0]` |
| `[1]` | 1 | `[0, 0]` |
| `[]` | 1 | `[-1, -1]` |
| `[2, 2, 2, 2, 2]` | 2 | `[0, 4]` |

---

## 12. Common Interview Mistakes

**Mistake 1: Returning immediately after finding the target**

Incorrect:

```java
if (nums[mid] == target) {
    return mid;
}
```

This may return any matching index, not necessarily the first or last.

Correct:

```java
answer = mid;
right = mid - 1;
```

For the first occurrence, continue searching left.

**Mistake 2: Searching in the wrong direction**

- First occurrence: Move left.
- Last occurrence: Move right.

**Mistake 3: Forgetting to initialize the answer**

Always initialize:

```java
int answer = -1;
```

If the target does not exist, `-1` is returned.

**Mistake 4: Using linear search**

The problem specifically requires O(log n), so use Binary Search.

---

## 13. Interview Questions

**Q1. Why do we need two Binary Searches?**

One search finds the first occurrence, and another finds the last occurrence.

**Q2. Why don't we return immediately when the target is found?**

Because there may be more occurrences on either side.

**Q3. What is the time complexity?**

O(log n), because we perform two Binary Searches.

**Q4. What is the difference between normal Binary Search and this problem?**

Normal Binary Search can return any matching index. This problem requires the exact first and last positions.

**Q5. Can we solve this using one Binary Search?**

A single carefully designed search can find one boundary, but finding both boundaries still requires searching for both positions. The standard solution uses two searches.

**Q6. What happens if the target does not exist?**

Both methods return `-1`, so the final result is `[-1, -1]`.

---

## 14. Practice Tasks

Try implementing these variations:

- [ ] Find the first occurrence of a target.
- [ ] Find the last occurrence of a target.
- [ ] Count the total occurrences of a target.
- [ ] Find the lower bound of a target.
- [ ] Find the upper bound of a target.
- [ ] Find the insertion position of a target in a sorted array.

**Hint:** Once you know the first and last positions:

\[
Count = LastIndex - FirstIndex + 1
\]

Only apply this formula when the target exists.

---

## 15. Key Takeaways

- Normal Binary Search may return any matching index.
- To find the first occurrence, continue searching left after finding the target.
- To find the last occurrence, continue searching right after finding the target.
- Two Binary Searches still take O(log n) overall.
- This problem is a foundation for lower-bound and upper-bound problems.

**Most important pattern to remember:**

```java
if (nums[mid] == target) {

    answer = mid;

    if (findFirst) {
        right = mid - 1;
    } else {
        left = mid + 1;
    }
}
```

Mastering this variation will make many advanced Binary Search problems easier.