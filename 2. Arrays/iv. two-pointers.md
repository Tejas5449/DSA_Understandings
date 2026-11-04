# Two Pointers

The **Two Pointers** technique is one of the most important patterns for solving array and string problems efficiently.

Instead of repeatedly comparing every possible pair of elements, we use two indexes, called **pointers**, to move through the data intelligently.

Two pointers can often reduce a solution from:

```text
O(n²)
```

to:

```text
O(n)
```

This pattern is especially useful for:

- Sorted arrays
- Pair-sum problems
- Removing duplicates
- Reversing arrays
- Palindromes
- Partitioning
- Merging arrays
- Container problems
- 3Sum

---

# 1. What Are Two Pointers?

Two pointers simply means maintaining two indexes while processing an array or string.

For example:

```text
[10, 20, 30, 40, 50]
 ↑                ↑
left             right
```

We have:

```text
left = 0
right = arr.length - 1
```

The pointers move based on the problem.

For example:

```text
left++
```

moves the left pointer forward.

And:

```text
right--
```

moves the right pointer backward.

---

# 2. Why Use Two Pointers?

Suppose we have:

```text
[1, 2, 3, 4, 5]
```

and want to find whether two numbers add up to `6`.

A brute-force solution checks every pair:

```text
1 + 2
1 + 3
1 + 4
1 + 5

2 + 3
2 + 4
...
```

This takes:

```text
O(n²)
```

With two pointers, we can solve the same problem in:

```text
O(n)
```

when the array is sorted.

---

# 3. Types of Two-Pointer Techniques

There are two major patterns.

## Pattern 1 — Opposite Direction

Pointers start from opposite ends:

```text
left →          ← right
```

Example:

```text
[1, 2, 3, 4, 5]
 ↑           ↑
left        right
```

Common problems:

- Pair sum in sorted array
- Reverse array
- Palindrome
- Container With Most Water

---

## Pattern 2 — Same Direction

Both pointers start from the beginning but move at different speeds.

Example:

```text
slow →
fast →
```

Common problems:

- Remove duplicates
- Move zeros
- Remove elements
- Fast/slow pointer problems

---

# 4. Pattern 1 — Opposite Direction

The basic structure is:

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {

    // process arr[left] and arr[right]

}
```

The pointers usually move like:

```java
left++;
```

or:

```java
right--;
```

or both.

---

# 5. Example — Reverse an Array

Given:

```text
[10, 20, 30, 40, 50]
```

Expected:

```text
[50, 40, 30, 20, 10]
```

We can use:

```text
left
right
```

## Code

```java
static void reverse(int[] arr) {

    int left = 0;
    int right = arr.length - 1;

    while (left < right) {

        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;

        left++;
        right--;
    }
}
```

---

# 6. Dry Run — Reverse Array

Input:

```text
[10, 20, 30, 40, 50]
```

Initial:

```text
left = 0
right = 4
```

Array:

```text
[10, 20, 30, 40, 50]
 ↑              ↑
 L              R
```

Swap:

```text
10 ↔ 50
```

Array:

```text
[50, 20, 30, 40, 10]
```

Move:

```text
left = 1
right = 3
```

Swap:

```text
20 ↔ 40
```

Array:

```text
[50, 40, 30, 20, 10]
```

Move:

```text
left = 2
right = 2
```

Now:

```text
left < right
```

is false.

Stop.

Final:

```text
[50, 40, 30, 20, 10]
```

---

# 7. Complexity

Each element is processed at most once.

Time:

```text
O(n)
```

Only a few variables are used.

Space:

```text
O(1)
```

This is an **in-place** algorithm.

---

# 8. Example — Check Palindrome

A palindrome reads the same from both directions.

Examples:

```text
madam
racecar
level
```

Not palindromes:

```text
hello
world
```

We can compare the first and last characters.

```text
m a d a m
↑       ↑
L       R
```

Then:

```text
m a d a m
  ↑   ↑
  L   R
