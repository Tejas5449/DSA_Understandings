# Sliding Window

Sliding Window is one of the most important techniques for solving **subarray** and **substring** problems efficiently.

It is especially useful when a problem asks us to find:

- Maximum/minimum sum of a subarray
- Longest/shortest subarray
- Longest/shortest substring
- Number of elements satisfying a condition
- A continuous range with some property
- A window of fixed size `k`
- A window whose size changes based on a condition

The main idea is:

> Instead of recalculating every subarray from scratch, maintain a "window" and move it across the array/string.

---

# 1. What is a Sliding Window?

Consider this array:

```text
[2, 1, 5, 1, 3, 2]
```

Suppose we need the **maximum sum of 3 consecutive elements**.

We can look at windows of size `3`:

```text
[2, 1, 5]  1  3  2
  ↑─────↑

  2  [1, 5, 1]  3  2
     ↑─────↑

  2  1  [5, 1, 3]  2
        ↑─────↑

  2  1  5  [1, 3, 2]
           ↑─────↑
```

Instead of calculating each sum independently, we maintain the current window.

For example:

```text
First window:

2 + 1 + 5 = 8
```

Move the window one position:

```text
[2, 1, 5]
     ↓ remove 2
     ↓ add 1

[1, 5, 1]

8 - 2 + 1 = 7
```

Move again:

```text
[1, 5, 1]

7 - 1 + 3 = 9
```

And so on.

This is the core idea of Sliding Window.

---

# 2. Why Do We Need Sliding Window?

Suppose:

```text
arr = [2, 1, 5, 1, 3, 2]
k = 3
```

We want the maximum sum of `3` consecutive elements.

## Brute Force

We could calculate every window separately.

```text
2 + 1 + 5 = 8
1 + 5 + 1 = 7
5 + 1 + 3 = 9
1 + 3 + 2 = 6
```

For every window, we calculate `k` elements.

Time complexity:

```text
O(n * k)
```

If:

```text
n = 1,000,000
k = 500,000
```

this becomes extremely expensive.

Sliding Window allows us to reuse the previous window.

Time complexity:

```text
O(n)
```

---

# 3. Important Terms

Before learning Sliding Window, understand these terms.

## Subarray

A subarray is a **continuous portion** of an array.

Example:

```text
[1, 2, 3, 4, 5]
```

Valid subarrays:

```text
[1, 2]
[2, 3, 4]
[3]
[4, 5]
[1, 2, 3, 4, 5]
```

Invalid:

```text
[1, 3]
```

because `1` and `3` are not continuous.

---

## Substring

A substring is a continuous portion of a string.

Example:

```text
"abcdef"
```

Valid substrings:

```text
"abc"
"bcd"
"def"
"cd"
```

Invalid:

```text
"ace"
```

because the characters are not continuous.

---

# 4. Two Types of Sliding Window

There are mainly two types:

```text
1. Fixed-size Sliding Window
2. Variable-size Sliding Window
```

---

# 5. Fixed-Size Sliding Window

In a fixed-size window, the window always has the same size.

For example:

```text
k = 3
```

The window always contains exactly `3` elements.

Example:

```text
[2, 1, 5, 1, 3, 2]
```

Windows:

```text
[2, 1, 5]
   [1, 5, 1]
      [5, 1, 3]
         [1, 3, 2]
```

The size is always `3`.

---

# 6. Example: Maximum Sum of Subarray of Size K

Problem:

> Given an integer array and an integer `k`, find the maximum sum of any contiguous subarray of size `k`.

Example:

```text
arr = [2, 1, 5, 1, 3, 2]
k = 3
```

Expected answer:

```text
9
```

Because:

```text
5 + 1 + 3 = 9
```

---

# 7. Brute Force Approach

We could use nested loops:

```java
static int maxSumBruteForce(int[] arr, int k) {

    int maxSum = Integer.MIN_VALUE;

    for (int i = 0; i <= arr.length - k; i++) {

        int sum = 0;

        for (int j = i; j < i + k; j++) {
            sum += arr[j];
        }

        maxSum = Math.max(maxSum, sum);
    }

    return maxSum;
}
```

### Complexity

There are approximately `n` windows.

Each window takes `k` operations.

Therefore:

```text
Time: O(n * k)
Space: O(1)
```

---

# 8. Sliding Window Approach

Instead of recalculating the entire window, we remove the element leaving the window and add the new element.

