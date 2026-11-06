# Two Sum

**Difficulty:** Easy  
**Topic:** Array, HashMap  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given an integer array `nums` and an integer `target`, return the indices of the two numbers whose sum is equal to the target.

You can assume that exactly one valid answer exists, and you cannot use the same element twice.

You can return the answer in any order.

### Example 1

**Input:**
```java
nums = [2, 7, 11, 15]
target = 9
```

**Output:**
```java
[0, 1]
```

**Explanation:**

- `nums[0] = 2`
- `nums[1] = 7`
- `2 + 7 = 9`

Therefore, the answer is `[0, 1]`.

### Example 2

**Input:**
```java
nums = [3, 2, 4]
target = 6
```

**Output:**
```java
[1, 2]
```

**Explanation:**

`2 + 4 = 6`, so the indices are `[1, 2]`.

### Example 3

**Input:**
```java
nums = [3, 3]
target = 6
```

**Output:**
```java
[0, 1]
```

**Explanation:**

Both elements have the value `3`, but they are at different indices.

---

## 2. Approach 1: Brute Force

### Concept

The simplest approach is to check every possible pair of elements.

Use two nested loops:

1. The outer loop selects the first element.
2. The inner loop selects the second element.
3. Check whether their sum equals the target.
4. If the sum matches, return both indices.

### Java Code

```java
public class TwoSumBruteForce {

    public static int[] twoSum(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {

            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] + nums[j] == target) {

                    return new int[]{i, j};
                }
            }
        }

        return new int[]{-1, -1};
    }

    public static void main(String[] args) {

        int[] nums = {2, 7, 11, 15};
        int target = 9;

        int[] result = twoSum(nums, target);

        System.out.println(
            result[0] + ", " + result[1]
        );
    }
}
```

### Dry Run

**Input:**
```java
nums = [2, 7, 11, 15]
target = 9
```

| i | j | nums[i] | nums[j] | Sum | Result |
|---|---|---:|---:|---:|---|
| 0 | 1 | 2 | 7 | 9 | Match |

Since `2 + 7 = 9`, the method returns `[0, 1]`.

### Complexity Analysis

**Time Complexity: O(n²)**

There are two nested loops. In the worst case, we check approximately:

\[
\frac{n(n-1)}{2}
\]

pairs.

**Space Complexity: O(1)**

We use only a constant amount of additional space.

---

## 3. Approach 2: Optimized Using HashMap

### Concept

Instead of checking every pair, we can use a HashMap to find the required number in constant average lookup time.

The main idea is:

\[
\text{Required Number} = \text{Target} - \text{Current Number}
\]

For every element:

1. Calculate the required number.
2. Check whether that number already exists in the HashMap.
3. If it exists, return its stored index and the current index.
4. Otherwise, store the current number and its index.

### Example

```java
nums = [2, 7, 11, 15]
target = 9
```

**Iteration 1:**

Current number = `2`

Required number:

```java
9 - 2 = 7
```

HashMap is empty, so `7` is not found.

Store:

```java
map.put(2, 0);
```

HashMap:

```text
{2 = 0}
```

**Iteration 2:**

Current number = `7`

Required number:

```java
9 - 7 = 2
```

Check whether `2` exists in the HashMap.

It exists at index `0`.

Therefore, return:

```java
[0, 1]
```

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

public class TwoSumOptimized {

    public static int[] twoSum(int[] nums, int target) {

        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int current = nums[i];

            int required = target - current;

            if (map.containsKey(required)) {

                return new int[]{
                    map.get(required),
                    i
                };
            }

            map.put(current, i);
        }

