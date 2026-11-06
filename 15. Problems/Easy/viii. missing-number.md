# Missing Number

**Difficulty:** Easy  
**Topics:** Array, Math, XOR, HashSet  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in that range that is missing from the array.

### Example 1

**Input:**
```java
nums = [3, 0, 1]
```

**Output:**
```java
2
```

**Explanation:**

The numbers in the range `[0, 3]` are:

```text
0, 1, 2, 3
```

The number `2` is missing.

### Example 2

**Input:**
```java
nums = [0, 1]
```

**Output:**
```java
2
```

**Explanation:**

The numbers in the range `[0, 2]` are:

```text
0, 1, 2
```

The number `2` is missing.

### Example 3

**Input:**
```java
nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]
```

**Output:**
```java
8
```

**Explanation:**

The number `8` is missing from the range `[0, 9]`.

---

## 2. Understanding the Problem

The important conditions are:

1. The array contains `n` elements.
2. All elements are distinct.
3. The numbers belong to the range `0` to `n`.
4. Exactly one number is missing.

For example:

```java
nums = [3, 0, 1]
```

Array length:

```java
n = 3;
```

Expected numbers:

```text
0, 1, 2, 3
```

Actual numbers:

```text
3, 0, 1
```

Missing number:

```text
2
```

---

## 3. Approach 1: Brute Force

### Concept

Check every number from `0` to `n`.

For each number, search whether it exists in the array.

If a number is not found, return it.

### Java Code

```java
public class MissingNumberBruteForce {

    public static int missingNumber(int[] nums) {

        int n = nums.length;

        for (int number = 0; number <= n; number++) {

            boolean found = false;

            for (int i = 0; i < nums.length; i++) {

                if (nums[i] == number) {
                    found = true;
                    break;
                }
            }

            if (!found) {
                return number;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {3, 0, 1};

        System.out.println(missingNumber(nums));
    }
}
```

### Dry Run

Input:

```java
nums = [3, 0, 1]
```

Expected range:

```text
0, 1, 2, 3
```

| Number | Found in Array? | Action |
|---:|---|---|
| 0 | Yes | Continue |
| 1 | Yes | Continue |
| 2 | No | Return 2 |

Output:

```text
2
```

### Complexity Analysis

- **Time Complexity:** O(n²), because each expected number may require a complete array traversal.
- **Space Complexity:** O(1).

This approach works but is inefficient.

---

## 4. Approach 2: Mathematical Formula

### Concept

We can calculate the expected sum of numbers from `0` to `n`.

The formula is:

\[
Sum = \frac{n(n+1)}{2}
\]

Then calculate the actual sum of the array.

Finally:

\[
MissingNumber = ExpectedSum - ActualSum
\]

### Example

```java
nums = [3, 0, 1]
```

Array length:

```java
n = 3;
```

Expected sum:

\[
\frac{3(3+1)}{2} = 6
\]

Actual sum:

\[
3 + 0 + 1 = 4
\]

Missing number:

\[
6 - 4 = 2
\]

### Java Code

```java
public class MissingNumberMath {

    public static int missingNumber(int[] nums) {

        long n = nums.length;

        long expectedSum = n * (n + 1) / 2;

        long actualSum = 0;

        for (int num : nums) {
            actualSum += num;
        }

        return (int) (expectedSum - actualSum);
    }

    public static void main(String[] args) {

        int[] nums = {3, 0, 1};

        System.out.println(missingNumber(nums));
    }
}
```

### Detailed Dry Run

Input:

```java
nums = [3, 0, 1]
```

Initial values:

```java
n = 3;
actualSum = 0;
```

Calculate expected sum:

```java
expectedSum = 3 * (3 + 1) / 2;

expectedSum = 6;
```

Calculate actual sum:

| Iteration | Element | Actual Sum |
|---|---:|---:|
| 1 | 3 | 3 |
| 2 | 0 | 3 |
| 3 | 1 | 4 |

Calculate missing number:

```java
missingNumber = 6 - 4;

missingNumber = 2;
```

Output:

```text
2
```

### Complexity Analysis

- **Time Complexity:** O(n), because we traverse the array once.
- **Space Complexity:** O(1), because we use only a few variables.

**Important:** Use `long` for the sum calculation to avoid integer overflow for larger constraints.

---

## 5. Approach 3: Using XOR

### Concept

XOR is a bitwise operator represented by:

```java
^
```

Important XOR properties:

| Operation | Result |
|---|---:|
| `a ^ a` | 0 |
| `a ^ 0` | a |
| `a ^ b ^ a` | b |

The most important property is:

\[
a \oplus a = 0
\]

When the same numbers are XORed together, they cancel each other.

We can use this property to find the missing number.

### Algorithm

1. Initialize `result = 0`.
2. XOR all numbers from `0` to `n`.
3. XOR all elements of the given array.
4. Duplicate values cancel each other.
5. The missing number remains.

### Java Code

```java
public class MissingNumberXOR {

    public static int missingNumber(int[] nums) {

        int result = nums.length;

        for (int i = 0; i < nums.length; i++) {

            result ^= i;

            result ^= nums[i];
        }

        return result;
    }

    public static void main(String[] args) {

        int[] nums = {3, 0, 1};

        System.out.println(missingNumber(nums));
    }
}
```

### Detailed Dry Run

Input:

```java
nums = [3, 0, 1]
```

Array length:

```java
n = 3;
```

Initially:

```java
result = 3;
```

The algorithm performs these operations:

