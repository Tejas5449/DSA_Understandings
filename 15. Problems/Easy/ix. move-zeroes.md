# Move Zeroes

**Difficulty:** Easy  
**Topics:** Array, Two Pointers, In-Place Modification  
**LeetCode:** 283

---

## 1. Problem Statement

Given an integer array `nums`, move all zeros to the end while maintaining the relative order of the non-zero elements.

You must modify the original array **in-place**, without creating a copy of the array.

### Example 1

**Input:**

```java
nums = [0, 1, 0, 3, 12]
```

**Output:**

```java
[1, 3, 12, 0, 0]
```

**Explanation:**

The non-zero elements are `1`, `3`, and `12`.

They must remain in the same order, while all zeros move to the end.

### Example 2

**Input:**

```java
nums = [0]
```

**Output:**

```java
[0]
```

### Example 3

**Input:**

```java
nums = [4, 0, 5, 0, 0, 3]
```

**Output:**

```java
[4, 5, 3, 0, 0, 0]
```

---

## 2. Understanding the Problem

There are three important requirements:

1. Move all zeros to the end.
2. Preserve the relative order of non-zero elements.
3. Modify the original array instead of creating another array.

Consider:

```text
Original:
[0, 1, 0, 3, 12]

Non-zero elements:
[1, 3, 12]

Expected:
[1, 3, 12, 0, 0]
```

Notice that the non-zero elements remain in their original order.

We cannot simply sort the array, because sorting may change that order.

---

## 3. Approach 1: Using an Extra Array

### Concept

This is an easy way to understand the problem before optimizing.

Steps:

1. Create a new array of the same size.
2. Copy all non-zero elements into it.
3. The remaining positions will automatically contain zero.
4. Copy the result back into the original array.

### Java Code

```java
import java.util.Arrays;

public class MoveZeroesExtraArray {

    public static void moveZeroes(int[] nums) {

        int[] result = new int[nums.length];

        int index = 0;

        for (int num : nums) {

            if (num != 0) {
                result[index] = num;
                index++;
            }
        }

        for (int i = 0; i < nums.length; i++) {
            nums[i] = result[i];
        }
    }

    public static void main(String[] args) {

        int[] nums = {0, 1, 0, 3, 12};

        moveZeroes(nums);

        System.out.println(Arrays.toString(nums));
    }
}
```

**Output:**

```text
[1, 3, 12, 0, 0]
```

### Dry Run

Input:

```text
[0, 1, 0, 3, 12]
```

Initially:

```text
result = [0, 0, 0, 0, 0]
index = 0
```

| Current Number | Action | Result Array |
|---|---|---|
| 0 | Skip | `[0, 0, 0, 0, 0]` |
| 1 | Insert at 0 | `[1, 0, 0, 0, 0]` |
| 0 | Skip | `[1, 0, 0, 0, 0]` |
| 3 | Insert at 1 | `[1, 3, 0, 0, 0]` |
| 12 | Insert at 2 | `[1, 3, 12, 0, 0]` |

Finally, copy the result into the original array.

### Complexity

- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

Although efficient in time, this solution does not satisfy the in-place requirement because it uses an additional array.

---

## 4. Approach 2: Two Pointers (Optimized)

### Concept

We can solve the problem without creating an extra array.

Use two pointers:

- `i`: Traverses the entire array.
- `insertPosition`: Tracks where the next non-zero element should be placed.

The algorithm works in two phases.

**Phase 1: Move non-zero elements to the beginning**

Whenever a non-zero element is found, place it at `insertPosition` and increment `insertPosition`.

**Phase 2: Fill the remaining positions with zeros**

After processing the array, fill every index from `insertPosition` to the end with zero.

### Java Code