```

Continue until the pointers meet.

---

# 9. Java Solution

```java
static boolean isPalindrome(String str) {

    int left = 0;
    int right = str.length() - 1;

    while (left < right) {

        if (str.charAt(left) != str.charAt(right)) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

---

# 10. Dry Run — Palindrome

Input:

```text
"madam"
```

Initial:

```text
m a d a m
↑       ↑
L       R
```

Compare:

```text
m == m
```

Move:

```text
m a d a m
  ↑   ↑
  L   R
```

Compare:

```text
a == a
```

Move:

```text
m a d a m
    ↑
    L/R
```

Pointers meet.

Return:

```text
true
```

---

# 11. Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

No additional string or array is created.

---

# 12. Pair Sum in a Sorted Array

This is one of the most important two-pointer problems.

Given:

```text
[1, 2, 3, 4, 6, 8, 10]
```

Target:

```text
10
```

Find two numbers whose sum is `10`.

Possible answer:

```text
2 + 8 = 10
```

---

# 13. Why Sorting Helps

Because the array is sorted:

```text
1 < 2 < 3 < 4 < 6 < 8 < 10
```

We can start:

```text
left = 0
right = n - 1
```

Visual:

```text
[1, 2, 3, 4, 6, 8, 10]
 ↑                    ↑
 L                    R
```

Calculate:

```text
1 + 10 = 11
```

The sum is too large.

So we need a smaller number.

Move:

```text
right--
```

---

# 14. Continue the Process

Now:

```text
[1, 2, 3, 4, 6, 8, 10]
 ↑                 ↑
 L                 R
```

Calculate:

```text
1 + 8 = 9
```

Too small.

Move:

```text
left++
```

Now:

```text
[1, 2, 3, 4, 6, 8, 10]
    ↑              ↑
    L              R
```

Calculate:

```text
2 + 8 = 10
```

Found.

---

# 15. Java Solution

```java
static boolean hasPairWithSum(int[] arr, int target) {

    int left = 0;
    int right = arr.length - 1;

    while (left < right) {

        int sum = arr[left] + arr[right];

        if (sum == target) {
            return true;
        }

        if (sum < target) {
            left++;
        } else {
            right--;
        }
    }

    return false;
}
```

---

# 16. Pointer Movement Rule

This rule is extremely important.

For a **sorted array**:

```text
sum < target
    ↓
move left forward
```

Why?

Because we need a larger value.

---

And:

```text
sum > target
    ↓
move right backward
```

Why?

Because we need a smaller value.

---

Remember:

```text
sum < target → left++

sum > target → right--
```

---

# 17. Dry Run

Input:

```text
arr = [1, 2, 3, 4, 6, 8, 10]
target = 10
```

### Step 1

```text
left = 0
right = 6

1 + 10 = 11
```

Too large.

```text
right--
```

---

### Step 2

```text
left = 0
right = 5

1 + 8 = 9
```

Too small.

```text
left++
```

---

### Step 3

```text
left = 1
right = 5

2 + 8 = 10
```

Found.

Return:

```text
true
```

---

# 18. Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

This is much better than checking every pair.

---

# 19. Brute Force vs Two Pointers

Suppose:

```text
[1, 2, 3, 4, 6, 8, 10]
```

### Brute Force

```java
for (int i = 0; i < arr.length; i++) {

    for (int j = i + 1; j < arr.length; j++) {

        if (arr[i] + arr[j] == target) {
            return true;
        }
    }
}
```

Time:

```text
O(n²)
```

---

### Two Pointers

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    ...
}
```

Time:

```text
O(n)
```

---

# 20. Important Condition

The pair-sum two-pointer technique depends on the array being **sorted**.

For example:

```text
[1, 2, 3, 4, 6, 8, 10]
```

works.

But:

```text
[8, 2, 10, 1, 6, 3, 4]
```

is not sorted.

You cannot blindly apply the same pointer movement rules.

You could sort it first:

```java
Arrays.sort(arr);
```

But remember that sorting changes the complexity.

Sorting:

```text
O(n log n)
```

Then two pointers:

```text
O(n)
```

Overall:

```text
O(n log n)
```

---

# 21. Two Sum vs Pair Sum

These problems may look similar but can have different constraints.

### Two Sum

Usually asks for two numbers in an unsorted array.

Example:

```text
[2, 7, 11, 15]
target = 9
```

A `HashMap` solution is commonly:

```text
O(n)
```

---

### Pair Sum in Sorted Array

If the array is already sorted:

```text
[1, 2, 3, 4, 6, 8]
```

Two pointers can solve it in:

```text
O(n)
```

This distinction is important in interviews.

---

# 22. Remove Duplicates from Sorted Array

Given:

```text
[1, 1, 2, 2, 3]
```

We want:

```text
[1, 2, 3]
```

Because the array is sorted, duplicates are next to each other.

We can use two pointers:

```text
slow
fast
```

---

# 23. Slow and Fast Pointer

Start:

```text
slow = 0
fast = 1
```

Visual:

```text
[1, 1, 2, 2, 3]
 ↑  ↑
 S  F
```

If:

```text
arr[slow] != arr[fast]
```

we found a new unique element.

Move `slow` forward and copy the value.

---

# 24. Java Solution

```java
static int removeDuplicates(int[] arr) {

    if (arr.length == 0) {
        return 0;
    }

    int slow = 0;

    for (int fast = 1; fast < arr.length; fast++) {

        if (arr[fast] != arr[slow]) {

            slow++;

            arr[slow] = arr[fast];
        }
    }

    return slow + 1;
}
```

The returned value is the number of unique elements.

---

# 25. Dry Run

Input:

```text
[1, 1, 2, 2, 3]
```

Initial:

```text
slow = 0
fast = 1
```

Compare:

```text
arr[0] = 1
arr[1] = 1
```

Same.

Don't move `slow`.

Move `fast`.

---

Now:

```text
slow = 0
fast = 2
```

Compare:

```text
1 != 2
```

Move:

```text
slow = 1
```

Copy:

```text
arr[1] = arr[2]
```

Array becomes:

```text
[1, 2, 2, 2, 3]
```

---

Continue.

Eventually:

```text
[1, 2, 3, ...]
```

Number of unique elements:

```text
3
```

---

# 26. Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

This is an **in-place** solution.

---

# 27. Move Zeros to the End

Given:

```text
[0, 1, 0, 3, 12]
```

Expected:

```text
[1, 3, 12, 0, 0]
```

We can use a slow pointer.

The idea:

```text
slow = position where next non-zero value should go
```

---

# 28. Java Solution

```java
static void moveZeros(int[] arr) {

    int slow = 0;

    for (int fast = 0; fast < arr.length; fast++) {

        if (arr[fast] != 0) {

            int temp = arr[slow];
            arr[slow] = arr[fast];
            arr[fast] = temp;

            slow++;
        }
    }
}
```

---

# 29. Dry Run — Move Zeros

Input:

```text
[0, 1, 0, 3, 12]
```

Initially:

```text
slow = 0
fast = 0
```

`arr[fast]` is `0`.

Nothing happens.

---

`fast = 1`

Value:

```text
1
```

Swap:

```text
arr[slow] ↔ arr[fast]
```

Result:

```text
[1, 0, 0, 3, 12]
```

Move:

```text
slow++
```

---

Eventually:

```text
[1, 3, 12, 0, 0]
```

---

# 30. Container With Most Water

This is a famous interview problem.

Given:

```text
[1, 8, 6, 2, 5, 4, 8, 3, 7]
```

Each number represents the height of a vertical line.

We need to find two lines that hold the maximum amount of water.

---

# 31. Core Formula

For two pointers:

```text
left
right
```

Width:

```text
right - left
```

Height:

```text
min(arr[left], arr[right])
```

Therefore:

```text
area =
min(arr[left], arr[right])
×
(right - left)
```

---

# 32. Why Two Pointers?

Start with the widest possible container:

```text
left = 0
right = n - 1
```

Calculate the area.

Then we need to decide which pointer to move.

The key rule is:

```text
Move the pointer with the smaller height.
```

Why?

Because the smaller height limits the water level.

Moving the taller pointer cannot increase the limiting height.

---

# 33. Java Solution

```java
static int maxArea(int[] height) {

    int left = 0;
    int right = height.length - 1;

    int maxArea = 0;

    while (left < right) {

        int width = right - left;

        int currentHeight =
                Math.min(height[left], height[right]);

        int currentArea = width * currentHeight;

        maxArea = Math.max(maxArea, currentArea);

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxArea;
}
```

---

# 34. Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 35. Three Sum

Three Sum is an important extension of the two-pointer pattern.

Problem:

Given:

```text
[-1, 0, 1, 2, -1, -4]
```

Find triplets whose sum is `0`.

Answer:

```text
[-1, -1, 2]
[-1, 0, 1]
```

---

# 36. Three Sum Strategy

First sort the array:

```text
[-4, -1, -1, 0, 1, 2]
```

Then choose one element.

For example:

```text
i = 0
```

Now solve a two-sum problem for the remaining elements.

We use:

```text
left = i + 1
right = n - 1
```

So the pattern becomes:

```text
i
   left
              right
```

---

# 37. Three Sum Formula

We want:

```text
arr[i] + arr[left] + arr[right] = 0
```

If:

```text
sum < 0
```

move:

```text
left++
```

If:

```text
sum > 0
```

move:

```text
right--
```

If:

```text
sum == 0
```

we found a valid triplet.

---

# 38. Java Solution — Three Sum

```java
static List<List<Integer>> threeSum(int[] arr) {

    List<List<Integer>> result = new ArrayList<>();

    Arrays.sort(arr);

    for (int i = 0; i < arr.length - 2; i++) {

        if (i > 0 && arr[i] == arr[i - 1]) {
            continue;
        }

        int left = i + 1;
        int right = arr.length - 1;

        while (left < right) {

            int sum = arr[i] + arr[left] + arr[right];

            if (sum == 0) {

                result.add(
                    Arrays.asList(
                        arr[i],
                        arr[left],
                        arr[right]
                    )
                );

                left++;
                right--;

                while (left < right &&
                       arr[left] == arr[left - 1]) {
                    left++;
                }

                while (left < right &&
                       arr[right] == arr[right + 1]) {
                    right--;
                }

            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }

    return result;
}
```

---

# 39. Three Sum Complexity

Sorting:

```text
O(n log n)
```

Outer loop:

```text
O(n)
```

Two-pointer scan for each element:

```text
O(n)
```

Overall:

```text
O(n²)
```

Space complexity depends on the output and implementation, but the extra working space apart from the result is generally:

```text
O(1)
```

excluding the sorting implementation and output storage.

---

# 40. Same-Direction Two Pointers

Not every two-pointer problem starts from opposite ends.

Sometimes both pointers move from left to right.

Example:

```text
slow →
fast →
```

The `fast` pointer scans the array.

The `slow` pointer represents a position where we want to place something.

This is extremely useful for:

- Removing duplicates
- Moving zeros
- Removing elements
- Partitioning arrays

---

# 41. Slow and Fast Pointer Concept

Consider:

```text
[1, 1, 2, 2, 3]
```

We want unique values.

The `fast` pointer explores:

```text
fast → → → → →
```

The `slow` pointer tracks:

```text
slow → 
```

the position where the next useful value should go.

This gives us a powerful general pattern:

```text
fast = explore
slow = build result
```

Remember this.

---

# 42. Two Pointer Decision Rules

When using opposite-direction pointers:

```text
left = 0
right = n - 1
```

Ask:

### Is the sum too small?

```text
sum < target
```

Usually:

```text
left++
```

---

### Is the sum too large?

```text
sum > target
```

Usually:

```text
right--
```

---

### Are values different from each other?

For palindrome:

```text
left++
right--
```

---

### Are we processing a container?

Move the pointer with the smaller height.

---

# 43. When Should You Think About Two Pointers?

Look for clues such as:

### Clue 1

The array is sorted.

```text
[1, 2, 3, 4, 5]
```

Think:

```text
Two Pointers
Binary Search
```

---

### Clue 2

The problem asks for a pair.

Examples:

```text
Two numbers with target sum
Closest pair
```

Think:

```text
Two Pointers
```

---

### Clue 3

The problem asks you to reverse something.

Think:

```text
left + right
```

---

### Clue 4

The problem asks whether a string is a palindrome.

Think:

```text
left + right
```

---

### Clue 5

The problem asks you to remove duplicates in a sorted array.

Think:

```text
slow + fast
```

---

### Clue 6

The problem asks you to move elements while maintaining order.

Think:

```text
slow + fast
```

---

# 44. Brute Force vs Two Pointers

Consider pair sum.

### Brute Force

```java
for (int i = 0; i < n; i++) {

    for (int j = i + 1; j < n; j++) {

        if (arr[i] + arr[j] == target) {
            return true;
        }
    }
}
```

Complexity:

```text
O(n²)
```

---

### Two Pointers

```java
int left = 0;
int right = n - 1;

while (left < right) {

    int sum = arr[left] + arr[right];

    if (sum == target) {
        return true;
    }

    if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

Complexity:

```text
O(n)
```

The improvement comes from **eliminating unnecessary comparisons**.

---

# 45. Important Limitation

Two pointers are not a magical solution for every array problem.

For example, if you have an unsorted array:

```text
[8, 1, 5, 3, 9]
```

and need a pair with a target sum, blindly placing pointers at both ends does not provide the necessary ordering information.

You need to consider:

- Sorting
- HashMap
- Another algorithm

Always understand **why the pointers can safely move**.

---

# 46. Common Mistakes

## Mistake 1 — Forgetting the Sorted Requirement

For pair-sum two pointers, the array usually needs to be sorted.

---

## Mistake 2 — Wrong Pointer Movement

For a sorted pair-sum problem:

```text
sum < target
```

means we need a larger value:

```text
left++
```

And:

```text
sum > target
```

means we need a smaller value:

```text
right--
```

---

## Mistake 3 — Infinite Loop

Make sure at least one pointer moves in every iteration.

Wrong:

```java
while (left < right) {

    if (arr[left] == arr[right]) {
        // no pointer movement
    }
}
```

This can cause an infinite loop.

---

## Mistake 4 — Incorrect Loop Condition

Usually:

```java
while (left < right)
```

is used for pairs.

Using:

```java
while (left <= right)
```

can cause the same element to be considered twice, depending on the problem.

---

## Mistake 5 — Ignoring Duplicates

Problems such as Three Sum require special handling of duplicate values.

---

# 47. Interview Questions

## Q1. What is the two-pointer technique?

It is an algorithmic technique where two indexes move through an array or string according to specific rules to reduce unnecessary comparisons.

---

## Q2. When is two-pointer technique useful?

Common cases include:

- Sorted arrays
- Pair-sum problems
- Palindromes
- Reversing arrays
- Removing duplicates
- Partitioning
- Container problems

---

## Q3. What is the usual complexity of a two-pointer solution?

Many two-pointer algorithms run in:

```text
O(n)
```

because each pointer moves through the array at most once.

---

## Q4. Does two-pointer always require a sorted array?

No.

Some two-pointer techniques such as:

- Reverse array
- Palindrome
- Move zeros

do not require sorting.

But pair-sum pointer movement generally relies on sorted order.

---

## Q5. What is the difference between slow and fast pointers?

The `fast` pointer usually scans or explores the array.

The `slow` pointer tracks the position where useful elements should be placed.

---

## Q6. Why does two-pointer improve over brute force?

It uses information about the array to eliminate unnecessary comparisons.

Instead of checking every pair:

```text
O(n²)
```

it may process the array with:

```text
O(n)
```

---

## Q7. What is an in-place two-pointer algorithm?

An algorithm where the pointers modify the original array without creating another array proportional to the input size.

---

# 48. Practice Problems — Easy

## 1. Reverse an Array

```text
Input:
[1, 2, 3, 4, 5]

Output:
[5, 4, 3, 2, 1]
```

---

## 2. Check Palindrome

```text
Input:
"racecar"

Output:
true
```

---

## 3. Pair Sum in Sorted Array

```text
Input:
[1, 2, 3, 4, 6]
target = 6

Output:
true
```

---

## 4. Find Pair Indices

```text
Input:
[1, 2, 3, 4, 6]
target = 6

Output:
[1, 3]
```

Because:

```text
2 + 4 = 6
```

---

## 5. Remove Duplicates

```text
Input:
[1, 1, 2, 2, 3]

Output:
[1, 2, 3]
```

---

## 6. Move Zeros

```text
Input:
[0, 1, 0, 3, 12]

Output:
[1, 3, 12, 0, 0]
```

---

# 49. Practice Problems — Medium

## 7. Container With Most Water

```text
Input:
[1,8,6,2,5,4,8,3,7]

Output:
49
```

---

## 8. Three Sum

```text
Input:
[-1,0,1,2,-1,-4]

Output:
[[-1,-1,2],[-1,0,1]]
```

---

## 9. Remove a Specific Element

Given:

```text
[3, 2, 2, 3]
```

Remove all occurrences of:

```text
3
```

Expected logical result:

```text
[2, 2]
```

---

## 10. Sort an Array of 0s and 1s

Given:

```text
[0, 1, 1, 0, 1, 0]
```

Expected:

```text
[0, 0, 0, 1, 1, 1]
```

Try solving it using two pointers.

---

## 11. Squares of a Sorted Array

Given:

```text
[-4, -1, 0, 3, 10]
```

Return:

```text
[0, 1, 9, 16, 100]
```

Try solving this in:

```text
O(n)
```

rather than sorting the squared values.

---

## 12. Valid Palindrome

Given:

```text
"A man, a plan, a canal: Panama"
```

Return:

```text
true
```

Ignore spaces, punctuation, and case.

---

# 50. Practice Problems — Interview Level

After mastering the basic problems, practice:

1. Two Sum II — Input Array Is Sorted
2. 3Sum
3. 4Sum
4. Container With Most Water
5. Trapping Rain Water
6. Remove Duplicates from Sorted Array
7. Remove Duplicates from Sorted Array II
8. Move Zeroes
9. Sort Colors
10. Merge Sorted Arrays
11. Intersection of Two Arrays
12. Squares of a Sorted Array
13. Valid Palindrome
14. Valid Palindrome II
15. Boats to Save People

These problems will make the two-pointer pattern much more natural.

---

# 51. Two-Pointer Cheat Sheet

## Opposite Direction

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {

    // process

    left++;
    right--;
}
```

Used for:

- Reverse
- Palindrome
- Pair Sum
- Container

---

## Same Direction

```java
int slow = 0;

for (int fast = 0; fast < arr.length; fast++) {

    if (condition) {
        // process
        slow++;
    }
}
```

Used for:

- Remove duplicates
- Move zeros
- Remove elements
- Partitioning

---

# 52. Mental Model

Remember these two diagrams.

### Opposite Direction

```text
[ 1  2  3  4  5  6 ]
  ↑               ↑
 left            right

  →             ←
```

---

### Same Direction

```text
[ 1  2  3  4  5  6 ]
  ↑  ↑
slow fast

      →  →
```

Think:

```text
Opposite pointers
    ↓
Compare from both sides

Same-direction pointers
    ↓
Explore + build
```

---

# 53. How to Recognize the Pattern

When you see:

```text
"sorted array"
"pair"
"two numbers"
"palindrome"
"reverse"
"remove duplicates"
"move elements"
"do it in-place"
```

ask yourself:

> **Can two pointers eliminate unnecessary work?**

If yes, try the two-pointer approach.

---

# 54. Final Summary

The Two Pointers technique is not just about using two variables called `left` and `right`.

The real idea is:

> **Use the structure of the problem to move pointers intelligently and avoid unnecessary comparisons.**

The most important patterns are:

```text
1. Left + Right
2. Slow + Fast
```

Remember:

```text
Sorted pair sum:
sum < target → left++

sum > target → right--
```

For palindrome:

```text
left++
right--
```

For slow/fast problems:

```text
fast → explore
slow → build result
```

Typical complexity:

```text
Time  → O(n)
Space → O(1)
```

when each pointer moves through the input a constant number of times.

---

# 55. What You Should Be Able to Do

Before moving to the next topic, make sure you can:

- [ ] Explain the two-pointer technique
- [ ] Explain opposite-direction pointers
- [ ] Explain same-direction pointers
- [ ] Reverse an array using two pointers
- [ ] Check a palindrome
- [ ] Find a pair with a target sum in a sorted array
- [ ] Explain why pointer movement works
- [ ] Remove duplicates from a sorted array
- [ ] Move zeros to the end
- [ ] Solve Container With Most Water
- [ ] Understand the basic idea of 3Sum
- [ ] Identify when two pointers can be used
- [ ] Compare brute force vs two pointers
- [ ] Analyze time and space complexity
- [ ] Explain why sorting may be required

---

# Next Topic

Your array section is now:

```text
02-arrays/
├── arrays.md          ✅
├── two-pointers.md    ✅
├── sliding-window.md  ⏳
└── prefix-sum.md      ⏳
```

Next:

**`02-arrays/sliding-window.md`**

The Sliding Window chapter will build on the array knowledge you have learned here and cover fixed-size windows, variable-size windows, subarrays, frequency maps, longest/shortest subarray problems, and important interview patterns.