```java
static int maxSumSubarray(int[] arr, int k) {

    if (arr == null || arr.length < k || k <= 0) {
        return -1;
    }

    int windowSum = 0;
    int maxSum = Integer.MIN_VALUE;

    // First window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }

    maxSum = windowSum;

    // Slide the window
    for (int right = k; right < arr.length; right++) {

        windowSum += arr[right];

        windowSum -= arr[right - k];

        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}
```

---

# 9. Dry Run

Input:

```text
arr = [2, 1, 5, 1, 3, 2]
k = 3
```

## Step 1: First Window

```text
[2, 1, 5]
```

Sum:

```text
2 + 1 + 5 = 8
```

```text
windowSum = 8
maxSum = 8
```

---

## Step 2: Move Window

Current:

```text
[2, 1, 5] 1 3 2
```

Remove:

```text
2
```

Add:

```text
1
```

New window:

```text
2 [1, 5, 1] 3 2
```

New sum:

```text
8 - 2 + 1 = 7
```

```text
maxSum = 8
```

---

## Step 3

Current:

```text
2 1 [5, 1, 3] 2
```

Calculation:

```text
7 - 1 + 3 = 9
```

```text
maxSum = 9
```

---

## Step 4

Current:

```text
2 1 5 [1, 3, 2]
```

Calculation:

```text
9 - 5 + 2 = 6
```

Final:

```text
maxSum = 9
```

Answer:

```text
9
```

---

# 10. Fixed-Size Sliding Window Template

This pattern is worth remembering.

```java
int window = 0;

for (int i = 0; i < k; i++) {
    window += arr[i];
}

for (int right = k; right < arr.length; right++) {

    // Add new element
    window += arr[right];

    // Remove old element
    window -= arr[right - k];

    // Process current window
}
```

Think:

```text
ADD right
REMOVE left
PROCESS window
```

---

# 11. Fixed Window Example: Maximum Average

Problem:

> Find the maximum average of any subarray of size `k`.

Example:

```text
arr = [1, 12, -5, -6, 50, 3]
k = 4
```

Windows:

```text
[1, 12, -5, -6]  = 2
[12, -5, -6, 50] = 51
[-5, -6, 50, 3]  = 42
```

Maximum sum:

```text
51
```

Maximum average:

```text
51 / 4 = 12.75
```

Java:

```java
static double findMaxAverage(int[] arr, int k) {

    int windowSum = 0;

    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }

    int maxSum = windowSum;

    for (int right = k; right < arr.length; right++) {

        windowSum += arr[right];
        windowSum -= arr[right - k];

        maxSum = Math.max(maxSum, windowSum);
    }

    return (double) maxSum / k;
}
```

Complexity:

```text
Time: O(n)
Space: O(1)
```

---

# 12. Variable-Size Sliding Window

In a variable-size window, the window size is not fixed.

It can:

```text
expand
shrink
expand
shrink
```

depending on the condition.

Usually we use two pointers:

```text
left
right
```

Example:

```text
left → [ window ] ← right
```

General pattern:

```java
int left = 0;

for (int right = 0; right < arr.length; right++) {

    // Add arr[right]

    while (condition) {

        // Process window

        // Remove arr[left]
        left++;
    }
}
```

---

# 13. Example: Smallest Subarray With Sum >= Target

Problem:

> Given an array of positive integers, find the minimum length subarray whose sum is greater than or equal to `target`.

Example:

```text
arr = [2, 3, 1, 2, 4, 3]
target = 7
```

Possible subarrays:

```text
[2, 3, 1, 2] = 8
[3, 1, 2, 4] = 10
[4, 3] = 7
```

The smallest is:

```text
[4, 3]
```

Length:

```text
2
```

Answer:

```text
2
```

---

# 14. Why Variable Sliding Window Works

Start with:

```text
[2]
```

Sum:

```text
2
```

Not enough.

Expand:

```text
[2, 3]
```

Sum:

```text
5
```

Still not enough.

Expand:

```text
[2, 3, 1, 2]
```

Sum:

```text
8
```

Now:

```text
sum >= target
```

So we try to shrink the window.

Remove `2`:

```text
[3, 1, 2]
```

Sum:

```text
6
```

Now it is below target.

So stop shrinking and expand again.

---

# 15. Java Implementation

```java
static int minSubArrayLen(int target, int[] arr) {

    int left = 0;
    int sum = 0;
    int minLength = Integer.MAX_VALUE;

    for (int right = 0; right < arr.length; right++) {

        sum += arr[right];

        while (sum >= target) {

            minLength = Math.min(
                minLength,
                right - left + 1
            );

            sum -= arr[left];
            left++;
        }
    }

    return minLength == Integer.MAX_VALUE
            ? 0
            : minLength;
}
```

---

# 16. Dry Run

Input:

```text
arr = [2, 3, 1, 2, 4, 3]
target = 7
```

Initially:

```text
left = 0
sum = 0
minLength = INF
```

### Add 2

```text
window = [2]
sum = 2
```

Not enough.

---

### Add 3

```text
window = [2, 3]
sum = 5
```

Not enough.

---

### Add 1

```text
window = [2, 3, 1]
sum = 6
```

Not enough.

---

### Add 2

```text
window = [2, 3, 1, 2]
sum = 8
```

Now:

```text
8 >= 7
```

Length:

```text
4
```

So:

```text
minLength = 4
```

Shrink:

```text
remove 2
```

Window:

```text
[3, 1, 2]
```

Sum:

```text
6
```

Stop shrinking.

---

### Add 4

Window:

```text
[3, 1, 2, 4]
```

Sum:

```text
10
```

Length:

```text
4
```

Shrink:

```text
remove 3
```

Window:

```text
[1, 2, 4]
```

Sum:

```text
7
```

Length:

```text
3
```

Update:

```text
minLength = 3
```

Shrink again:

```text
remove 1
```

Window:

```text
[2, 4]
```

Sum:

```text
6
```

Stop.

---

### Add 3

Window:

```text
[2, 4, 3]
```

Sum:

```text
9
```

Length:

```text
3
```

Shrink:

```text
remove 2
```

Window:

```text
[4, 3]
```

Sum:

```text
7
```

Length:

```text
2
```

Update:

```text
minLength = 2
```

Shrink:

```text
remove 4
```

Window:

```text
[3]
```

Sum:

```text
3
```

Stop.

Final answer:

```text
2
```

---

# 17. Important Rule: Positive Numbers

The standard sum-based variable sliding window works especially well when the array contains **positive numbers**.

Why?

Because when we expand:

```text
sum increases
```

When we shrink:

```text
sum decreases
```

This predictable behavior allows us to control the window.

For example:

```text
[2, 3, 1, 2, 4, 3]
```

all values are positive.

---

# 18. What About Negative Numbers?

Consider:

```text
[2, -5, 10, -2, 7]
```

Removing an element from the left does not always decrease the sum.

Adding an element does not always increase the sum.

Therefore, the normal sliding-window logic may not work.

For such problems, techniques such as:

```text
Prefix Sum
HashMap
Monotonic Queue
Binary Search
```

may be required depending on the problem.

This is an important interview point.

---

# 19. Sliding Window for Strings

Sliding Window is extremely common with strings.

Typical questions:

```text
Longest substring without repeating characters
Longest substring with at most K distinct characters
Minimum window substring
Permutation in string
Find all anagrams
```

Usually we combine Sliding Window with:

```text
HashMap
HashSet
Frequency Array
```

---

# 20. Longest Substring Without Repeating Characters

Problem:

> Given a string, find the length of the longest substring without duplicate characters.

Example:

```text
s = "abcabcbb"
```

Possible substrings:

```text
"abc"
```

Length:

```text
3
```

Another:

```text
"bca"
```

Length:

```text
3
```

Answer:

```text
3
```

---

# 21. Sliding Window Idea

Use:

```text
left
right
```

and a `HashSet`.

Example:

```text
abcabcbb
```

Start:

```text
[a]
```

Then:

```text
[ab]
```

Then:

```text
[abc]
```

Next character:

```text
a
```

But `a` already exists.

So shrink from the left.

```text
[bc]
```

Then add `a`:

```text
[bca]
```

Continue.

---

# 22. Java Implementation

```java
import java.util.HashSet;
import java.util.Set;

static int lengthOfLongestSubstring(String s) {

    Set<Character> set = new HashSet<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        char current = s.charAt(right);

        while (set.contains(current)) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(current);

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

---

# 23. Dry Run

Input:

```text
"abcabcbb"
```

### Character `a`

```text
[a]
```

Length:

```text
1
```

---

### Character `b`

```text
[ab]
```

Length:

```text
2
```

---

### Character `c`

```text
[abc]
```

Length:

```text
3
```

---

### Character `a`

Duplicate found.

Current:

```text
[abc]
```

Remove from left:

```text
remove a
```

Window becomes:

```text
[bc]
```

Add new `a`:

```text
[bca]
```

Length:

```text
3
```

Continue.

Final answer:

```text
3
```

---

# 24. Using HashMap Instead of HashSet

Sometimes we can make the solution faster by storing the last position of each character.

```java
import java.util.HashMap;
import java.util.Map;

