# Prefix Sum

Prefix Sum is a very useful technique for solving problems involving:

- Range sums
- Subarray sums
- Cumulative sums
- Multiple sum queries
- Finding subarrays with a particular sum
- Converting repeated `O(n)` calculations into `O(1)` queries

The main idea is simple:

> **Precompute cumulative information so that a range can be calculated quickly.**

---

# 1. What is Prefix Sum?

Consider this array:

```text
[2, 4, 1, 5, 3]
```

The prefix sum array stores the sum from the beginning up to each position.

```text
Original:
[2, 4, 1, 5, 3]

Prefix:
[2, 6, 7, 12, 15]
```

Because:

```text
2
2 + 4 = 6
2 + 4 + 1 = 7
2 + 4 + 1 + 5 = 12
2 + 4 + 1 + 5 + 3 = 15
```

So:

```text
prefix[i] = sum of elements from index 0 to i
```

---

# 2. Why Do We Need Prefix Sum?

Suppose we have:

```text
arr = [2, 4, 1, 5, 3]
```

And we need to answer:

```text
What is the sum from index 1 to index 3?
```

That means:

```text
4 + 1 + 5 = 10
```

We could loop through the range every time.

```java
int sum = 0;

for (int i = 1; i <= 3; i++) {
    sum += arr[i];
}
```

This takes:

```text
O(n)
```

for each query in the worst case.

If we have thousands of queries, this becomes expensive.

Prefix Sum allows us to answer the range sum in:

```text
O(1)
```

after an initial:

```text
O(n)
```

preprocessing step.

---

# 3. Building a Prefix Sum Array

Given:

```text
arr = [2, 4, 1, 5, 3]
```

We create:

```text
prefix = [2, 6, 7, 12, 15]
```

Formula:

```text
prefix[i] = prefix[i - 1] + arr[i]
```

For the first element:

```text
prefix[0] = arr[0]
```

---

# 4. Java Implementation

```java
static int[] buildPrefixSum(int[] arr) {

    int[] prefix = new int[arr.length];

    if (arr.length == 0) {
        return prefix;
    }

    prefix[0] = arr[0];

    for (int i = 1; i < arr.length; i++) {
        prefix[i] = prefix[i - 1] + arr[i];
    }

    return prefix;
}
```

Example:

```java
int[] arr = {2, 4, 1, 5, 3};

int[] prefix = buildPrefixSum(arr);
```

Result:

```text
[2, 6, 7, 12, 15]
```

---

# 5. Range Sum Using Prefix Sum

Suppose:

```text
arr = [2, 4, 1, 5, 3]
```

Prefix:

```text
prefix = [2, 6, 7, 12, 15]
```

We want:

```text
sum(1, 3)
```

That means:

```text
arr[1] + arr[2] + arr[3]

= 4 + 1 + 5

= 10
```

Use:

```text
prefix[right] - prefix[left - 1]
```

Therefore:

```text
prefix[3] - prefix[0]

= 12 - 2

= 10
```

---

# 6. Range Sum Formula

For:

```text
left > 0
```

the formula is:

```text
sum(left, right) = prefix[right] - prefix[left - 1]
```

For example:

```text
left = 2
right = 4
```

Then:

```text
sum(2, 4)
=
prefix[4] - prefix[1]
```

---

# 7. The Special Case: left = 0

Suppose we want:

```text
sum(0, 3)
```

There is no `left - 1` index.

So:

```text
sum(0, 3) = prefix[3]
```

This is why a very common implementation uses:

```java
if (left == 0) {
    return prefix[right];
}

return prefix[right] - prefix[left - 1];
```

---

# 8. Complete Range Sum Function

```java
static int rangeSum(int[] prefix, int left, int right) {

    if (left == 0) {
        return prefix[right];
    }

    return prefix[right] - prefix[left - 1];
}
```

Example:

```java
int[] arr = {2, 4, 1, 5, 3};

int[] prefix = buildPrefixSum(arr);

System.out.println(rangeSum(prefix, 1, 3));
```

Output:

```text
10
```

---

# 9. Dry Run

Array:

```text
[2, 4, 1, 5, 3]
```

Prefix:

```text
[2, 6, 7, 12, 15]
```

Query:

```text
left = 1
right = 3
```

Formula:

```text
prefix[3] - prefix[0]
```

Values:

```text
12 - 2
```

Answer:

```text
10
```

---

# 10. Why Does the Formula Work?

Suppose:

```text
arr = [2, 4, 1, 5, 3]
```

Prefix:

```text
[2, 6, 7, 12, 15]
```

For:

```text
sum(1, 3)
```

`prefix[3]` contains:

```text
2 + 4 + 1 + 5
```

`prefix[0]` contains:

```text
2
```

Subtract:

```text
(2 + 4 + 1 + 5) - 2
```

