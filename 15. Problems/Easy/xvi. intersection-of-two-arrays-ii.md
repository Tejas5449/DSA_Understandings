# Intersection of Two Arrays II

**Difficulty:** Easy  
**LeetCode:** [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)  
**Topics:** Arrays, HashMap, Frequency Counting, Two Pointers

---

## 1. Problem Statement

Given two integer arrays `nums1` and `nums2`, return their intersection.

Each element in the result must appear as many times as it appears in both arrays.

The order of the result does not matter.

### Example 1

**Input:**

```java
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
```

**Output:**

```java
[2, 2]
```

**Explanation:** The number `2` appears twice in both arrays, so it appears twice in the result.

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

**Explanation:**
- `4` appears once in both arrays.
- `9` appears once in `nums1` and twice in `nums2`.
- `5` does not appear in `nums2`.

Therefore, the result is `[4, 9]`.

---

## 2. Understanding the Problem

This problem is similar to Intersection of Two Arrays (LeetCode 349), but there is one important difference.

In LeetCode 349, every common element appears only once.

In LeetCode 350, duplicate occurrences must be preserved.

### Example

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]

Intersection = [2, 2]
```

The frequency of an element in the result is:

```text
Minimum frequency in nums1 and nums2
```

For example:

| Element | Frequency in nums1 | Frequency in nums2 | Result Frequency |
|---|---:|---:|---:|
| 1 | 2 | 0 | 0 |
| 2 | 2 | 2 | 2 |

This is the main concept to understand before implementing the solution.

---

## 3. Approach 1: Brute Force

We can compare every element in `nums1` with elements in `nums2`.

To avoid using the same occurrence more than once, we mark matched elements in `nums2` as used.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class IntersectionBruteForce {

    public static int[] intersect(int[] nums1, int[] nums2) {

        boolean[] used = new boolean[nums2.length];

        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < nums1.length; i++) {

            for (int j = 0; j < nums2.length; j++) {

                if (nums1[i] == nums2[j] && !used[j]) {

                    result.add(nums1[i]);

                    used[j] = true;

                    break;
                }
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
            Arrays.toString(intersect(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[2, 2]
```

### Complexity Analysis

- Time Complexity: O(n × m), where `n` and `m` are the array lengths.
- Space Complexity: O(m + r), where `m` is the size of the `used` array and `r` is the number of intersection elements.

This approach works, but it is inefficient for large arrays.

---

## 4. Approach 2: Using HashMap

This is an efficient solution when we don't want to modify the input arrays.

### Main Idea

Use a HashMap to store the frequency of every element in the first array.

Then traverse the second array and check whether each element exists in the HashMap.

If it exists:
1. Add that element to the result.
2. Decrease its frequency by one.
3. Remove it from the map when its frequency reaches zero.

### Step 1: Create a Frequency Map

```java
Map<Integer, Integer> frequency = new HashMap<>();
```

### Step 2: Count Elements in nums1

For every number:

```java
frequency.put(
    num,
    frequency.getOrDefault(num, 0) + 1
);
```

### Step 3: Traverse nums2

Check whether the element exists:

```java
if (frequency.getOrDefault(num, 0) > 0) {
    result.add(num);
}
```

After adding it, decrease its frequency.