static int lengthOfLongestSubstring(String s) {

    Map<Character, Integer> map = new HashMap<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        char current = s.charAt(right);

        if (map.containsKey(current)) {
            left = Math.max(left, map.get(current) + 1);
        }

        map.put(current, right);

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

This avoids repeatedly removing characters from the `HashSet`.

---

# 25. At Most K Distinct Characters

Another common Sliding Window problem:

> Find the longest substring containing at most `k` distinct characters.

Example:

```text
s = "eceba"
k = 2
```

Longest valid substring:

```text
"ece"
```

It contains:

```text
e
c
```

Only `2` distinct characters.

Answer:

```text
3
```

---

# 26. Java Implementation

```java
import java.util.HashMap;
import java.util.Map;

static int longestSubstringKDistinct(String s, int k) {

    if (k <= 0) {
        return 0;
    }

    Map<Character, Integer> frequency = new HashMap<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        char current = s.charAt(right);

        frequency.put(
            current,
            frequency.getOrDefault(current, 0) + 1
        );

        while (frequency.size() > k) {

            char leftChar = s.charAt(left);

            frequency.put(
                leftChar,
                frequency.get(leftChar) - 1
            );

            if (frequency.get(leftChar) == 0) {
                frequency.remove(leftChar);
            }

            left++;
        }

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

---

# 27. The Most Important Sliding Window Pattern

For variable-size problems, remember this:

```java
int left = 0;

for (int right = 0; right < n; right++) {

    // Add arr[right] to the window

    while (windowIsInvalid()) {

        // Remove arr[left]
        left++;
    }

    // Window is valid here
    // Process it
}
```

The logic is:

```text
Expand → Check → Shrink → Process
```

---

# 28. Fixed vs Variable Sliding Window

| Type | Window Size | Typical Problem |
|---|---|---|
| Fixed | Always `k` | Maximum sum of size `k` |
| Variable | Changes | Minimum subarray with sum >= target |
| String | Changes | Longest substring without duplicates |
| Frequency | Changes | At most K distinct characters |

---

# 29. How to Recognize Sliding Window Problems

Look for words like:

```text
subarray
substring
contiguous
consecutive
window
longest
shortest
maximum
minimum
at most K
at least K
exactly K
size K
```

Examples:

> Maximum sum of `k` consecutive elements

Think:

```text
Fixed Sliding Window
```

---

> Longest substring without repeating characters

Think:

```text
Variable Sliding Window + HashSet
```

---

> Longest substring with at most K distinct characters

Think:

```text
Variable Sliding Window + HashMap
```

---

> Minimum subarray whose sum is at least target

Think:

```text
Variable Sliding Window
```

when the relevant values are positive.

---

# 30. Common Mistakes

## Mistake 1: Forgetting `+1`

Window length is:

```java
right - left + 1
```

Not:

```java
right - left
```

Example:

```text
left = 2
right = 5
```

Elements:

```text
2, 3, 4, 5
```

Count:

```text
4
```

Therefore:

```text
5 - 2 + 1 = 4
```

---

## Mistake 2: Removing the Wrong Element

For fixed window:

```java
windowSum -= arr[right - k];
```

The element leaving the window is:

```text
right - k
```

---

## Mistake 3: Forgetting to Move `left`

Incorrect:

```java
while (condition) {
    // nothing
}
```

This creates an infinite loop.

Correct:

```java
while (condition) {

    // remove left element

    left++;
}
```

---

## Mistake 4: Using Sliding Window When It Doesn't Apply

Not every subarray problem is a Sliding Window problem.

Especially with:

```text
negative numbers
```

standard sum-based sliding window may fail.

---

# 31. Why Sliding Window Is O(n)

This is a very important interview question.

Suppose:

```text
left = 0
right = 0
```

Both pointers move only forward.

`right` moves at most:

```text
n times
```

`left` also moves at most:

```text
n times
```

Therefore:

```text
O(n + n)
```

which is:

```text
O(n)
```

We don't count it as `O(n²)` just because there is a `while` loop inside a `for` loop.

The key is that `left` never moves backward.

---

# 32. Sliding Window Complexity

Typical complexity:

```text
Time: O(n)
```

For HashMap/HashSet operations:

```text
Average: O(1)
```

Therefore:

```text
Time: O(n)
Space: O(k)
```

or:

```text
Space: O(characters)
```

depending on the problem.

---

# 33. Interview Questions

### Q1. What is Sliding Window?

Sliding Window is a technique used to efficiently process contiguous ranges of arrays or strings by maintaining a current window and moving its boundaries instead of recalculating every range.

---

### Q2. What are the two main types?

```text
1. Fixed-size Sliding Window
2. Variable-size Sliding Window
```

---

### Q3. When should you use Sliding Window?

When the problem involves:

```text
contiguous subarrays
contiguous substrings
consecutive elements
longest/shortest range
maximum/minimum range
fixed-size windows
```

---

### Q4. Why is Sliding Window usually O(n)?

Because each element is generally added to and removed from the window at most once.

---

### Q5. Why can negative numbers be a problem?

Because adding/removing an element no longer guarantees that the sum will move predictably in one direction.

---

### Q6. What data structures are commonly used with Sliding Window?

```text
HashSet
HashMap
Frequency Array
Deque
```

---

### Q7. What is the difference between Two Pointers and Sliding Window?

Two Pointers is a broader technique where two indices are used.

Sliding Window is a common application of Two Pointers where the two pointers represent the boundaries of a contiguous range.

---

# 34. Practice Problems

## Easy

1. Maximum Sum Subarray of Size K
2. Maximum Average Subarray I
3. Find All Anagrams in a String
4. Longest Substring Without Repeating Characters

## Medium

5. Minimum Size Subarray Sum
6. Longest Substring with At Most K Distinct Characters
7. Longest Repeating Character Replacement
8. Permutation in String
9. Fruit Into Baskets
10. Max Consecutive Ones III

## Hard

11. Minimum Window Substring
12. Sliding Window Maximum
13. Subarrays with K Different Integers

---

# 35. Sliding Window Cheat Sheet

## Fixed Window

```java
int windowSum = 0;

for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}