```java
import java.util.Arrays;

public class MoveZeroesOptimized {

    public static void moveZeroes(int[] nums) {

        int insertPosition = 0;

        // Move all non-zero elements to the beginning.
        for (int i = 0; i < nums.length; i++) {

            if (nums[i] != 0) {

                nums[insertPosition] = nums[i];

                insertPosition++;
            }
        }

        // Fill the remaining positions with zero.
        while (insertPosition < nums.length) {

            nums[insertPosition] = 0;

            insertPosition++;
        }
    }

    public static void main(String[] args) {

        int[] nums = {0, 1, 0, 3, 12};

        moveZeroes(nums);

        System.out.println(Arrays.toString(nums));
    }
}
```

**Output:**

```text
[1, 3, 12, 0, 0]
```

---

## 5. Detailed Dry Run

**Input:**

```java
nums = [0, 1, 0, 3, 12]
```

Initially:

```java
insertPosition = 0;
```

### Phase 1: Move Non-Zero Elements

| i | nums[i] | insertPosition Before | Action | Array After |
|---|---:|---:|---|---|
| 0 | 0 | 0 | Skip | `[0, 1, 0, 3, 12]` |
| 1 | 1 | 0 | `nums[0] = 1` | `[1, 1, 0, 3, 12]` |
| 2 | 0 | 1 | Skip | `[1, 1, 0, 3, 12]` |
| 3 | 3 | 1 | `nums[1] = 3` | `[1, 3, 0, 3, 12]` |
| 4 | 12 | 2 | `nums[2] = 12` | `[1, 3, 12, 3, 12]` |

At the end of Phase 1:

```java
insertPosition = 3;
```

Current array:

```text
[1, 3, 12, 3, 12]
```

The first three positions are correct. The remaining values must be replaced with zeros.

### Phase 2: Fill Remaining Positions

| insertPosition | Action | Array After |
|---|---|---|
| 3 | `nums[3] = 0` | `[1, 3, 12, 0, 12]` |
| 4 | `nums[4] = 0` | `[1, 3, 12, 0, 0]` |

Final result:

```text
[1, 3, 12, 0, 0]
```

---

## 6. Why Does the Two-Pointer Approach Work?

The key idea is that `insertPosition` always points to the next location where a non-zero element should be stored.

For example:

```text
Input:
[0, 1, 0, 3, 12]
```

As we traverse:

```text
Found 1  -> store at index 0
Found 3  -> store at index 1
Found 12 -> store at index 2
```

Since we process the elements from left to right, their original relative order is preserved.

After placing all non-zero elements, we fill the remaining positions with zeros.

This guarantees the correct result.

---

## 7. Approach 3: Two Pointers Using Swapping

Another optimized approach is to swap non-zero elements into their correct positions.

### Concept

Use:

- `left`: The position where the next non-zero element belongs.
- `right`: The current element being examined.

When `nums[right]` is non-zero:

1. Swap `nums[left]` and `nums[right]`.
2. Increment `left`.

This keeps non-zero elements at the beginning while moving zeros toward the end.

### Java Code

```java
import java.util.Arrays;

public class MoveZeroesSwapping {

    public static void moveZeroes(int[] nums) {

        int left = 0;

        for (int right = 0; right < nums.length; right++) {

            if (nums[right] != 0) {

                int temp = nums[left];

                nums[left] = nums[right];

                nums[right] = temp;

                left++;
            }
        }
    }

    public static void main(String[] args) {

        int[] nums = {0, 1, 0, 3, 12};

        moveZeroes(nums);

        System.out.println(Arrays.toString(nums));
    }
}
```

**Output:**

```text
[1, 3, 12, 0, 0]
```

### Dry Run

| right | nums[right] | left Before | Action | Array After |
|---|---:|---:|---|---|
| 0 | 0 | 0 | Skip | `[0, 1, 0, 3, 12]` |
| 1 | 1 | 0 | Swap indices 0 and 1 | `[1, 0, 0, 3, 12]` |
| 2 | 0 | 1 | Skip | `[1, 0, 0, 3, 12]` |
| 3 | 3 | 1 | Swap indices 1 and 3 | `[1, 3, 0, 0, 12]` |
| 4 | 12 | 2 | Swap indices 2 and 4 | `[1, 3, 12, 0, 0]` |

### Complexity

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

