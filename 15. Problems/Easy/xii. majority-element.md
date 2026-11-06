# Majority Element

**Difficulty:** Easy (LeetCode)  
**LeetCode:** [169. Majority Element](https://leetcode.com/problems/majority-element/)  
**Topics:** Arrays, HashMap, Boyer-Moore Voting Algorithm

---

## 1. Problem Statement

Given an array `nums` of size `n`, return the majority element.

The majority element is the element that appears more than `n / 2` times.

You may assume that the majority element always exists in the array.

### Example 1

**Input:**
```java
nums = [3, 2, 3]
```

**Output:**
```java
3
```

**Explanation:** The number `3` appears 2 times out of 3 elements.

### Example 2

**Input:**
```java
nums = [2, 2, 1, 1, 1, 2, 2]
```

**Output:**
```java
2
```

**Explanation:** The number `2` appears 4 times out of 7 elements.

---

## 2. Understanding the Problem

We need to find the element that appears more than half the size of the array.

For example:

```text
Array = [2, 2, 1, 1, 1, 2, 2]

Array size = 7
Half = 7 / 2 = 3

Frequency:
2 → 4 times
1 → 3 times

Majority element = 2
```

Important condition:

```text
Frequency of majority element > n / 2
```

Since the majority element is guaranteed to exist, we don't need to handle the case where no majority element exists.

---

## 3. Approach 1: Brute Force

Check each element and count how many times it appears.

If its frequency is greater than `n / 2`, return it.

### Java Code

```java
public class MajorityElementBruteForce {

    public static int majorityElement(int[] nums) {

        int n = nums.length;

        for (int i = 0; i < n; i++) {

            int count = 0;

            for (int j = 0; j < n; j++) {

                if (nums[i] == nums[j]) {
                    count++;
                }
            }

            if (count > n / 2) {
                return nums[i];
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {2, 2, 1, 1, 1, 2, 2};

        System.out.println(majorityElement(nums));
    }
}
```

**Output:**

```text
2
```

### Complexity Analysis

- Time Complexity: O(n²), because we use nested loops.
- Space Complexity: O(1), because we use only a counter.

This approach is easy to understand but inefficient for large arrays.

---

## 4. Approach 2: Using HashMap

We can use a HashMap to store the frequency of each element.

### Logic

1. Create a HashMap.
2. Traverse the array.
3. Increase the frequency of each element.
4. Check whether its frequency is greater than `n / 2`.
5. Return that element.

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

public class MajorityElementHashMap {

    public static int majorityElement(int[] nums) {

        Map<Integer, Integer> frequency = new HashMap<>();

        int n = nums.length;

        for (int num : nums) {

            int count = frequency.getOrDefault(num, 0) + 1;

            frequency.put(num, count);

            if (count > n / 2) {
                return num;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {2, 2, 1, 1, 1, 2, 2};

        System.out.println(majorityElement(nums));
    }
}
```

**Output:**

```text
2
```

### Complexity Analysis

- Time Complexity: O(n) average.
- Space Complexity: O(n), because the HashMap can store up to n distinct elements.

This is efficient, but it uses extra memory.

---

## 5. Approach 3: Sorting

Another simple approach is to sort the array.

Since the majority element appears more than half the time, it must occupy the middle position after sorting.

### Example

```text
Original:
[2, 2, 1, 1, 1, 2, 2]

After sorting:
[1, 1, 1, 2, 2, 2, 2]

Middle index = 7 / 2 = 3

Element at index 3 = 2
```

### Java Code

```java
import java.util.Arrays;

public class MajorityElementSorting {

    public static int majorityElement(int[] nums) {

        Arrays.sort(nums);

        return nums[nums.length / 2];
    }

    public static void main(String[] args) {

        int[] nums = {2, 2, 1, 1, 1, 2, 2};

        System.out.println(majorityElement(nums));
    }
}
```

**Output:**

```text
2
```

### Complexity Analysis

- Time Complexity: O(n log n), due to sorting.
- Space Complexity: Depends on the sorting implementation; Java's primitive array sorting may use additional workspace.

**Note:** This solution modifies the original array.

---

## 6. Approach 4: Boyer-Moore Voting Algorithm (Optimal)

This is the most important approach for this problem.

The Boyer-Moore Voting Algorithm finds the majority element in:

- O(n) time
- O(1) extra space

### Main Idea

Imagine every occurrence of a non-majority element cancels one occurrence of the majority element.

Because the majority element appears more than half the time, it cannot be completely cancelled.

We maintain two variables:

```java
int candidate = 0;
int count = 0;
```

- `candidate`: Stores the current possible majority element.
- `count`: Tracks its current voting balance.

### Algorithm Rules

For every number in the array:

**Rule 1:** If `count == 0`, select the current number as the candidate.

**Rule 2:** If the current number equals the candidate, increase the count.

**Rule 3:** Otherwise, decrease the count.

At the end, the candidate is the majority element because the problem guarantees that one exists.

### Java Code

```java
public class MajorityElement {

    public static int majorityElement(int[] nums) {

        int candidate = 0;
        int count = 0;

        for (int num : nums) {

            if (count == 0) {
                candidate = num;
            }

            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }

    public static void main(String[] args) {

        int[] nums = {2, 2, 1, 1, 1, 2, 2};

        int answer = majorityElement(nums);

        System.out.println("Majority Element: " + answer);
    }
}
```

**Output:**

```text
Majority Element: 2
```

---

## 7. Detailed Dry Run

Let's understand the Boyer-Moore Voting Algorithm step by step.

**Input:**

```java
nums = [2, 2, 1, 1, 1, 2, 2]
```

Initial values:

```java
candidate = 0;
count = 0;
```

### Iteration-by-Iteration Explanation

| Iteration | Current Number | Candidate | Count | Explanation |
|---|---:|---:|---:|---|
| Initial | - | 0 | 0 | Initial state |
| 1 | 2 | 2 | 1 | Count is 0, select 2; increase count |
| 2 | 2 | 2 | 2 | Same as candidate, increase count |
| 3 | 1 | 2 | 1 | Different number, decrease count |
| 4 | 1 | 2 | 0 | Different number, decrease count |
| 5 | 1 | 1 | 1 | Count is 0, select 1; increase count |
| 6 | 2 | 1 | 0 | Different number, decrease count |
| 7 | 2 | 2 | 1 | Count is 0, select 2; increase count |

Final values:

```text
candidate = 2
count = 1
```

Final answer:

```text
2
```

### Visual Understanding

The algorithm works by cancelling different elements against each other.

For example:

```text
[2, 2, 1, 1, 1, 2, 2]

Cancel pairs of different elements:

2 cancels 1
2 cancels 1

Remaining:
1, 2, 2

2 is still the majority candidate.
```

The important observation is that a majority element has more occurrences than all other elements combined. Therefore, cancelling different pairs cannot eliminate it.

---

## 8. Why Does Boyer-Moore Work?

Suppose the array has:

```text
n = 7

Majority element = 2
Frequency of 2 = 4

Other elements = 3
```

Even if every other element cancels one occurrence of `2`, one occurrence of `2` will remain.

In general:

```text
Majority frequency > n / 2

Majority frequency > all other elements combined
```

Therefore, the majority element survives the cancellation process.

---

## 9. Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| HashMap | O(n) average | O(n) |
| Sorting | O(n log n) | Depends on sorting implementation |
| Boyer-Moore | O(n) | O(1) |

**Best approach:** Boyer-Moore Voting Algorithm.

---

## 10. Important Note: What If Majority Is Not Guaranteed?

The original LeetCode problem guarantees that a majority element exists.

If a different problem does not guarantee this, we must verify the candidate.

### Java Code With Verification

```java
public class MajorityElementVerify {

    public static int majorityElement(int[] nums) {

        int candidate = 0;
        int count = 0;

        // First pass: Find a candidate
        for (int num : nums) {

            if (count == 0) {
                candidate = num;
            }

            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        // Second pass: Verify the candidate
        int frequency = 0;

        for (int num : nums) {

            if (num == candidate) {
                frequency++;
            }
        }

        if (frequency > nums.length / 2) {
            return candidate;
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] nums = {1, 2, 3, 2, 1};

        System.out.println(majorityElement(nums));
    }
}
```

**Output:**

```text
-1
```

Here, no element appears more than half the array size.

Complexity:

- Time: O(n), because we make two passes.
- Space: O(1).

---

## 11. Edge Cases

| Input | Output | Explanation |
|---|---:|---|
| `[1]` | 1 | Only one element |
| `[2, 2, 1]` | 2 | 2 appears more than half |
| `[3, 3, 3, 2, 2]` | 3 | 3 is the majority |
| `[1, 2, 1, 1, 2, 1, 1]` | 1 | 1 appears 5 times |
| `[-1, -1, -1, 2, 2]` | -1 | Negative numbers are supported |

---

## 12. Interview Questions

### Q1. What is the Boyer-Moore Voting Algorithm?

It is an algorithm used to find the majority element in an array using O(n) time and O(1) extra space.

### Q2. Why do we initialize count to zero?

Because we do not have a candidate initially. When the count reaches zero, the next element becomes the new candidate.

### Q3. Why do we increase count?

When the current element matches the candidate, it adds one vote.

### Q4. Why do we decrease count?

When the current element differs from the candidate, it cancels one vote.

### Q5. Why does the algorithm work?

The majority element appears more than all other elements combined. Therefore, it cannot be completely cancelled by different elements.

### Q6. Does the algorithm work when there is no majority element?

It still produces a candidate, but that candidate may not be a true majority element. We need a second pass to verify its frequency.

### Q7. What is the time and space complexity?

- Time: O(n)
- Extra space: O(1)

### Q8. Can Boyer-Moore find elements appearing more than n/3 times?

Yes, but the algorithm needs to track two candidates and their counts, followed by a verification pass.

---

## 13. Practice Problems

Try these problems to strengthen your understanding.

1. [Majority Element II – LeetCode 229](https://leetcode.com/problems/majority-element-ii/)
2. [Top K Frequent Elements – LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)
3. [Find All Numbers Disappeared in an Array – LeetCode 448](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
4. [Find the Duplicate Number – LeetCode 287](https://leetcode.com/problems/find-the-duplicate-number/)

---

## 14. Final Summary

The Majority Element problem can be solved using different approaches.

The most efficient solution is the Boyer-Moore Voting Algorithm.

Remember these three rules:

1. If `count == 0`, choose the current number as the candidate.
2. If the current number equals the candidate, increase the count.
3. Otherwise, decrease the count.

### Optimal Java Solution

```java
public int majorityElement(int[] nums) {

    int candidate = 0;
    int count = 0;

    for (int num : nums) {

        if (count == 0) {
            candidate = num;
        }

        if (num == candidate) {
            count++;
        } else {
            count--;
        }
    }

    return candidate;
}
```

**Remember:** When an array contains an element appearing more than n/2 times, think about the Boyer-Moore Voting Algorithm.
