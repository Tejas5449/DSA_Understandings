# Binary Search

**Difficulty:** Easy  
**Topics:** Array, Binary Search, Searching, Divide and Conquer  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given a sorted array of integers `nums` and an integer `target`, return the index of the target if it exists in the array.

If the target does not exist, return `-1`.

**Important:** The input array is sorted in ascending order.

### Example 1

**Input:**
```java
nums = [-1, 0, 3, 5, 9, 12]
target = 9
```

**Output:**
```java
4
```

**Explanation:** The target `9` is present at index `4`.

### Example 2

**Input:**
```java
nums = [-1, 0, 3, 5, 9, 12]
target = 2
```

**Output:**
```java
-1
```

**Explanation:** The target `2` is not present.

### Example 3

**Input:**
```java
nums = [5]
target = 5
```

**Output:**
```java
0
```

---

## 2. What Is Binary Search?

Binary Search is an efficient searching algorithm used on sorted data.

Instead of checking every element one by one, it repeatedly divides the search range into two halves.

At every iteration:

1. Find the middle index.
2. Compare the middle element with the target.
3. If both are equal, return the middle index.
4. If the target is smaller, search the left half.
5. If the target is greater, search the right half.

The search space becomes approximately half its previous size after every iteration.

### Visual Example

```text
Array:
[1, 3, 5, 7, 9, 11, 13]

Target = 11

Step 1:
[1, 3, 5, 7, 9, 11, 13]
             ^
           mid = 7

11 > 7, search right half.

Step 2:
[9, 11, 13]
     ^
   mid = 11

Target found!
```

---

## 3. Approach 1: Linear Search

Before understanding Binary Search, let's see the simple approach.

### Concept

Check every element from left to right until the target is found.

### Java Code

```java
public class LinearSearch {

    public static int search(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] == target) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {1, 3, 5, 7, 9, 11};

        int target = 9;

        System.out.println(search(nums, target));
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

Linear Search works even if the array is unsorted, but it can be slow for large arrays.

---

## 4. Approach 2: Binary Search Using Iteration

### Concept

Use three variables:

```java
int left = 0;
int right = nums.length - 1;
int mid;
```

Where:

- `left` represents the starting index of the search range.
- `right` represents the ending index.
- `mid` represents the middle index.

Calculate the middle index:

```java
int mid = left + (right - left) / 2;
```

We use this formula instead of `(left + right) / 2` to avoid possible integer overflow.

### Algorithm

1. Initialize `left = 0`.
2. Initialize `right = nums.length - 1`.
3. Continue while `left <= right`.
4. Calculate the middle index.
5. Compare `nums[mid]` with the target.
6. If equal, return `mid`.
7. If `nums[mid]` is smaller, update `left = mid + 1`.
8. If `nums[mid]` is greater, update `right = mid - 1`.
9. If the loop finishes, return `-1`.

### Java Code

```java
public class BinarySearch {