The `2` cancels.

Remaining:

```text
4 + 1 + 5
```

Therefore:

```text
10
```

---

# 11. Prefix Sum With an Extra Element

A cleaner approach is to create a prefix array of size:

```text
n + 1
```

instead of:

```text
n
```

For:

```text
arr = [2, 4, 1, 5, 3]
```

we create:

```text
prefix = [0, 2, 6, 7, 12, 15]
```

Notice the first element is `0`.

This makes the range formula much simpler.

---

# 12. Formula With n + 1 Prefix Array

For a range:

```text
[left, right]
```

use:

```text
prefix[right + 1] - prefix[left]
```

Example:

```text
left = 1
right = 3
```

Then:

```text
prefix[4] - prefix[1]
```

Values:

```text
12 - 2
```

Answer:

```text
10
```

---

# 13. Why Is This Version Better?

Without the extra `0`, we need:

```java
if (left == 0) {
    return prefix[right];
}

return prefix[right] - prefix[left - 1];
```

With the extra `0`:

```java
return prefix[right + 1] - prefix[left];
```

No special case.

This is one of the most useful Prefix Sum patterns to remember.

---

# 14. Java Implementation

```java
static int[] buildPrefixSum(int[] arr) {

    int[] prefix = new int[arr.length + 1];

    for (int i = 0; i < arr.length; i++) {
        prefix[i + 1] = prefix[i] + arr[i];
    }

    return prefix;
}
```

Range sum:

```java
static int rangeSum(int[] prefix, int left, int right) {

    return prefix[right + 1] - prefix[left];
}
```

---

# 15. Example

Input:

```text
arr = [2, 4, 1, 5, 3]
```

Prefix:

```text
[0, 2, 6, 7, 12, 15]
```

Query:

```text
left = 2
right = 4
```

Formula:

```text
prefix[5] - prefix[2]
```

```text
15 - 6
```

Answer:

```text
9
```

Check manually:

```text
1 + 5 + 3 = 9
```

Correct.

---

# 16. Multiple Range Queries

This is where Prefix Sum becomes especially useful.

Suppose:

```text
arr = [2, 4, 1, 5, 3]
```

Queries:

```text
[1, 3]
[0, 2]
[2, 4]
[1, 4]
```

Without Prefix Sum, each query may require traversing multiple elements.

With Prefix Sum:

```text
prefix = [0, 2, 6, 7, 12, 15]
```

Each query takes:

```text
O(1)
```

---

# 17. Example of Multiple Queries

### Query 1

```text
[1, 3]
```

```text
prefix[4] - prefix[1]
= 12 - 2
= 10
```

### Query 2

```text
[0, 2]
```

```text
prefix[3] - prefix[0]
= 7 - 0
= 7
```

### Query 3

```text
[2, 4]
```

```text
prefix[5] - prefix[2]
= 15 - 6
= 9
```

### Query 4

```text
[1, 4]
```

```text
prefix[5] - prefix[1]
= 15 - 2
= 13
```

---

# 18. Complexity

Building Prefix Sum:

```text
Time: O(n)
Space: O(n)
```

Each range query:

```text
Time: O(1)
```

For `q` queries:

```text
Total = O(n + q)
```

This is much better than:

```text
O(n * q)
```

when every query requires scanning a range.

---

# 19. Prefix Sum vs Sliding Window

These two techniques can look similar but solve different problems.

| Feature | Sliding Window | Prefix Sum |
|---|---|---|
| Main idea | Maintain a moving window | Store cumulative sums |
| Fixed window | Excellent | Possible |
| Variable window | Excellent | Sometimes |
| Range sum query | Not ideal for arbitrary queries | Excellent |
| Multiple range queries | Not ideal | Excellent |
| Extra space | Usually O(1) | Usually O(n) |
| Negative numbers | Depends on problem | Works for range sums |

Example:

> Maximum sum of `k` consecutive elements

Use:

```text
Sliding Window
```

Example:

> Answer 100,000 range-sum queries

Use:

```text
Prefix Sum
```

---

# 20. Prefix Sum With Negative Numbers

Prefix Sum works even when the array contains negative numbers.

Example:

```text
arr = [3, -2, 5, -1]
```

Prefix:

```text
[0, 3, 1, 6, 5]
```

Range:

```text
[1, 3]
```

Calculate:

```text
prefix[4] - prefix[1]

= 5 - 3

= 2
```

Manual calculation:

```text
-2 + 5 - 1 = 2
```

Correct.

This is an important difference from some Sliding Window sum techniques.

---

# 21. Prefix Sum and Subarray Sum

Prefix Sum becomes even more powerful when combined with a `HashMap`.

A very famous problem is:

> Find the number of subarrays whose sum equals `k`.

Example:

```text
arr = [1, 1, 1]
k = 2
```

Valid subarrays:

```text
[1, 1]
[1, 1]
```

Answer:

```text
2
```

---

# 22. Key Mathematical Idea

Suppose the current prefix sum is:

```text
currentSum
```

We want a previous prefix sum such that:

```text
currentSum - previousSum = k
```

Rearrange:

```text
previousSum = currentSum - k
```

So while traversing the array, we can ask:

> Have I seen a prefix sum equal to `currentSum - k`?

If yes, we found one or more subarrays with sum `k`.

This is one of the most important Prefix Sum + HashMap patterns.

---

# 23. Example

Array:

```text
[1, 1, 1]
```

Target:

```text
k = 2
```

Start:

```text
prefixSum = 0
```

Store:

```text
map = {0=1}
```

The `0=1` means:

> Prefix sum `0` has occurred once.

---

### First 1

```text
prefixSum = 1
```

Need:

```text
1 - 2 = -1
```

Not found.

Store:

```text
{0=1, 1=1}
```

---

### Second 1

```text
prefixSum = 2
```

Need:

```text
2 - 2 = 0
```

`0` exists.

Therefore:

```text
count = 1
```

Store prefix sum `2`.

---

### Third 1

```text
prefixSum = 3
```

Need:

```text
3 - 2 = 1
```

`1` exists.

Therefore:

```text
count = 2
```

Final answer:

```text
2
```

---

# 24. Java Implementation

```java
import java.util.HashMap;
import java.util.Map;

static int subarraySum(int[] arr, int k) {

    Map<Integer, Integer> prefixFrequency = new HashMap<>();

    prefixFrequency.put(0, 1);

    int prefixSum = 0;
    int count = 0;

    for (int num : arr) {

        prefixSum += num;

        int required = prefixSum - k;

        if (prefixFrequency.containsKey(required)) {
            count += prefixFrequency.get(required);
        }

        prefixFrequency.put(
            prefixSum,
            prefixFrequency.getOrDefault(prefixSum, 0) + 1
        );
    }

    return count;
}
```

---

# 25. Why Do We Store Frequencies?

Consider:

```text
arr = [0, 0, 0]
k = 0
```

There are multiple subarrays whose sum is `0`.

The same prefix sum can occur multiple times.

Therefore, we need:

```text
prefixSum → frequency
```

not just:

```text
prefixSum → exists
```

---

# 26. Important Initialization

This line is extremely important:

```java
prefixFrequency.put(0, 1);
```

Why?

It represents the empty prefix before the array begins.

Suppose:

```text
arr = [5]
k = 5
```

At the first element:

```text
prefixSum = 5
```

Required:

```text
5 - 5 = 0
```

If we already stored:

```text
0 → 1
```

we correctly detect:

```text
[5]
```

as a valid subarray.

Without this initialization, we'd miss subarrays starting at index `0`.

---

# 27. Prefix Sum Pattern to Remember

For:

> Count subarrays with sum `k`

remember:

```text
currentPrefixSum - previousPrefixSum = k
```

Therefore:

```text
previousPrefixSum = currentPrefixSum - k
```

And use:

```java
Map<Integer, Integer>
```

to store prefix-sum frequencies.

---

# 28. Another Example

```text
arr = [3, 4, 7, 2, -3, 1, 4, 2]
k = 7
```

The HashMap approach can find all subarrays whose sum is `7` in:

```text
O(n)
```

instead of checking every possible subarray.

This is a very common interview problem.

---

# 29. Prefix Sum for Product? No

Prefix Sum is naturally suited to operations where a cumulative value can be combined and subtracted.

It works especially well for:

```text
sum
```

For multiplication, subtraction, minimum, maximum, etc., the same simple prefix-sum formula generally does not apply.

For example:

```text
prefix[right] - prefix[left]
```

works for sums because:

```text
(A + B + C) - A = B + C
```

But subtraction does not generally work the same way for every operation.

---

# 30. 2D Prefix Sum

Prefix Sum can also be extended to two-dimensional arrays.

Example matrix:

```text
1  2  3
4  5  6
7  8  9
```

A 2D prefix sum lets us quickly calculate the sum of any rectangular region.

This is useful for problems involving:

```text
matrix
grid
rectangle
submatrix
2D range sum
```

The formula is based on the same idea:

> Add the required regions and subtract the overlapping regions.

2D Prefix Sum is an advanced extension and will become easier once the basic 1D version is comfortable.

---

# 31. Common Mistakes

## Mistake 1: Wrong Formula

For an `n + 1` prefix array:

```text
prefix[right + 1] - prefix[left]
```

not:

```text
prefix[right] - prefix[left]
```

---

## Mistake 2: Forgetting the Extra Zero

Recommended:

```text
prefix = new int[n + 1];
```