### Complete Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class IntersectionHashMap {

    public static int[] intersect(int[] nums1, int[] nums2) {

        Map<Integer, Integer> frequency = new HashMap<>();

        List<Integer> result = new ArrayList<>();

        // Step 1: Count frequencies in nums1
        for (int num : nums1) {

            frequency.put(
                num,
                frequency.getOrDefault(num, 0) + 1
            );
        }

        // Step 2: Find common elements
        for (int num : nums2) {

            int count = frequency.getOrDefault(num, 0);

            if (count > 0) {

                result.add(num);

                frequency.put(num, count - 1);
            }
        }

        // Step 3: Convert List into int[]
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
            Arrays.toString(intersect(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[2, 2]
```

---

## 5. Detailed Dry Run: HashMap Approach

**Input:**

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]
```

### Step 1: Build the Frequency Map

Traverse `nums1`.

| Number | Frequency Map |
|---|---|
| 1 | `{1=1}` |
| 2 | `{1=1, 2=1}` |
| 2 | `{1=1, 2=2}` |
| 1 | `{1=2, 2=2}` |

Final frequency map:

```text
{
    1 = 2,
    2 = 2
}
```

### Step 2: Traverse nums2

Initial result:

```text
result = []
```

**Iteration 1**

Current number:

```text
num = 2
```

Frequency of `2` is `2`.

Since the frequency is greater than zero:

- Add `2` to the result.
- Decrease its frequency to `1`.

```text
frequency = {1=2, 2=1}
result = [2]
```

**Iteration 2**

Current number:

```text
num = 2
```

Frequency of `2` is `1`.

Since the frequency is greater than zero:

- Add `2` to the result.
- Decrease its frequency to `0`.

```text
frequency = {1=2, 2=0}
result = [2, 2]
```

### Final Result

```text
[2, 2]
```

Both occurrences are included because the number appears twice in both arrays.

---

## 6. Approach 3: Sorting and Two Pointers

We can also solve this problem using the two-pointer technique.

This approach is particularly useful when both arrays can be sorted.

### Main Idea

1. Sort both arrays.
2. Initialize two pointers at index zero.
3. Compare the current elements.
4. If both elements are equal, add the value to the result and move both pointers.
5. If the first element is smaller, move the first pointer.
6. Otherwise, move the second pointer.

Because we move both pointers after finding a match, duplicate occurrences are handled correctly.

### Java Code

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class IntersectionTwoPointers {

    public static int[] intersect(int[] nums1, int[] nums2) {

        Arrays.sort(nums1);
        Arrays.sort(nums2);

        List<Integer> result = new ArrayList<>();

        int i = 0;
        int j = 0;

        while (i < nums1.length && j < nums2.length) {

            if (nums1[i] == nums2[j]) {

                result.add(nums1[i]);

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

        int[] nums1 = {1, 2, 2, 1};
        int[] nums2 = {2, 2};

        System.out.println(
            Arrays.toString(intersect(nums1, nums2))
        );
    }
}
```

**Output:**

```text
[2, 2]
```

**Important:** This approach modifies the original arrays because it sorts them.

---

## 7. Detailed Dry Run: Two-Pointer Approach

**Input:**

```text
nums1 = [4, 9, 5]
nums2 = [9, 4, 9, 8, 4]
```

### Step 1: Sort Both Arrays

```text
nums1 = [4, 5, 9]
nums2 = [4, 4, 8, 9, 9]
```

Initial pointers:

```java
int i = 0;
int j = 0;
```

### Step 2: Compare Elements

| Iteration | nums1[i] | nums2[j] | Action | Result |
|---|---:|---:|---|---|
| 1 | 4 | 4 | Equal, add 4 and move both | `[4]` |
| 2 | 5 | 4 | Move `j` | `[4]` |
| 3 | 5 | 8 | Move `i` | `[4]` |
| 4 | 9 | 8 | Move `j` | `[4]` |
| 5 | 9 | 9 | Equal, add 9 and move both | `[4, 9]` |

The second occurrence of `9` in `nums2` is not matched because `nums1` has no remaining `9`.

### Final Result

```text
[4, 9]
```

---

## 8. Complexity Analysis

Let:

- `n` = Length of `nums1`.
- `m` = Length of `nums2`.
- `r` = Number of elements in the intersection, including duplicates.

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Brute Force | O(n × m) | O(m + r) |
| HashMap | O(n + m) average | O(n + r) |
| Sorting + Two Pointers | O(n log n + m log m) | O(r), excluding sorting workspace |

### Which Approach Should We Use?

**HashMap approach:**
- When input arrays should not be modified.
- When efficient frequency counting is needed.
- When you want O(n + m) average time.

**Two-pointer approach:**
- When sorting is acceptable.
- When you want to practice pointer-based array traversal.
- When the arrays are already sorted, you can skip sorting and achieve O(n + m) time.

---

## 9. Important Difference: LeetCode 349 vs. LeetCode 350

These two problems look similar, but their requirements are different.

| Feature | LeetCode 349 | LeetCode 350 |
|---|---|---|
| Problem | Intersection of Two Arrays | Intersection of Two Arrays II |
| Duplicates | Removed | Preserved |
| Result frequency | At most once | Minimum frequency in both arrays |
| Common approach | HashSet | HashMap |
| Example | `[2]` | `[2, 2]` |

Example:

```text
nums1 = [1, 2, 2, 1]
nums2 = [2, 2]

LeetCode 349 → [2]

LeetCode 350 → [2, 2]
```

---

## 10. Edge Cases

| nums1 | nums2 | Expected Result |
|---|---|---|
| `[1, 2, 2, 1]` | `[2, 2]` | `[2, 2]` |
| `[4, 9, 5]` | `[9, 4, 9, 8, 4]` | `[4, 9]` |
| `[1, 2, 3]` | `[4, 5, 6]` | `[]` |
| `[1, 1, 1]` | `[1, 1]` | `[1, 1]` |
| `[5]` | `[5]` | `[5]` |
| `[]` | `[1, 2]` | `[]` |
| `[1, 1, 2]` | `[1, 2, 2]` | `[1, 2]` |

---

## 11. Interview Questions

### Q1. What is the difference between HashSet and HashMap?

- HashSet stores unique elements.
- HashMap stores key-value pairs.
- HashMap is useful for maintaining element frequencies.

### Q2. Why do we use HashMap in this problem?

Because we need to track how many times each element appears in the first array.

### Q3. Why do we decrease the frequency after finding a match?

To ensure that one occurrence in `nums1` is matched with at most one occurrence in `nums2`.

### Q4. What is the average time complexity of HashMap lookup?

O(1) average.

### Q5. Why do we use two pointers after sorting?

Sorting allows us to compare elements in order and move only the pointer pointing to the smaller value.

### Q6. Does the two-pointer solution preserve duplicate occurrences?

Yes. Every time two elements match, we add one occurrence and move both pointers.

### Q7. Which approach is better when input arrays must remain unchanged?

The HashMap approach, because it does not sort or modify the input arrays.

### Q8. Can we use a HashSet for this problem?

A simple HashSet is not sufficient because it removes duplicates. We need frequency information, which can be maintained using a HashMap.

---

## 12. Practice Problems

Try these problems to strengthen your understanding.

1. [Intersection of Two Arrays – LeetCode 349](https://leetcode.com/problems/intersection-of-two-arrays/)
2. [Two Sum – LeetCode 1](https://leetcode.com/problems/two-sum/)
3. [Contains Duplicate – LeetCode 217](https://leetcode.com/problems/contains-duplicate/)
4. [Find Common Characters – LeetCode 1002](https://leetcode.com/problems/find-common-characters/)

---

## 13. Final Summary

We learned three approaches:

1. Brute Force – compare elements and mark matched occurrences.
2. HashMap – count frequencies and decrease them when matched.
3. Sorting + Two Pointers – sort both arrays and compare elements.

### Recommended Interview Solution: HashMap

```java
public int[] intersect(int[] nums1, int[] nums2) {

    Map<Integer, Integer> frequency = new HashMap<>();

    List<Integer> result = new ArrayList<>();

    for (int num : nums1) {
        frequency.put(
            num,
            frequency.getOrDefault(num, 0) + 1
        );
    }

    for (int num : nums2) {

        int count = frequency.getOrDefault(num, 0);

        if (count > 0) {
            result.add(num);
            frequency.put(num, count - 1);
        }
    }

    int[] answer = new int[result.size()];

    for (int i = 0; i < result.size(); i++) {
        answer[i] = result.get(i);
    }

    return answer;
}
```

**Remember:**
- Use HashSet for unique elements.
- Use HashMap for frequency counting.
- Use two pointers for sorted arrays.
- For intersection with duplicates, the result frequency is the minimum frequency in the two arrays.
