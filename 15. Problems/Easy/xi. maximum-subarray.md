# Maximum Subarray – Kadane's Algorithm

**Difficulty:** Medium  
**Topics:** Array, Dynamic Programming, Greedy, Kadane's Algorithm  
**LeetCode:** 53  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given an integer array `nums`, find the contiguous subarray (containing at least one number) that has the largest sum, and return its sum.

A subarray is a continuous portion of an array.

### Example 1

**Input:**

```java
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

**Output:**

```text
6
```

**Explanation:**

The maximum-sum subarray is:

```text
[4, -1, 2, 1]
```

Its sum is:

```text
4 + (-1) + 2 + 1 = 6
```

### Example 2

**Input:**

```java
nums = [1]
```

**Output:**

```text
1
```

### Example 3

**Input:**

```java
nums = [5, 4, -1, 7, 8]
```

**Output:**

```text
23
```

**Explanation:**

The entire array has the maximum sum:

```text
5 + 4 - 1 + 7 + 8 = 23
```

---

## 2. Understanding the Problem

We need to find a continuous sequence of elements whose sum is maximum.

Consider:

```text
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Some possible subarrays:

| Subarray | Sum |
|---|---:|
| `[-2]` | -2 |
| `[1]` | 1 |
| `[4]` | 4 |
| `[4, -1]` | 3 |
| `[4, -1, 2]` | 5 |
| `[4, -1, 2, 1]` | 6 |
| `[4, -1, 2, 1, -5]` | 1 |

The maximum sum is `6`.

**Important:** We need a contiguous subarray. We cannot select arbitrary elements from different positions.

---

## 3. Approach 1: Brute Force

### Concept

Generate every possible contiguous subarray and calculate its sum.

Maintain a variable `maxSum` to store the maximum sum found.

### Java Code

```java
public class MaximumSubarrayBruteForce {

    public static int maxSubArray(int[] nums) {

        int maxSum = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++) {

            int currentSum = 0;

            for (int j = i; j < nums.length; j++) {

                currentSum += nums[j];

                maxSum = Math.max(maxSum, currentSum);
            }
        }

        return maxSum;
    }

    public static void main(String[] args) {

        int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

        System.out.println(maxSubArray(nums));
    }
}
```

### Dry Run

Input:

```text
[1, -2, 3, 4]
```

Possible subarrays:

| Starting Index | Subarray | Sum |
|---|---|---:|
| 0 | `[1]` | 1 |
| 0 | `[1, -2]` | -1 |
| 0 | `[1, -2, 3]` | 2 |
| 0 | `[1, -2, 3, 4]` | 6 |
| 1 | `[-2]` | -2 |
| 1 | `[-2, 3]` | 1 |
| 1 | `[-2, 3, 4]` | 5 |
| 2 | `[3]` | 3 |
| 2 | `[3, 4]` | 7 |
| 3 | `[4]` | 4 |

Maximum sum:

```text
7
```

Maximum subarray:

```text
[3, 4]
```

### Complexity Analysis

- **Time Complexity:** O(n²), because we generate all possible starting and ending positions while maintaining the running sum.
- **Space Complexity:** O(1), because we use only a few variables.

This is better than recalculating each subarray sum from scratch, but we can optimize further.

---

## 4. Approach 2: Kadane's Algorithm

### Concept

Kadane's Algorithm finds the maximum subarray sum using a single traversal.

The main idea:

At every element, decide whether to:

1. Extend the previous subarray by adding the current element.
2. Start a new subarray from the current element.

We use two variables:

- `currentSum`: Maximum subarray sum ending at the current position.
- `maxSum`: Maximum subarray sum found so far.

### Important Formula

\[
currentSum = \max(nums[i], currentSum + nums[i])
\]

\[
maxSum = \max(maxSum, currentSum)
\]

### Understanding the Formula

Suppose:

```text
currentSum = 5
current element = -3
```

Option 1: Extend the existing subarray.

```text
5 + (-3) = 2
```

Option 2: Start a new subarray.

```text
-3
```

Choose the larger value:

```text
max(2, -3) = 2
```

Therefore, continuing the existing subarray is better.