| Iteration | Operation | Result |
|---|---|---:|
| Initial | `result = 3` | 3 |
| 1 | `3 ^ 0` | 3 |
| 1 | `3 ^ 3` | 0 |
| 2 | `0 ^ 1` | 1 |
| 2 | `1 ^ 0` | 1 |
| 3 | `1 ^ 2` | 3 |
| 3 | `3 ^ 1` | 2 |

Final result:

```text
2
```

### Why Does XOR Work?

We are effectively calculating:

```text
0 ^ 1 ^ 2 ^ 3
^ 3 ^ 0 ^ 1
```

Rearrange the values:

```text
(0 ^ 0) ^ (1 ^ 1) ^ (3 ^ 3) ^ 2
```

Using the XOR property:

```text
0 ^ 0 ^ 0 ^ 2
```

Therefore:

```text
2
```

The missing number remains.

### Complexity Analysis

- **Time Complexity:** O(n).
- **Space Complexity:** O(1).

---

## 6. Approach 4: Using HashSet

### Concept

A HashSet stores unique elements and provides average O(1) lookup.

We can:

1. Add all array elements to a HashSet.
2. Check every number from `0` to `n`.
3. Return the first number that does not exist in the set.

### Java Code

```java
import java.util.HashSet;
import java.util.Set;

public class MissingNumberHashSet {

    public static int missingNumber(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {
            set.add(num);
        }

        for (int i = 0; i <= nums.length; i++) {

            if (!set.contains(i)) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {3, 0, 1};

        System.out.println(missingNumber(nums));
    }
}
```

### Dry Run

Input:

```java
nums = [3, 0, 1]
```

After inserting elements:

```text
HashSet = {0, 1, 3}
```

Now check:

| Number | Exists in Set? | Action |
|---:|---|---|
| 0 | Yes | Continue |
| 1 | Yes | Continue |
| 2 | No | Return 2 |

Output:

```text
2
```

### Complexity Analysis

- **Time Complexity:** O(n) average.
- **Space Complexity:** O(n).

---

## 7. Compare All Four Approaches

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| Mathematical Formula | O(n) | O(1) |
| XOR | O(n) | O(1) |
| HashSet | O(n) average | O(n) |

### Which Approach Is Best?

Both the mathematical formula and XOR approaches provide:

- O(n) time complexity.
- O(1) auxiliary space.

**Interview recommendation:**

Learn both mathematical and XOR approaches.

The mathematical approach is easier to understand, while XOR is an important bit manipulation technique that appears in many other DSA problems.

---

## 8. Edge Cases

| Input | Expected Output | Explanation |
|---|---:|---|
| `[3, 0, 1]` | 2 | Missing middle number |
| `[0, 1]` | 2 | Missing last number |
| `[1]` | 0 | Missing first number |
| `[0]` | 1 | Missing last number |
| `[9, 6, 4, 2, 3, 5, 7, 0, 1]` | 8 | Missing number in between |
| `[]` | 0 | Only possible number is 0 |

---

## 9. Common Interview Mistakes

1. Assuming the array is sorted. The problem does not guarantee sorted order.
2. Forgetting that the range starts at `0`.
3. Using an `int` for a potentially large expected sum.
4. Confusing array length `n` with the largest possible number `n`.
5. Forgetting that XOR works because all numbers are distinct and exactly one number is missing.

---

## 10. Interview Questions

**Q1. Why does the mathematical formula work?**

Because the sum formula calculates the total of all numbers from `0` to `n`. Subtracting the actual array sum leaves the missing number.

**Q2. What is the formula for the sum of the first n natural numbers?**

\[
Sum = \frac{n(n+1)}{2}
\]

For this problem, the sequence starts at zero, so the same formula gives the sum from `0` through `n`.

**Q3. Why does XOR work?**

Because XORing a number with itself produces zero. Every number present in both the expected range and array cancels, leaving the missing number.

**Q4. What is the time complexity of the XOR solution?**

O(n), because we process each array element once.

**Q5. What is the space complexity of the XOR solution?**

O(1), because we only use a fixed number of variables.

**Q6. Does the XOR approach require a sorted array?**

No. XOR is independent of element order.

**Q7. Can we use HashSet?**

Yes. Store all elements and check which number from `0` to `n` is missing. This requires O(n) extra space.

---

## 11. Practice Tasks

Try implementing these variations:

- [ ] Find the missing number using sorting.
- [ ] Find two missing numbers from a given range.
- [ ] Find the duplicate number in an array.
- [ ] Find the number that appears only once when every other number appears twice.
- [ ] Find the missing number from an array starting at `1`.
- [ ] Solve the missing-number problem using XOR without looking at the solution.

---

## 12. Key Takeaways

- The array contains `n` distinct numbers from the range `0` to `n`.
- The mathematical formula calculates the expected sum.
- Subtracting the actual sum gives the missing number.
- XOR cancels matching numbers.
- Both mathematical and XOR solutions use O(n) time and O(1) auxiliary space.
- HashSet is another easy-to-understand approach but requires O(n) space.

**Most important patterns to remember:**

Mathematical approach:

```java
long n = nums.length;

long expectedSum = n * (n + 1) / 2;

long actualSum = 0;

for (int num : nums) {
    actualSum += num;
}

return (int) (expectedSum - actualSum);
```

XOR approach:

```java
int result = nums.length;

for (int i = 0; i < nums.length; i++) {

    result ^= i;

    result ^= nums[i];
}

return result;
```

These mathematical and XOR patterns are useful in many array and bit manipulation interview problems.