# Intersection of Two Arrays

**Difficulty:** Easy  
**LeetCode:** [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)  
**Topics:** Arrays, HashSet, HashMap, Two Pointers

---

## 1. Problem Statement

Given two integer arrays `nums1` and `nums2`, return an array containing their intersection.

Each element in the result must be unique, and you may return the result in any order.

An element belongs to the intersection if it appears in both arrays.

### Example 1

**Input:**

```java
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
```

**Output:**

```java
[2]
```

**Explanation:** The number `2` appears in both arrays. It should appear only once in the result.

### Example 2

**Input:**

```java
nums1 = [4, 9, 5]
nums2 = [9, 4, 9, 8, 4]
```

**Output:**

```java
[4, 9]
```

**Explanation:** Both `4` and `9` appear in both arrays. The output order does not matter.

---

## 2. Understanding the Problem

We need to find common elements between two arrays.

For example:

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]

Common element = 2

Result = [2]
```

Important points:

- The result must contain unique elements.
- Duplicate elements should not appear more than once.
- The output order does not matter.
- An element must exist in both arrays to be included.

---

## 3. Approach 1: Brute Force

Compare every element of the first array with every element of the second array.

If an element exists in both arrays, add it to the result only if it has not already been added.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class IntersectionBruteForce {

    public static int[] intersection(int[] nums1, int[] nums2) {

        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < nums1.length; i++) {

            boolean found = false;

            for (int j = 0; j < nums2.length; j++) {

                if (nums1[i] == nums2[j]) {
                    found = true;
                    break;
                }
            }

            if (found && !result.contains(nums1[i])) {
                result.add(nums1[i]);
            }
        }

        int[] answer = new int[result.size()];

        for (int i = 0; i < result.size(); i++) {
            answer[i] = result.get(i);
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 2, 1};
        int[] nums2 = {2, 2};

        System.out.println(
            Arrays.toString(intersection(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[2]
```

### Complexity Analysis

Let `n` be the length of `nums1` and `m` be the length of `nums2`.

- Time Complexity: O(n × m + n × r), where `r` is the number of unique elements collected. The `contains()` operation on the result list takes O(r).
- Space Complexity: O(r) for the result.

This approach is easy to understand but inefficient for large arrays.

---

## 4. Approach 2: Using HashSet

This is the recommended solution.

A `HashSet` stores unique elements and provides average O(1) lookup time.

### Logic