Now suppose:

```text
currentSum = -5
current element = 4
```

Option 1:

```text
-5 + 4 = -1
```

Option 2:

```text
4
```

Choose:

```text
max(-1, 4) = 4
```

Starting a new subarray is better.

This is the central idea behind Kadane's Algorithm.

---

## 5. Java Code – Optimized Solution

```java
public class MaximumSubarrayKadane {

    public static int maxSubArray(int[] nums) {

        int currentSum = nums[0];

        int maxSum = nums[0];

        for (int i = 1; i < nums.length; i++) {

            currentSum = Math.max(
                nums[i],
                currentSum + nums[i]
            );

            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }

    public static void main(String[] args) {

        int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

        System.out.println(maxSubArray(nums));
    }
}
```

**Output:**

```text
6
```

This implementation assumes the array contains at least one element, as guaranteed by the standard problem.

---

## 6. Detailed Dry Run

Input:

```text
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Initial values:

```java
currentSum = -2;
maxSum = -2;
```

Now process the remaining elements.

| i | nums[i] | currentSum Calculation | currentSum | maxSum |
|---|---:|---|---:|---:|
| 0 | -2 | Initial value | -2 | -2 |
| 1 | 1 | max(1, -2 + 1) | 1 | 1 |
| 2 | -3 | max(-3, 1 - 3) | -2 | 1 |
| 3 | 4 | max(4, -2 + 4) | 4 | 4 |
| 4 | -1 | max(-1, 4 - 1) | 3 | 4 |
| 5 | 2 | max(2, 3 + 2) | 5 | 5 |
| 6 | 1 | max(1, 5 + 1) | 6 | 6 |
| 7 | -5 | max(-5, 6 - 5) | 1 | 6 |
| 8 | 4 | max(4, 1 + 4) | 5 | 6 |

Final result:

```text
6
```

The maximum subarray is:

```text
[4, -1, 2, 1]
```

Its sum is `6`.

---

## 7. Visual Understanding of Kadane's Algorithm

Consider:

```text
nums = [-2, 1, -3, 4, -1, 2, 1]
```

We calculate the maximum subarray sum ending at each position.

```text
Element:     -2   1  -3   4  -1   2   1

CurrentSum:  -2   1  -2   4   3   5   6

MaxSum:      -2   1   1   4   4   5   6
```

The current sum resets when starting a new subarray gives a better result.

The maximum sum found is:

```text
6
```

---

## 8. Why Do We Initialize with nums[0]?

Consider:

```text
nums = [-5, -2, -8]
```

The correct answer is:

```text
-2
```

If we initialize:

```java
int maxSum = 0;
```

we would incorrectly return `0`, even though no empty subarray is allowed.

Instead, initialize:

```java
int currentSum = nums[0];
int maxSum = nums[0];
```

This correctly handles arrays containing only negative numbers.

### Dry Run

| Element | currentSum | maxSum |
|---:|---:|---:|
| -5 | -5 | -5 |
| -2 | -2 | -2 |
| -8 | -8 | -2 |

Final answer:

```text
-2
```

---

## 9. Approach 3: Kadane's Algorithm – Find the Actual Subarray

Sometimes interviewers ask for the maximum sum and the actual subarray that produces it.

We can maintain:

- `start`: Starting index of the current candidate subarray.
- `bestStart`: Starting index of the best subarray.
- `bestEnd`: Ending index of the best subarray.

### Java Code

```java
public class MaximumSubarrayWithIndices {

