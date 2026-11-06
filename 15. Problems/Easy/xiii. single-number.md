# Single Number

**Difficulty:** Easy  
**LeetCode:** [136. Single Number](https://leetcode.com/problems/single-number/)  
**Topic:** Arrays, Bit Manipulation, XOR

---

## 1. Problem Statement

Given a non-empty integer array `nums`, every element appears twice except for one element that appears exactly once.

Find the element that appears only once.

You must implement a solution with **O(n) time complexity and O(1) extra space**.

### Example 1

**Input:**
```java
nums = [2, 2, 1]
```

**Output:**
```java
1
```

**Explanation:** The number `2` appears twice, while `1` appears only once.

### Example 2

**Input:**
```java
nums = [4, 1, 2, 1, 2]
```

**Output:**
```java
4
```

### Example 3

**Input:**
```java
nums = [1]
```

**Output:**
```java
1
```

---

## 2. Understanding the Problem

We need to find the number that does not have a duplicate.

For example:

```text
Array = [4, 1, 2, 1, 2]

4 → appears once
1 → appears twice
2 → appears twice

Answer = 4
```

We could solve this using a HashMap, but the problem requires constant extra space.

A better approach uses the **XOR operator**.

---

## 3. Understanding XOR

XOR is a bitwise operator represented by `^` in Java.

It compares two numbers bit by bit.

| A | B | A ^ B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Important XOR Properties

There are three important properties to remember:

**Property 1: Same numbers produce zero**

```java
5 ^ 5 = 0
```

Because equal bits produce `0`.

**Property 2: XOR with zero returns the same number**

```java
5 ^ 0 = 5
```

**Property 3: Order does not matter**

```java
5 ^ 3 ^ 5 = 5 ^ 5 ^ 3
             = 0 ^ 3
             = 3
```

This means duplicate numbers cancel each other out.

---

## 4. Approach 1: Brute Force

Check every element and count how many times it appears.

If an element appears only once, return it.

### Java Code

```java
public class SingleNumberBruteForce {

    public static int singleNumber(int[] nums) {

        for (int i = 0; i < nums.length; i++) {

            int count = 0;

            for (int j = 0; j < nums.length; j++) {

                if (nums[i] == nums[j]) {
                    count++;
                }
            }

            if (count == 1) {
                return nums[i];
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {4, 1, 2, 1, 2};

        System.out.println(singleNumber(nums));
    }
}
```

**Output:**

```text
4
```

### Complexity Analysis

- Time Complexity: O(n²), because we use nested loops.
- Space Complexity: O(1), because we do not use extra data structures.

This solution works, but it is inefficient for large arrays.

---

## 5. Approach 2: Using HashMap

We can use a HashMap to store the frequency of each number.

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

public class SingleNumberHashMap {

    public static int singleNumber(int[] nums) {

        Map<Integer, Integer> frequency = new HashMap<>();

        for (int num : nums) {

            frequency.put(
                num,
                frequency.getOrDefault(num, 0) + 1
            );
        }

        for (int num : nums) {

            if (frequency.get(num) == 1) {
                return num;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {4, 1, 2, 1, 2};

        System.out.println(singleNumber(nums));
    }
}
```

**Output:**

```text
4
```

### Complexity Analysis

- Time Complexity: O(n) average.
- Space Complexity: O(n), because the HashMap stores distinct elements.

This is better than brute force, but it does not satisfy the O(1) extra-space requirement.

---

## 6. Approach 3: Optimal Solution Using XOR

This is the best solution for this problem.

### Key Idea

Every number appears twice except one number.

When we XOR all array elements:

- Duplicate numbers cancel each other.
- XOR with zero keeps the remaining number unchanged.
- The number appearing once remains as the final result.

### Java Code

```java
public class SingleNumber {

    public static int singleNumber(int[] nums) {

        int result = 0;

        for (int num : nums) {

            result = result ^ num;
        }

        return result;
    }

    public static void main(String[] args) {

        int[] nums = {4, 1, 2, 1, 2};

        int answer = singleNumber(nums);

        System.out.println("Single Number: " + answer);
    }
}
```

**Output:**

```text
Single Number: 4
```

---

## 7. Dry Run

Let's understand the XOR solution step by step.

**Input:**

```java
nums = [4, 1, 2, 1, 2]
```

Initially:

```java
int result = 0;
```

Now process each element.

| Iteration | Current Number | Operation | Result |
|---|---:|---|---:|
| Initial | - | 0 | 0 |
| 1 | 4 | 0 ^ 4 | 4 |
| 2 | 1 | 4 ^ 1 | 5 |
| 3 | 2 | 5 ^ 2 | 7 |
| 4 | 1 | 7 ^ 1 | 6 |
| 5 | 2 | 6 ^ 2 | 4 |

Final result:

```text
4
```

Therefore, the number appearing only once is `4`.

### Visual Understanding

```text
4 ^ 1 ^ 2 ^ 1 ^ 2

Rearrange:

4 ^ (1 ^ 1) ^ (2 ^ 2)

Since:

1 ^ 1 = 0
2 ^ 2 = 0

Therefore:

4 ^ 0 ^ 0 = 4
```

The duplicate numbers cancel each other.

---

## 8. Complexity Analysis of the Optimal Solution

| Complexity | Value | Explanation |
|---|---|---|
| Time | O(n) | We traverse the array once. |
| Space | O(1) | We use only one variable. |

This solution satisfies both requirements of the problem.

---

## 9. Edge Cases

| Input | Output | Explanation |
|---|---:|---|
| `[1]` | 1 | Only one element |
| `[2, 2, 1]` | 1 | One unique element |
| `[4, 1, 2, 1, 2]` | 4 | Duplicate elements cancel |
| `[-1, -1, -2]` | -2 | XOR also works with negative integers |
| `[0, 1, 1]` | 0 | Zero is the unique element |

---

## 10. Interview Questions

### Q1. What is XOR?

XOR is a bitwise operator that returns `1` when two corresponding bits are different and `0` when they are the same.

### Q2. Why does XOR solve the Single Number problem?

Because XOR has the property:

```java
a ^ a = 0
```

All duplicate numbers cancel each other, leaving the unique number.

### Q3. What is the time complexity?

O(n), because we traverse the array once.

### Q4. What is the space complexity?

O(1), because we use only one additional variable.

### Q5. Does XOR work with negative numbers?

Yes. Java supports bitwise XOR on negative integers using their two's-complement representation.

### Q6. Does this approach work if every number appears three times except one?

No. The simple XOR approach does not work for that variation because:

```java
a ^ a ^ a = a
```

That requires a different bit-counting approach.

---

## 11. Important Things to Remember

- XOR operator in Java: `^`
- `a ^ a = 0`
- `a ^ 0 = a`
- XOR is commutative and associative.
- Duplicate numbers cancel each other.
- XOR is useful in array and bit-manipulation problems.
- Always check the problem constraints before choosing an algorithm.

---

## 12. Practice Problems

Try solving these problems to strengthen your understanding of XOR and bit manipulation.

1. [Missing Number – LeetCode 268](https://leetcode.com/problems/missing-number/)
2. [Single Number II – LeetCode 137](https://leetcode.com/problems/single-number-ii/)
3. [Number of 1 Bits – LeetCode 191](https://leetcode.com/problems/number-of-1-bits/)
4. [Reverse Bits – LeetCode 190](https://leetcode.com/problems/reverse-bits/)

---

## 13. Final Summary

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| HashMap | O(n) average | O(n) |
| XOR | O(n) | O(1) |

**Recommended interview solution:** XOR approach.

```java
public int singleNumber(int[] nums) {

    int result = 0;

    for (int num : nums) {
        result ^= num;
    }

    return result;
}
```

Remember this pattern: **When every element appears twice except one, think about XOR.**