Then:

```text
prefix[0] = 0;
```

---

## Mistake 3: Forgetting `prefixFrequency.put(0, 1)`

For subarray-sum problems:

```java
prefixFrequency.put(0, 1);
```

is essential.

---

## Mistake 4: Using Set Instead of Map

For counting subarrays:

```text
HashSet
```

is not enough.

We need:

```text
HashMap<prefixSum, frequency>
```

because the same prefix sum can occur multiple times.

---

# 32. Interview Questions

### Q1. What is Prefix Sum?

Prefix Sum is a technique where cumulative sums are precomputed so that range-sum queries can be answered efficiently.

---

### Q2. What is the complexity of building Prefix Sum?

```text
O(n)
```

---

### Q3. What is the complexity of a range query?

With a prefix array:

```text
O(1)
```

---

### Q4. What is the formula for range sum?

With an `n + 1` prefix array:

```text
prefix[right + 1] - prefix[left]
```

---

### Q5. Why do we use an extra zero?

It eliminates special cases for ranges beginning at index `0`.

---

### Q6. Can Prefix Sum work with negative numbers?

Yes.

---

### Q7. What is Prefix Sum + HashMap used for?

It is commonly used to solve:

```text
Count subarrays with sum K
```

and similar problems.

---

### Q8. Why do we initialize the map with `{0: 1}`?

It represents the empty prefix and allows us to correctly count subarrays starting from index `0`.

---

### Q9. Prefix Sum vs Sliding Window?

Use Prefix Sum when you need efficient range queries or when prefix differences can represent the desired range property.

Use Sliding Window when you need to dynamically maintain a contiguous window.

---

# 33. Practice Problems

## Easy

1. Running Sum of 1D Array
2. Find Pivot Index
3. Range Sum Query
4. Left and Right Sum Differences
5. Find the Middle Index in Array

## Medium

6. Subarray Sum Equals K
7. Continuous Subarray Sum
8. Product of Array Except Self
9. Binary Subarrays With Sum
10. Number of Ways to Split Array

## Advanced

11. 2D Range Sum Query
12. Submatrix Sum Target
13. Maximum Size Subarray Sum Equals K

---

# 34. Prefix Sum Cheat Sheet

## Build Prefix Sum

```java
int[] prefix = new int[arr.length + 1];

for (int i = 0; i < arr.length; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
```

---

## Range Sum

```java
int sum = prefix[right + 1] - prefix[left];
```

---

## Count Subarrays With Sum K

```java
Map<Integer, Integer> map = new HashMap<>();

map.put(0, 1);

int prefixSum = 0;
int count = 0;

for (int num : arr) {

    prefixSum += num;

    int required = prefixSum - k;

    count += map.getOrDefault(required, 0);

    map.put(
        prefixSum,
        map.getOrDefault(prefixSum, 0) + 1
    );
}
```

---

# 35. Prefix Sum Recognition Guide

When you see:

```text
range sum
sum between L and R
multiple range queries
cumulative sum
subarray sum
count subarrays with sum K
```

Think:

```text
PREFIX SUM
```

If you see:

```text
subarray + sum K
```

especially with negative numbers, think:

```text
PREFIX SUM + HASHMAP
```

If you see:

```text
fixed-size contiguous window
```

think:

```text
SLIDING WINDOW
```

---

# 36. Final Takeaway

Prefix Sum transforms repeated range calculations into fast queries.

Instead of repeatedly doing:

```text
for every query:
    loop through the range
```

we do:

```text
Build Prefix Sum → O(n)

Each Query → O(1)
```

The most important formula is:

```text
rangeSum(left, right)
=
prefix[right + 1] - prefix[left]
```

And the most important interview pattern is:

```text
currentPrefixSum - previousPrefixSum = target
```

which leads to:

```text
previousPrefixSum = currentPrefixSum - target
```

and therefore:

```text
Prefix Sum + HashMap
```

for many subarray-sum problems.

---

# Checklist

Before moving forward, make sure you can explain and implement:

- [ ] What is Prefix Sum?
- [ ] How to build a Prefix Sum array
- [ ] Why we use an extra `0`
- [ ] Range sum in `O(1)`
- [ ] Multiple range queries
- [ ] Prefix Sum with negative numbers
- [ ] Prefix Sum + HashMap
- [ ] Subarray Sum Equals K
- [ ] Why `{0: 1}` is required
- [ ] Why HashMap frequency is required
- [ ] Difference between Prefix Sum and Sliding Window
- [ ] Basic idea of 2D Prefix Sum

---

# Next Topic

```text
03-strings/strings.md
```

The next section starts **Strings**, where we'll cover String basics in Java, `String` vs `StringBuilder` vs `StringBuffer`, immutability, character access, comparisons, common operations, and important interview concepts.