# Plus One

**Difficulty:** Easy  
**LeetCode:** [66. Plus One](https://leetcode.com/problems/plus-one/)  
**Topics:** Arrays, Math, Digit Manipulation, Carry

---

## 1. Problem Statement

You are given a large integer represented as an integer array `digits`, where each `digits[i]` represents one digit of the integer.

The digits are ordered from most significant to least significant, from left to right.

Increment the large integer by one and return the resulting array of digits.

**Important:**
- The integer does not contain any leading zeroes.
- The number may be too large to fit into a standard integer data type.
- You must handle carry operations correctly.

### Example 1

**Input:**

```java
digits = [1, 2, 3]
```

**Output:**

```java
[1, 2, 4]
```

**Explanation:**

123 + 1 = 124

### Example 2

**Input:**

```java
digits = [4, 3, 9]
```

**Output:**

```java
[4, 4, 0]
```

**Explanation:**

439 + 1 = 440

### Example 3

**Input:**

```java
digits = [9, 9, 9]
```

**Output:**

```java
[1, 0, 0, 0]
```

**Explanation:**

999 + 1 = 1000

---

## 2. Understanding the Problem

The array represents a number, with each element representing one digit.

For example:

```text
digits = [1, 2, 3]

Number = 123

After adding 1:

123 + 1 = 124

Result = [1, 2, 4]
```

The main challenge is handling the carry when a digit becomes 10.

For example:

```text
digits = [1, 2, 9]

Number = 129

129 + 1 = 130

Result = [1, 3, 0]
```

When adding one:
- 9 + 1 = 10
- Write 0 in the current position.
- Carry 1 to the previous position.

---

## 3. Approach 1: Convert Array to Integer

One possible approach is to convert the digit array into a number, add one, and convert it back into an array.

### Java Code

```java
import java.util.Arrays;

public class PlusOneConversion {

    public static int[] plusOne(int[] digits) {

        long number = 0;

        // Convert array into a number
        for (int digit : digits) {
            number = number * 10 + digit;
        }

        // Add one
        number++;

        // Convert number back into an array
        String value = String.valueOf(number);

        int[] result = new int[value.length()];

        for (int i = 0; i < value.length(); i++) {
            result[i] = value.charAt(i) - '0';
        }

        return result;
    }

    public static void main(String[] args) {

        int[] digits = {1, 2, 3};

        System.out.println(
            Arrays.toString(plusOne(digits))
        );
    }
}
```

**Output:**

```text
[1, 2, 4]
```

### Why Is This Approach Not Recommended?

The problem specifically mentions that the integer can be very large.

Even `long` has a maximum value.

```java
long maxValue = Long.MAX_VALUE;
```

If the input contains many digits, converting it into a `long` can cause overflow.

Therefore, we should perform the addition directly on the array.

---

## 4. Approach 2: Optimal Solution Using Carry

This is the recommended interview solution.

### Main Idea

Start from the last digit because the rightmost digit represents the units position.

There are three possible situations:

**Case 1: Current digit is less than 9**

Increase it by one and return the array.

Example:

```text
digits = [1, 2, 3]

Last digit = 3

3 + 1 = 4

Result = [1, 2, 4]
```

**Case 2: Current digit is 9**

Change it to 0 and carry one to the previous digit.

Example:

```text
digits = [1, 2, 9]

9 + 1 = 10

Change 9 to 0.
Carry 1 to the previous digit.

Result = [1, 3, 0]
```

**Case 3: Every digit is 9**

All digits become zero, and we need an additional digit at the beginning.

Example:

```text
digits = [9, 9, 9]

999 + 1 = 1000

Result = [1, 0, 0, 0]
```

### Java Code

```java
import java.util.Arrays;

public class PlusOne {

    public static int[] plusOne(int[] digits) {

        // Traverse from right to left
        for (int i = digits.length - 1; i >= 0; i--) {

            // If digit is less than 9
            if (digits[i] < 9) {

                digits[i]++;

                return digits;
            }

            // If digit is 9, make it 0
            digits[i] = 0;
        }

        // If all digits were 9,
        // create a new array with one extra position
        int[] result = new int[digits.length + 1];

        result[0] = 1;

        return result;
    }

    public static void main(String[] args) {

        int[] digits = {1, 2, 9};

        int[] result = plusOne(digits);

        System.out.println(Arrays.toString(result));
    }
}
```

**Output:**

```text
[1, 3, 0]
```

---

## 5. Detailed Dry Run

Let's understand the optimal solution step by step.

### Example 1: No Carry Required

**Input:**

```text
digits = [1, 2, 3]
```

Start from the last index.

```text
i = 2
digits[2] = 3
```

Since `3 < 9`:

```java
digits[2]++;
```

Array becomes:

```text
[1, 2, 4]
```

Return the array.

**Final Output:**

```text
[1, 2, 4]
```

### Example 2: Carry Required

**Input:**

```text
digits = [1, 2, 9]
```

Initial array:

```text
[1, 2, 9]
```

**Iteration 1**

```text
i = 2
digits[2] = 9
```

Since the digit is 9:

```java
digits[2] = 0;
```

Array:

```text
[1, 2, 0]
```

Move to the previous index.

**Iteration 2**

```text
i = 1
digits[1] = 2
```

Since `2 < 9`:

```java
digits[1]++;
```

Array becomes:

```text
[1, 3, 0]
```

Return the array.

**Final Output:**

```text
[1, 3, 0]
```

### Example 3: All Digits Are 9

**Input:**

```text
digits = [9, 9, 9]
```

**Iteration 1**

```text
i = 2
digits[2] = 9
```

Change it to zero:

```text
[9, 9, 0]
```

**Iteration 2**

```text
i = 1
digits[1] = 9
```

Change it to zero:

```text
[9, 0, 0]
```

**Iteration 3**

```text
i = 0
digits[0] = 9
```

Change it to zero:

```text
[0, 0, 0]
```

The loop finishes without returning because every digit was 9.

Now create a new array:

```java
int[] result = new int[digits.length + 1];
```

Its initial value is:

```text
[0, 0, 0, 0]
```

Set the first element to 1:

```java
result[0] = 1;
```

Final array:

```text
[1, 0, 0, 0]
```

**Final Output:**

```text
[1, 0, 0, 0]
```

---

## 6. Dry Run Table

Input:

```text
digits = [1, 2, 9]
```

| Iteration | Index | Current Digit | Action | Array |
|---|---:|---:|---|---|
| Initial | - | - | Original array | `[1,2,9]` |
| 1 | 2 | 9 | Change to 0 | `[1,2,0]` |
| 2 | 1 | 2 | Increase by 1 | `[1,3,0]` |

Final output:

```text
[1, 3, 0]
```

---

## 7. Why Do We Traverse From Right to Left?

Consider the number:

```text
123
```

When we add one:

```text
123 + 1 = 124
```

Only the last digit changes.

Now consider:

```text
129 + 1 = 130
```

The last digit becomes zero, and the carry moves to the previous digit.

Therefore, we should start from the rightmost digit.

This follows normal addition rules:

```text
    129
  +   1
  -----
    130
```

---

## 8. Why Do We Return Immediately When a Digit Is Less Than 9?

Consider:

```text
digits = [1, 2, 3]
```

The last digit is 3.

After adding one:

```text
[1, 2, 4]
```

There is no carry.

All digits before the current position remain unchanged.

Therefore, there is no need to continue traversing the array.

We can immediately return the result.

---

## 9. Complexity Analysis

Let `n` be the length of the input array.

| Complexity | Value | Explanation |
|---|---|---|
| Time | O(n) | In the worst case, we traverse every digit. |
| Extra Space | O(1) | We modify the original array directly. |
| Worst-case output allocation | O(n) | When every digit is 9, we create a new array of size n + 1. |

The algorithm is optimal because we may need to inspect every digit in the worst case.

---

## 10. Edge Cases

| Input | Output | Explanation |
|---|---|---|
| `[1, 2, 3]` | `[1, 2, 4]` | No carry |
| `[1, 2, 9]` | `[1, 3, 0]` | Carry to previous digit |
| `[9, 9, 9]` | `[1, 0, 0, 0]` | All digits are 9 |
| `[0]` | `[1]` | Single zero digit |
| `[9]` | `[1, 0]` | Single digit with carry |
| `[8, 9, 9]` | `[9, 0, 0]` | Carry through multiple digits |
| `[1, 9, 9, 9]` | `[2, 0, 0, 0]` | Carry reaches first digit |

---

## 11. Interview Questions

### Q1. Why should we not convert the array into an integer?

The number may contain too many digits to fit into `int` or `long`, causing overflow.

### Q2. Why do we start from the last index?

Because addition starts from the units digit, which is the rightmost digit.

### Q3. What happens when a digit is 9?

It becomes zero, and the carry moves to the previous digit.

### Q4. Why do we create a new array when all digits are 9?

Because adding one increases the total number of digits.

For example:

```text
999 + 1 = 1000
```

The output requires one extra position.

### Q5. What is the time complexity?

O(n), where `n` is the number of digits.

### Q6. What is the extra space complexity?

O(1) in the normal case. In the all-9 case, a new output array of size `n + 1` is allocated.

### Q7. Can we solve this problem using recursion?

Yes. We can recursively process digits from right to left, but the iterative solution is simpler and avoids recursion stack usage.

---

## 12. Practice Problems

Try these problems to improve your understanding of digit manipulation and carry handling.

1. [Add Binary – LeetCode 67](https://leetcode.com/problems/add-binary/)
2. [Add Strings – LeetCode 415](https://leetcode.com/problems/add-strings/)
3. [Multiply Strings – LeetCode 43](https://leetcode.com/problems/multiply-strings/)
4. [Add Two Numbers – LeetCode 2](https://leetcode.com/problems/add-two-numbers/)

---

## 13. Final Summary

We learned two approaches:

| Approach | Main Idea | Limitation |
|---|---|---|
| Integer Conversion | Convert array to number and add one | Can overflow |
| Carry-Based Approach | Process digits from right to left | None for the given constraints |

### Recommended Interview Solution

```java
public int[] plusOne(int[] digits) {

    for (int i = digits.length - 1; i >= 0; i--) {

        if (digits[i] < 9) {

            digits[i]++;

            return digits;
        }

        digits[i] = 0;
    }

    int[] result = new int[digits.length + 1];

    result[0] = 1;

    return result;
}
```

**Remember:**
- Start from the last digit.
- If the digit is less than 9, increment and return.
- If the digit is 9, change it to zero and continue.
- If all digits are 9, create a new array with one extra position.

**Important interview concept:** Never convert a very large digit array into a primitive integer when you can perform arithmetic directly on the digits.