1. Create a HashSet.
2. Add all elements of `nums1` into the set.
3. Traverse `nums2`.
4. Check whether each element exists in the set.
5. If it exists, add it to the result.
6. Remove the element from the set so it cannot be added again.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class IntersectionHashSet {

    public static int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> set = new HashSet<>();

        Set<Integer> result = new HashSet<>();

        // Add nums1 elements into the set
        for (int num : nums1) {
            set.add(num);
        }

        // Find common elements
        for (int num : nums2) {

            if (set.contains(num)) {
                result.add(num);
            }
        }

        // Convert result set into an array
        int[] answer = new int[result.size()];

        int index = 0;

        for (int num : result) {
            answer[index++] = num;
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 2, 1};
        int[] nums2 = {2, 2};

        System.out.println(
            Arrays.toString(intersection(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[2]
```

### Complexity Analysis

- Time Complexity: O(n + m) average.
- Space Complexity: O(n + r), where `r` is the number of unique common elements.

The HashSet provides average O(1) insertion and lookup.

---

## 5. Optimized HashSet Approach

We can use a single HashSet instead of maintaining separate sets for the input and result.

After finding a common element, remove it from the original set. This ensures that duplicate values are not added again.

### Java Code

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class IntersectionOptimized {

    public static int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> set = new HashSet<>();

        Set<Integer> result = new HashSet<>();

        for (int num : nums1) {
            set.add(num);
        }

        for (int num : nums2) {

            if (set.remove(num)) {
                result.add(num);
            }
        }

        int[] answer = new int[result.size()];

        int index = 0;

        for (int num : result) {
            answer[index++] = num;
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] nums1 = {4, 9, 5};
        int[] nums2 = {9, 4, 9, 8, 4};

        System.out.println(
            Arrays.toString(intersection(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[4, 9]
```

### Why Does `remove()` Work?

Consider:

```java
if (set.remove(num)) {
    result.add(num);
}
```

`remove()` returns:

- `true` if the element existed and was removed.
- `false` if the element did not exist.

Once an element is removed, another occurrence of the same element cannot be added again.

This automatically handles duplicates.

---

## 6. Detailed Dry Run

Let's understand the optimized HashSet approach.

**Input:**

```text
nums1 = [4, 9, 5]
nums2 = [9, 4, 9, 8, 4]
```

### Step 1: Add nums1 Elements

```java
Set<Integer> set = new HashSet<>();
```

After adding all elements:

```text
set = [4, 9, 5]
result = []
```

### Step 2: Traverse nums2

**Iteration 1**

Current element:

```text
num = 9
```

`set.remove(9)` returns `true`.

```text
set = [4, 5]
result = [9]
```

**Iteration 2**

Current element:

```text
num = 4
```

`set.remove(4)` returns `true`.

```text
set = [5]
result = [9, 4]
```

**Iteration 3**

Current element:

```text
num = 9
```

`set.remove(9)` returns `false` because `9` was already removed.

```text
result = [9, 4]
```

**Iteration 4**

Current element:

```text
num = 8
```

`set.remove(8)` returns `false`.

```text
result = [9, 4]
```

**Iteration 5**

Current element:

```text
num = 4
```

`set.remove(4)` returns `false` because `4` was already removed.

```text
result = [9, 4]
```

### Final Result

```text
[4, 9]
```

The order may vary because HashSet does not guarantee insertion order.

---

## 7. Approach 3: Sorting and Two Pointers

Another approach is to sort both arrays and compare their elements using two pointers.

### Logic

1. Sort both arrays.
2. Initialize two pointers `i` and `j` to zero.
3. Compare `nums1[i]` and `nums2[j]`.
4. If they are equal, add the value to the result and move both pointers.
5. If `nums1[i]` is smaller, move `i`.
6. Otherwise, move `j`.
7. Skip duplicate values to keep the result unique.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class IntersectionTwoPointers {

    public static int[] intersection(int[] nums1, int[] nums2) {

        Arrays.sort(nums1);
        Arrays.sort(nums2);

        List<Integer> result = new ArrayList<>();

        int i = 0;
        int j = 0;

        while (i < nums1.length && j < nums2.length) {

            if (nums1[i] == nums2[j]) {

                // Add only if it is not a duplicate
                if (result.isEmpty() ||
                    result.get(result.size() - 1) != nums1[i]) {

                    result.add(nums1[i]);
                }

                i++;
                j++;

            } else if (nums1[i] < nums2[j]) {

                i++;

            } else {

                j++;
            }
        }

        int[] answer = new int[result.size()];

        for (int k = 0; k < result.size(); k++) {
            answer[k] = result.get(k);
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] nums1 = {4, 9, 5};
        int[] nums2 = {9, 4, 9, 8, 4};

        System.out.println(
            Arrays.toString(intersection(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[4, 9]
```

### Complexity Analysis

- Time Complexity: O(n log n + m log m), due to sorting, followed by O(n + m) pointer traversal.
- Extra Space: O(r) for the result list, excluding sorting workspace.

**Important:** This approach modifies the original input arrays because it sorts them.

---

## 8. Understanding the Two-Pointer Technique

Suppose we have:

```text
nums1 = [1, 2, 4, 5]
nums2 = [2, 3, 4, 6]
```

Both arrays are sorted.

Initially:

```text
i = 0
j = 0
```

Compare the current values:

| nums1[i] | nums2[j] | Action |
|---:|---:|---|
| 1 | 2 | Move `i` because 1 is smaller |
| 2 | 2 | Common element: add 2 |
| 4 | 3 | Move `j` because 3 is smaller |
| 4 | 4 | Common element: add 4 |
| 5 | 6 | Move `i` because 5 is smaller |

Final result:

```text
[2, 4]
```

### Important Rule

When both arrays are sorted:

- If the first value is smaller, move the first pointer.
- If the second value is smaller, move the second pointer.
- If both values are equal, we found a common element.

---

## 9. Complexity Comparison

Let:

- `n` = Length of `nums1`.
- `m` = Length of `nums2`.
- `r` = Number of unique common elements.

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Brute Force | O(n × m + n × r) | O(r) |
| HashSet | O(n + m) average | O(n + r) |
| HashSet with remove | O(n + m) average | O(n + r) |
| Sorting + Two Pointers | O(n log n + m log m) | O(r), excluding sorting workspace |

**Recommended approach:** HashSet, when the input arrays do not need to remain sorted or unchanged.

---

## 10. Important Difference: Intersection vs. Intersection II

There are two similar LeetCode problems.

### Intersection of Two Arrays (LeetCode 349)

Each element must appear only once in the result.

Example:

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]

Output = [2]
```

### Intersection of Two Arrays II (LeetCode 350)

Each element must appear as many times as it appears in both arrays.

Example:

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]

Output = [2, 2]
```

The second problem requires frequency counting, usually using a HashMap or sorting with two pointers.

---

## 11. Edge Cases

| nums1 | nums2 | Expected Result |
|---|---|---|
| `[1, 2, 2, 1]` | `[2, 2]` | `[2]` |
| `[4, 9, 5]` | `[9, 4, 9, 8, 4]` | `[4, 9]` |
| `[1, 2, 3]` | `[4, 5, 6]` | `[]` |
| `[1, 1, 1]` | `[1, 1]` | `[1]` |
| `[5]` | `[5]` | `[5]` |
| `[0, 0]` | `[0]` | `[0]` |

---

## 12. Interview Questions

### Q1. What is a HashSet?

A HashSet is a Java collection that stores unique elements. It does not guarantee insertion order.

### Q2. Why do we use HashSet for intersection?

It provides average O(1) insertion and lookup, making it efficient to find common elements.

### Q3. How do we avoid duplicate elements in the result?

We can use another HashSet or remove an element from the first set after finding it.

### Q4. What is the difference between HashSet and HashMap?

- HashSet stores unique values.
- HashMap stores key-value pairs.
- HashSet internally uses a HashMap implementation.

### Q5. What is the average time complexity of HashSet `contains()`?

O(1) average, although worst-case performance can be worse because of hash collisions.

### Q6. When should we use two pointers?

Two pointers are especially useful when arrays are sorted and we need to compare elements efficiently.

### Q7. What is the difference between LeetCode 349 and 350?

- LeetCode 349 returns unique common elements.
- LeetCode 350 returns common elements including duplicate occurrences up to the minimum frequency in both arrays.

---

## 13. Practice Problems

Try these problems to strengthen your understanding.

1. [Intersection of Two Arrays II – LeetCode 350](https://leetcode.com/problems/intersection-of-two-arrays-ii/)
2. [Contains Duplicate – LeetCode 217](https://leetcode.com/problems/contains-duplicate/)
3. [Two Sum – LeetCode 1](https://leetcode.com/problems/two-sum/)
4. [Intersection of Multiple Arrays – LeetCode 2248](https://leetcode.com/problems/intersection-of-multiple-arrays/)

---

## 14. Final Summary

We learned three approaches:

1. Brute Force – compare every element.
2. HashSet – efficiently find unique common elements.
3. Sorting + Two Pointers – compare sorted arrays.

### Recommended Interview Solution

```java
public int[] intersection(int[] nums1, int[] nums2) {

    Set<Integer> set = new HashSet<>();

    Set<Integer> result = new HashSet<>();

    for (int num : nums1) {
        set.add(num);
    }

    for (int num : nums2) {

        if (set.remove(num)) {
            result.add(num);
        }
    }

    int[] answer = new int[result.size()];

    int index = 0;

    for (int num : result) {
        answer[index++] = num;
    }

    return answer;
}
```

**Remember:**

- Use HashSet when you need unique elements.
- Use HashMap when you need element frequencies.
- Use two pointers when working with sorted arrays.