    public static int search(int[] nums, int target) {

        int left = 0;

        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {

                return mid;

            } else if (nums[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {-1, 0, 3, 5, 9, 12};

        int target = 9;

        System.out.println(search(nums, target));
    }
}
```

---

## 5. Detailed Dry Run

**Input:**

```java
nums = [-1, 0, 3, 5, 9, 12]
target = 9
```

Initial values:

```java
left = 0;
right = 5;
```

### Iteration 1

Calculate middle:

```java
mid = 0 + (5 - 0) / 2;
mid = 2;
```

Current element:

```java
nums[2] = 3;
```

Comparison:

```java
3 < 9
```

The target must be in the right half.

Update:

```java
left = mid + 1;
left = 3;
```

### Iteration 2

Current range:

```text
[5, 9, 12]
```

Indexes:

```text
 3  4   5
```

Calculate middle:

```java
mid = 3 + (5 - 3) / 2;
mid = 4;
```

Current element:

```java
nums[4] = 9;
```

Comparison:

```java
9 == 9
```

Target found.

Return:

```java
4
```

### Dry Run Table

| Iteration | Left | Right | Mid | nums[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 5 | 2 | 3 | Search right |
| 2 | 3 | 5 | 4 | 9 | Target found |

---

## 6. Understanding the Binary Search Conditions

These three conditions are extremely important.

### Condition 1: Target Found

```java
if (nums[mid] == target) {
    return mid;
}
```

The middle element matches the target.

### Condition 2: Search Right Half

```java
else if (nums[mid] < target) {
    left = mid + 1;
}
```

Because the array is sorted, all elements to the left of `mid` are smaller than or equal to `nums[mid]`.

Therefore, the target must be to the right.

### Condition 3: Search Left Half

```java
else {
    right = mid - 1;
}
```

The middle element is greater than the target.

Therefore, the target must be to the left.

---

## 7. Why Do We Use left <= right?

Consider:

```java
while (left <= right)
```

We use `<=` because the search range may contain exactly one element.

For example:

```text
nums = [5]
target = 5

left = 0
right = 0
```

Since `left == right`, we must still check that element.

If we use:

```java
while (left < right)
```

the loop will not execute, and we will incorrectly return `-1`.

---

## 8. Approach 3: Binary Search Using Recursion

Binary Search can also be implemented using recursion.

### Concept

Instead of updating `left` and `right` inside a loop, call the same method with a smaller search range.

### Java Code

```java
public class BinarySearchRecursive {

    public static int search(
            int[] nums,
            int target,
            int left,
            int right) {

        if (left > right) {
            return -1;
        }

        int mid = left + (right - left) / 2;

        if (nums[mid] == target) {
            return mid;
        }

        if (nums[mid] < target) {

            return search(
                nums,
                target,
                mid + 1,
                right
            );

        } else {

            return search(
                nums,
                target,
                left,
                mid - 1
            );
        }
    }

    public static void main(String[] args) {

        int[] nums = {-1, 0, 3, 5, 9, 12};

        int target = 9;

        int result = search(
            nums,
            target,
            0,
            nums.length - 1
        );

        System.out.println(result);
    }
}
```

### Understanding the Base Condition

```java
if (left > right) {
    return -1;
}
```

When `left` becomes greater than `right`, there are no elements left to search.

Therefore, the target does not exist.

### Complexity Analysis

| Complexity | Value |
|---|---|
| Time | O(log n) |
| Auxiliary Space | O(log n) due to recursive call stack |

The iterative implementation uses O(1) auxiliary space.

---

## 9. Binary Search Complexity

Every iteration reduces the search range by approximately half.

For example, if the array contains 16 elements:

```text
16 -> 8 -> 4 -> 2 -> 1
```

Only a small number of iterations are required.

| Array Size | Approximate Maximum Halving Steps |
|---:|---:|
| 8 | 3 |
| 16 | 4 |
| 32 | 5 |
| 64 | 6 |
| 128 | 7 |
| 1024 | 10 |

Therefore:

**Time Complexity:**

\[
O(\log n)
\]

**Space Complexity:**

- Iterative: O(1)
- Recursive: O(log n)

---

## 10. Important Binary Search Rules

1. Binary Search requires sorted data for this standard implementation.
2. Initialize `left` to `0`.
3. Initialize `right` to `nums.length - 1`.
4. Use `left <= right` for an inclusive search range.
5. Use `mid + 1` and `mid - 1` to avoid repeatedly checking the same middle element.
6. Use `left + (right - left) / 2` to calculate the middle safely.
7. Return `-1` if the target does not exist.

---

## 11. Edge Cases

| Input | Target | Expected Output |
|---|---:|---:|
| `[1, 3, 5, 7, 9]` | 5 | 2 |
| `[1, 3, 5, 7, 9]` | 1 | 0 |
| `[1, 3, 5, 7, 9]` | 9 | 4 |
| `[1, 3, 5, 7, 9]` | 4 | -1 |
| `[5]` | 5 | 0 |
| `[5]` | 2 | -1 |
| `[]` | 5 | -1 |
| `[-10, -5, 0, 5, 10]` | -5 | 1 |

---

## 12. Common Interview Mistakes

**Mistake 1: Incorrect loop condition**

```java
while (left < right)
```

This can skip checking the final remaining element in this inclusive-range implementation.

Use:

```java
while (left <= right)
```

**Mistake 2: Incorrect pointer updates**

Incorrect:

```java
left = mid;
right = mid;
```

These updates can cause an infinite loop.

Correct:

```java
left = mid + 1;
right = mid - 1;
```

**Mistake 3: Incorrect middle calculation**

Potentially unsafe:

```java
int mid = (left + right) / 2;
```

Safer:

```java
int mid = left + (right - left) / 2;
```

**Mistake 4: Using Binary Search on an unsorted array**

The standard algorithm depends on sorted order. Without it, the left/right elimination logic is not valid.

---

## 13. Interview Questions

**Q1. What is Binary Search?**

Binary Search is an algorithm that finds an element in sorted data by repeatedly dividing the search range into two halves.

**Q2. What is its time complexity?**

O(log n).

**Q3. Why must the array be sorted?**

Because sorting allows us to determine which half can safely be discarded after comparing the middle element with the target.

**Q4. What is the difference between Linear Search and Binary Search?**

- Linear Search: O(n) time.
- Binary Search: O(log n) time on sorted data.

**Q5. What is the difference between iterative and recursive Binary Search?**

- Iterative uses a loop and O(1) auxiliary space.
- Recursive uses function calls and O(log n) call-stack space.

**Q6. Why do we use `mid + 1` and `mid - 1`?**

To exclude the middle element after confirming that it is not the target.

**Q7. Can Binary Search work on a descending sorted array?**

Yes, but the comparison conditions must be reversed.

**Q8. What happens if the target appears multiple times?**

This standard implementation may return any matching index. To find the first or last occurrence, use a modified Binary Search.

---

## 14. Practice Tasks

Try these problems to strengthen your Binary Search understanding:

- [ ] Find the first occurrence of a target in a sorted array.
- [ ] Find the last occurrence of a target in a sorted array.
- [ ] Count the occurrences of a target in a sorted array.
- [ ] Find the insertion position of a target.
- [ ] Find the square root of an integer using Binary Search.
- [ ] Search in a sorted array using recursion.
- [ ] Implement Binary Search for a descending sorted array.

---

## 15. Key Takeaways

- Binary Search works on sorted data.
- It reduces the search range by half in every iteration.
- Its time complexity is O(log n).
- Iterative Binary Search uses O(1) auxiliary space.
- Recursive Binary Search uses O(log n) call-stack space.
- Always update the boundaries using `mid + 1` and `mid - 1`.

**Most important pattern to remember:**

```java
int left = 0;
int right = nums.length - 1;

while (left <= right) {

    int mid = left + (right - left) / 2;

    if (nums[mid] == target) {

        return mid;

    } else if (nums[mid] < target) {

        left = mid + 1;

    } else {

        right = mid - 1;
    }
}

return -1;
```

This Binary Search template is the foundation for many advanced problems involving sorted arrays, answer-space searching, and optimization.