for (int right = k; right < arr.length; right++) {

    windowSum += arr[right];
    windowSum -= arr[right - k];

    // process
}
```

---

## Variable Window

```java
int left = 0;

for (int right = 0; right < arr.length; right++) {

    // add right

    while (condition) {

        // remove left
        left++;
    }

    // process window
}
```

---

## String + HashSet

```java
Set<Character> set = new HashSet<>();

int left = 0;

for (int right = 0; right < s.length(); right++) {

    while (set.contains(s.charAt(right))) {
        set.remove(s.charAt(left));
        left++;
    }

    set.add(s.charAt(right));

    // process
}
```

---

## String + HashMap

```java
Map<Character, Integer> map = new HashMap<>();

int left = 0;

for (int right = 0; right < s.length(); right++) {

    char c = s.charAt(right);

    map.put(c, map.getOrDefault(c, 0) + 1);

    while (/* window invalid */) {

        char leftChar = s.charAt(left);

        map.put(
            leftChar,
            map.get(leftChar) - 1
        );

        if (map.get(leftChar) == 0) {
            map.remove(leftChar);
        }

        left++;
    }

    // process
}
```

---

# 36. Quick Recognition Guide

When you see:

```text
"size K"
```

Think:

```text
Fixed Sliding Window
```

When you see:

```text
"longest"
"shortest"
"minimum"
"maximum"
"at most K"
"at least K"
```

and the problem involves a contiguous range, think:

```text
Variable Sliding Window
```

When the problem involves:

```text
characters + frequency
```

think:

```text
Sliding Window + HashMap
```

When it involves:

```text
unique characters
```

think:

```text
Sliding Window + HashSet
```

---

# 37. Final Takeaway

Sliding Window helps us avoid repeatedly processing the same elements.

Instead of:

```text
Window 1 → calculate everything
Window 2 → calculate everything
Window 3 → calculate everything
```

we reuse the previous work:

```text
Add new element
Remove old element
Update answer
```

The most important patterns to remember are:

```text
Fixed Window:
ADD right → REMOVE old left → PROCESS

Variable Window:
EXPAND right → SHRINK left → PROCESS

String:
Sliding Window + HashMap/HashSet
```

The biggest skill is not memorizing the code.

It is recognizing:

> **"This problem is asking about a contiguous range, and I can maintain that range while moving two pointers."**

---

# Checklist

Before moving to the next topic, make sure you can explain and implement:

- [ ] What is Sliding Window?
- [ ] What is a subarray?
- [ ] What is a substring?
- [ ] Fixed-size Sliding Window
- [ ] Variable-size Sliding Window
- [ ] Maximum sum of size `k`
- [ ] Minimum subarray with sum >= target
- [ ] Longest substring without repeating characters
- [ ] Sliding Window + HashSet
- [ ] Sliding Window + HashMap
- [ ] Why Sliding Window is usually O(n)
- [ ] When negative numbers cause problems
- [ ] How to identify Sliding Window problems

---

# Next Topic

```text
02-arrays/prefix-sum.md
```

Prefix Sum is the next important array technique for answering **range-sum and cumulative-sum problems efficiently**.