        return new int[]{-1, -1};
    }

    public static void main(String[] args) {

        int[] nums = {2, 7, 11, 15};
        int target = 9;

        int[] result = twoSum(nums, target);

        System.out.println(
            result[0] + ", " + result[1]
        );
    }
}
```

### Dry Run

**Input:**

```java
nums = [2, 7, 11, 15]
target = 9
```

| Iteration | Current Value | Required Value | HashMap Before Check | Action |
|---|---:|---:|---|---|
| 1 | 2 | 7 | `{}` | Store `2 = 0` |
| 2 | 7 | 2 | `{2=0}` | Found! Return `[0, 1]` |

### Complexity Analysis

**Time Complexity: O(n)** average case

- We traverse the array once.
- HashMap insertion and lookup take O(1) average time.
- Therefore, total time complexity is O(n) on average.

**Space Complexity: O(n)**

In the worst case, the HashMap stores up to `n` elements.

---

## 4. Why Do We Check Before Inserting?

Consider this input:

```java
nums = [3, 3]
target = 6
```

Iteration 1:

- Current number = `3`
- Required number = `3`
- HashMap is empty.
- Store `3 = 0`.

Iteration 2:

- Current number = `3`
- Required number = `3`
- HashMap already contains `3` at index `0`.
- Return `[0, 1]`.

If we insert the current element before checking, we could accidentally use the same array element twice.

Checking first ensures that the matching element comes from an earlier index.

---

## 5. Important Java HashMap Methods

| Method | Description |
|---|---|
| `put(key, value)` | Inserts a key-value pair |
| `get(key)` | Returns the value associated with a key |
| `containsKey(key)` | Checks whether a key exists |
| `remove(key)` | Removes a key-value pair |
| `size()` | Returns the number of entries |
| `isEmpty()` | Checks whether the map is empty |

Example:

```java
Map<Integer, Integer> map = new HashMap<>();

map.put(10, 0);
map.put(20, 1);

System.out.println(map.containsKey(10)); // true
System.out.println(map.get(20));         // 1
System.out.println(map.size());          // 2
```

---

## 6. Edge Cases to Remember

| Input | Target | Expected Output |
|---|---:|---|
| `[3, 3]` | 6 | `[0, 1]` |
| `[3, 2, 4]` | 6 | `[1, 2]` |
| `[-1, -2, -3, -4]` | -6 | `[1, 3]` |
| `[0, 4, 3, 0]` | 0 | `[0, 3]` |
| `[1, 5, 8, 10]` | 15 | `[1, 3]` |

The standard problem guarantees one valid answer. The `[-1, -1]` return in our implementation is a fallback for cases where no pair exists.

---

## 7. Brute Force vs Optimized Approach

| Comparison | Brute Force | HashMap |
|---|---|---|
| Approach | Check every pair | Store previously visited elements |
| Data structure | Array | HashMap |
| Time complexity | O(n²) | O(n) average |
| Space complexity | O(1) | O(n) |
| Best use | Understanding the basic solution | Efficient interview solution |

**Interview recommendation:** First explain the brute-force approach, then optimize it using a HashMap. This demonstrates your ability to improve time complexity.

---

## 8. Interview Questions

**Q1. Why do we use HashMap in Two Sum?**

To store previously visited numbers and their indices. This allows us to find the required number in O(1) average lookup time.

**Q2. Why is the optimized solution O(n)?**

Because we traverse the array only once, performing an average O(1) HashMap lookup and insertion per element.

**Q3. Can we solve Two Sum without HashMap?**

Yes. We can use nested loops with O(n²) time and O(1) extra space.

**Q4. Can we use sorting and the two-pointer technique?**

Yes, but sorting changes the original order. We must preserve the original indices, for example by storing each number with its original index before sorting.

**Q5. Does HashMap allow duplicate keys?**

No. A HashMap stores unique keys. Inserting an existing key replaces its previous value. The Two Sum algorithm handles duplicates because it checks for a match before inserting the current element.

**Q6. What happens if the array contains negative numbers?**

The same algorithm works because subtraction and HashMap lookup work with both positive and negative integers.

---

## 9. Practice Tasks

Try implementing these variations without looking at the solution:

- [ ] Return the actual pair of numbers instead of their indices.
- [ ] Return `true` or `false` depending on whether a pair exists.
- [ ] Return all unique pairs whose sum equals the target.
- [ ] Solve Two Sum using sorting and two pointers while preserving original indices.

---

## 10. Key Takeaways

- Two Sum is a common array and HashMap interview problem.
- Brute force checks every possible pair.
- HashMap reduces average time complexity from O(n²) to O(n).
- The complement formula is `target - current`.
- Store the current number only after checking for its complement.
- Always understand the brute-force approach before presenting the optimized solution.

**Most important pattern to remember:**

```java
int required = target - nums[i];

if (map.containsKey(required)) {
    return new int[]{map.get(required), i};
}

map.put(nums[i], i);
```

This complement-search pattern is useful in many other array and HashMap problems.