    public static void maxSubArray(int[] nums) {

        int currentSum = nums[0];
        int maxSum = nums[0];

        int start = 0;
        int bestStart = 0;
        int bestEnd = 0;

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] > currentSum + nums[i]) {

                currentSum = nums[i];

                start = i;

            } else {

                currentSum += nums[i];
            }

            if (currentSum > maxSum) {

                maxSum = currentSum;

                bestStart = start;

                bestEnd = i;
            }
        }

        System.out.println("Maximum Sum: " + maxSum);

        System.out.println("Start Index: " + bestStart);

        System.out.println("End Index: " + bestEnd);

        System.out.print("Subarray: ");

        for (int i = bestStart; i <= bestEnd; i++) {

            System.out.print(nums[i] + " ");
        }
    }

    public static void main(String[] args) {

        int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

        maxSubArray(nums);
    }
}
```

**Output:**

```text
Maximum Sum: 6
Start Index: 3
End Index: 6
Subarray: 4 -1 2 1
```

### Complexity Analysis

- **Time Complexity:** O(n).
- **Space Complexity:** O(1) auxiliary space.

We only maintain a fixed number of variables.

---

## 10. Comparing Both Approaches

| Comparison | Brute Force | Kadane's Algorithm |
|---|---|---|
| Approach | Generate every subarray | Track the best ending sum |
| Time Complexity | O(n²) | O(n) |
| Space Complexity | O(1) | O(1) |
| Traversals | Nested loops | Single traversal |
| Recommended | For understanding | For interviews |

**Interview recommendation:** Explain the brute-force approach first, then optimize using Kadane's Algorithm.

---

## 11. Edge Cases

| Input | Expected Output | Explanation |
|---|---:|---|
| `[1, 2, 3, 4]` | 10 | Entire array |
| `[-1, -2, -3]` | -1 | Largest negative element |
| `[5]` | 5 | Single element |
| `[0, 0, 0]` | 0 | All zeros |
| `[-2, 1, -3, 4, -1, 2, 1]` | 6 | Maximum subarray |
| `[-2, -1, 5, -3]` | 5 | Best subarray is `[5]` |
| `[4, -1, 2, 1]` | 6 | Entire array |

---

## 12. Common Interview Mistakes

1. Initializing `maxSum` to zero, which fails for all-negative arrays.
2. Confusing a subarray with a subsequence.
3. Forgetting that a subarray must be contiguous.
4. Updating `maxSum` before updating `currentSum`.
5. Using nested loops when O(n) is possible.
6. Assuming that negative numbers always mean the current subarray should be discarded.

---

## 13. Interview Questions

**Q1. What is Kadane's Algorithm?**

It is an algorithm used to find the maximum sum of a contiguous subarray in O(n) time.

**Q2. What is the time complexity?**

O(n), because we traverse the array once.

**Q3. What is the space complexity?**

O(1), because we use only a fixed number of variables.

**Q4. Why do we use Math.max()?**

To decide whether extending the existing subarray or starting a new subarray produces a better sum.

**Q5. Why do we initialize maxSum with nums[0]?**

To correctly handle arrays containing only negative values.

**Q6. Can Kadane's Algorithm work with negative numbers?**

Yes. It correctly handles positive, negative, and zero values.

**Q7. What is the difference between a subarray and a subsequence?**

- Subarray: Elements must be contiguous.
- Subsequence: Elements maintain their relative order but do not need to be contiguous.

**Q8. Is Kadane's Algorithm related to Dynamic Programming?**

Yes. It uses the previous optimal result to calculate the best subarray sum ending at the current position.

---

## 14. Practice Tasks

Try implementing these variations:

- [ ] Find the maximum subarray and return its starting and ending indices.
- [ ] Find the minimum subarray sum.
- [ ] Find the maximum sum of a circular subarray.
- [ ] Find the maximum product subarray.
- [ ] Find the maximum sum of a subarray of fixed size `k`.
- [ ] Find the longest subarray with a positive sum, under suitable input constraints.

---

## 15. Key Takeaways

- Maximum Subarray is a classic array optimization problem.
- Brute force checks all possible subarrays.
- Kadane's Algorithm reduces the time complexity to O(n).
- `currentSum` tracks the best sum ending at the current index.
- `maxSum` tracks the best sum found anywhere.
- Initialize with the first array element to handle all-negative arrays.
- The same algorithm can be extended to return the actual subarray.

**Most important pattern to remember:**

```java
int currentSum = nums[0];
int maxSum = nums[0];

for (int i = 1; i < nums.length; i++) {

    currentSum = Math.max(
        nums[i],
        currentSum + nums[i]
    );

    maxSum = Math.max(maxSum, currentSum);
}

return maxSum;
```

This Kadane's Algorithm pattern is frequently used in array optimization and dynamic programming interview problems.