This solution is fully in-place and does not require a second pass to fill zeros.

---

## 8. Comparing All Approaches

| Approach | Time Complexity | Extra Space | In-Place |
|---|---|---|---|
| Extra Array | O(n) | O(n) | No |
| Two Pointers + Fill Zeros | O(n) | O(1) | Yes |
| Two Pointers + Swap | O(n) | O(1) | Yes |

**Recommended interview approach:** Two Pointers.

Both optimized versions are valid. The swapping approach is particularly useful for learning in-place array rearrangement.

---

## 9. Edge Cases

| Input | Expected Output |
|---|---|
| `[0, 1, 0, 3, 12]` | `[1, 3, 12, 0, 0]` |
| `[0]` | `[0]` |
| `[1, 2, 3]` | `[1, 2, 3]` |
| `[0, 0, 0]` | `[0, 0, 0]` |
| `[0, 0, 1]` | `[1, 0, 0]` |
| `[1, 0, 2, 0, 3]` | `[1, 2, 3, 0, 0]` |
| `[-1, 0, 2]` | `[-1, 2, 0]` |
| `[]` | `[]` |

---

## 10. Common Interview Mistakes

### Mistake 1: Sorting the Array

Incorrect:

```java
Arrays.sort(nums);
```

Sorting changes the relative order of non-zero elements and may place negative numbers before zeros.

For example:

```text
Input:
[3, 0, 1, 0, 2]

Required:
[3, 1, 2, 0, 0]

Sorted:
[0, 0, 1, 2, 3]
```

Sorting does not solve the problem.

### Mistake 2: Creating Another Array

The problem specifically asks for in-place modification.

Using an additional array requires O(n) extra space.

### Mistake 3: Forgetting to Fill Zeros

In the overwrite approach, moving non-zero elements to the beginning is not enough.

You must fill the remaining positions with zeros.

### Mistake 4: Returning a New Array

The standard problem expects the original array to be modified.

The method signature is:

```java
public void moveZeroes(int[] nums)
```

You do not need to return the array.

---

## 11. Interview Questions

**Q1. What is the Two Pointers technique?**

It is an algorithmic technique that uses two indices to process elements efficiently, often avoiding nested loops or extra arrays.

**Q2. Why do we use two pointers in Move Zeroes?**

One pointer traverses the array, while the other tracks the next position for a non-zero element.

**Q3. What is the optimized time complexity?**

O(n), because each element is processed a constant number of times.

**Q4. What is the optimized space complexity?**

O(1), because only a fixed number of variables are used.

**Q5. Why should we not sort the array?**

Because sorting changes the relative order of non-zero elements.

**Q6. What does in-place modification mean?**

It means changing the original data structure without allocating another array proportional to the input size.

**Q7. Can we solve the problem with a single pass?**

Yes. The swapping approach completes the rearrangement in one traversal.

**Q8. What happens if there are no zeros?**

The array remains unchanged.

---

## 12. Practice Tasks

Try solving these variations:

- [ ] Move all negative numbers to the end while preserving the order of non-negative numbers.
- [ ] Move all even numbers to the beginning of an array.
- [ ] Remove all occurrences of a given value in-place.
- [ ] Remove duplicates from a sorted array.
- [ ] Move all zeros to the beginning while preserving the order of non-zero elements.
- [ ] Count how many zeros are present without modifying the array.

---

## 13. Key Takeaways

- Move Zeroes is an important Two Pointers problem.
- Preserve the relative order of non-zero elements.
- Do not use sorting.
- The overwrite approach moves non-zero values first, then fills zeros.
- The swapping approach rearranges the array in one pass.
- Both optimized approaches use O(n) time and O(1) extra space.

**Most important pattern to remember:**

```java
int left = 0;

for (int right = 0; right < nums.length; right++) {

    if (nums[right] != 0) {

        int temp = nums[left];

        nums[left] = nums[right];

        nums[right] = temp;

        left++;
    }
}
```

This two-pointer pattern is useful for many in-place array problems, including removing elements, partitioning arrays, and removing duplicates.