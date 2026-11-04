# Arrays

Arrays are one of the most fundamental data structures in programming and DSA.

Before learning advanced topics such as:

- Two Pointers
- Sliding Window
- Prefix Sum
- Binary Search
- Sorting
- Dynamic Programming

you should be comfortable working with arrays.

---

# 1. What is an Array?

An **array** is a collection of elements stored in a fixed-size sequence.

For example:

```text
[10, 20, 30, 40, 50]
```

Here:

```text
10 → index 0
20 → index 1
30 → index 2
40 → index 3
50 → index 4
```

In Java:

```java
int[] numbers = {10, 20, 30, 40, 50};
```

---

# 2. Array Indexing

Array indexes start from `0`.

For:

```java
int[] arr = {10, 20, 30, 40, 50};
```

the indexes are:

```text
Index:   0   1   2   3   4
Value:  10  20  30  40  50
```

Therefore:

```java
System.out.println(arr[0]);
```

Output:

```text
10
```

And:

```java
System.out.println(arr[3]);
```

Output:

```text
40
```

---

# 3. Why Does Array Index Start From 0?

An array index represents the **offset from the beginning** of the array.

The first element has an offset of:

```text
0
```

The second:

```text
1
```

The third:

```text
2
```

Therefore:

```text
index = position - 1
```

So the element at position `5` has index:

```text
5 - 1 = 4
```

---

# 4. Creating an Array in Java

There are several ways to create arrays.

## Method 1 — Declare and Initialize

```java
int[] arr = {10, 20, 30, 40};
```

---

## Method 2 — Create with Size

```java
int[] arr = new int[5];
```

This creates:

```text
[0, 0, 0, 0, 0]
```

The default value for an `int` array is `0`.

---

## Method 3 — Assign Values Later

```java
int[] arr = new int[5];

arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
arr[3] = 40;
arr[4] = 50;
```

Result:

```text
[10, 20, 30, 40, 50]
```

---

# 5. Array Size

Use:

```java
arr.length
```

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr.length);
```

Output:

```text
5
```

Important:

```java
arr.length
```

is a property, not a method.

Correct:

```java
arr.length
```

Incorrect:

```java
arr.length()
```

---

# 6. Accessing Elements

Array elements can be accessed using their index.

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr[0]);
System.out.println(arr[2]);
System.out.println(arr[4]);
```

Output:

```text
10
30
50
```

Accessing an element is very fast.

### Time Complexity

```text
O(1)
```

---

# 7. Updating an Element

We can change an element using its index.

```java
int[] arr = {10, 20, 30, 40, 50};

arr[2] = 100;
```

Array becomes:

```text
[10, 20, 100, 40, 50]
```

### Time Complexity

```text
O(1)
```

Because we directly access the index.

---

# 8. Traversing an Array

Traversal means visiting every element of an array.

There are several ways to traverse an array.

---

## 8.1 Using a for Loop

This is the most common approach in DSA.

```java
int[] arr = {10, 20, 30, 40, 50};

for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Output:

```text
10
20
30
40
50
```

Time complexity:

```text
O(n)
```

because every element is visited once.

---

# 9. Enhanced for Loop

Java also provides a for-each loop.

```java
int[] arr = {10, 20, 30, 40, 50};

for (int value : arr) {
    System.out.println(value);
}
```

Output:

```text
10
20
30
40
50
```

This is useful when you only need the values and don't need the indexes.

---

# 10. for Loop vs for-each

### Normal for Loop

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Use this when you need:

- index
- previous element
- next element
- modifying elements
- comparing positions

---

### Enhanced for Loop

```java
for (int value : arr) {
    System.out.println(value);
}
```

Use this when you simply need to read each value.

---

# 11. Reverse Traversal

We can traverse an array from the last element to the first.

```java
int[] arr = {10, 20, 30, 40, 50};

for (int i = arr.length - 1; i >= 0; i--) {
    System.out.println(arr[i]);
}
```

Output:

```text
50
40
30
20
10
```

---

# 12. Finding the Sum of an Array

Suppose:

```text
[10, 20, 30, 40]
```

We want:

```text
10 + 20 + 30 + 40 = 100
```

Java:

```java
static int arraySum(int[] arr) {

    int sum = 0;

    for (int i = 0; i < arr.length; i++) {
        sum += arr[i];
    }

    return sum;
}
```

Usage:

```java
int[] arr = {10, 20, 30, 40};

System.out.println(arraySum(arr));
```

Output:

```text
100
```

### Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 13. Finding the Maximum Element

Given:

```text
[10, 25, 7, 40, 15]
```

Maximum:

```text
40
```

Code:

```java
static int findMax(int[] arr) {

    int max = arr[0];

    for (int i = 1; i < arr.length; i++) {

        if (arr[i] > max) {
            max = arr[i];
        }
    }

    return max;
}
```

---

# 14. Dry Run — Maximum

Given:

```text
[10, 25, 7, 40, 15]
```

Initially:

```text
max = 10
```

Compare with `25`:

```text
25 > 10
max = 25
```

Compare with `7`:

```text
7 > 25 → false
max = 25
```

Compare with `40`:

```text
40 > 25
max = 40
```

Compare with `15`:

```text
15 > 40 → false
```

Final:

```text
max = 40
```

### Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 15. Finding the Minimum Element

```java
static int findMin(int[] arr) {

    int min = arr[0];

    for (int i = 1; i < arr.length; i++) {

        if (arr[i] < min) {
            min = arr[i];
        }
    }

    return min;
}
```

For:

```text
[10, 25, 7, 40, 15]
```

Result:

```text
7
```

---

# 16. Linear Search

Linear search means checking elements one by one.

Suppose:

```text
arr = [10, 20, 30, 40, 50]
target = 30
```

We check:

```text
10 → no
20 → no
30 → yes
```

Code:

```java
static int linearSearch(int[] arr, int target) {

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            return i;
        }
    }

    return -1;
}
```

Usage:

```java
int index = linearSearch(arr, 30);

System.out.println(index);
```

Output:

```text
2
```

If the element doesn't exist:

```text
-1
```

### Complexity

Best case:

```text
O(1)
```

Worst case:

```text
O(n)
```

Average case:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 17. Why Return -1?

Array indexes start from:

```text
0
```

So `-1` is not a valid array index.

Therefore it can represent:

```text
Element not found
```

This is a common convention in Java search methods.

---

# 18. Count Occurrences

Problem:

Count how many times a target appears.

Given:

```text
[10, 20, 10, 30, 10]
```

Target:

```text
10
```

Answer:

```text
3
```

Code:

```java
static int countOccurrences(int[] arr, int target) {

    int count = 0;

    for (int value : arr) {

        if (value == target) {
            count++;
        }
    }

    return count;
}
```

---

# 19. Check if Array Contains an Element

```java
static boolean contains(int[] arr, int target) {

    for (int value : arr) {

        if (value == target) {
            return true;
        }
    }

    return false;
}
```

Example:

```java
int[] arr = {10, 20, 30};

System.out.println(contains(arr, 20));
```

Output:

```text
true
```

---

# 20. Find the Second Largest Element

Given:

```text
[10, 30, 20, 50, 40]
```

Largest:

```text
50
```

Second largest:

```text
40
```

A common approach is to maintain two variables.

```java
static int secondLargest(int[] arr) {

    int largest = Integer.MIN_VALUE;
    int secondLargest = Integer.MIN_VALUE;

    for (int value : arr) {

        if (value > largest) {
            secondLargest = largest;
            largest = value;
        } else if (value > secondLargest && value != largest) {
            secondLargest = value;
        }
    }

    return secondLargest;
}
```

### Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 21. Reverse an Array

Given:

```text
[10, 20, 30, 40, 50]
```

Expected:

```text
[50, 40, 30, 20, 10]
```

We can use two pointers:

```text
left → beginning
right → end
```

Code:

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

# 22. Dry Run — Reverse Array

Given:

```text
[10, 20, 30, 40, 50]
```

Initially:

```text
left = 0
right = 4
```

Swap:

```text
10 ↔ 50
```

Array:

```text
[50, 20, 30, 40, 10]
```

Move pointers:

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

Stop.

Final:

```text
[50, 40, 30, 20, 10]
```

---

# 23. Why Is Reverse O(n)?

Every element doesn't necessarily need an individual operation.

We swap pairs:

```text
1st ↔ last
2nd ↔ second last
3rd ↔ third last
```

Approximately:

```text
n / 2
```

operations are performed.

Ignoring constants:

```text
O(n / 2) = O(n)
```

Space:

```text
O(1)
```

because we only use a temporary variable and two pointers.

---

# 24. Copying an Array

We can use:

```java
int[] copy = arr.clone();
```

Example:

```java
int[] arr = {10, 20, 30};

int[] copy = arr.clone();
```

Now:

```text
arr  → [10, 20, 30]
copy → [10, 20, 30]
```

The arrays are separate objects.

---

# 25. Arrays Are Fixed Size

One important limitation of Java arrays is that their size cannot be changed after creation.

Example:

```java
int[] arr = new int[5];
```

This array always has:

```text
length = 5
```

You cannot directly increase it to:

```text
length = 10
```

Instead, you need to create another array.

For dynamic-size collections, Java provides:

```java
ArrayList<Integer>
```

We will focus on `ArrayList` separately when discussing Java collections.

---

# 26. Insertion in an Array

Suppose:

```text
[10, 20, 30, 40]
```

We want to insert:

```text
25
```

between `20` and `30`.

Because arrays have fixed size, we need space available.

Suppose:

```text
[10, 20, 30, 40, 0]
```

We need to shift elements:

```text
40 → right
30 → right
```

Result:

```text
[10, 20, 25, 30, 40]
```

This requires shifting elements.

### Time Complexity

Inserting near the beginning:

```text
O(n)
```

because many elements may need to move.

---

# 27. Deletion in an Array

Suppose:

```text
[10, 20, 30, 40, 50]
```

We want to remove:

```text
30
```

We shift:

```text
40 → left
50 → left
```

Result:

```text
[10, 20, 40, 50]
```

Again, shifting may require:

```text
O(n)
```

time.

---

# 28. Array Operation Complexity

| Operation | Time Complexity |
|---|---:|
| Access by index | O(1) |
| Update by index | O(1) |
| Search | O(n) |
| Traversal | O(n) |
| Find minimum | O(n) |
| Find maximum | O(n) |
| Insert at beginning | O(n) |
| Insert at middle | O(n) |
| Delete from beginning | O(n) |
| Delete from middle | O(n) |
| Reverse | O(n) |

The exact complexity of insertion/deletion depends on how much shifting is required.

---

# 29. Array Memory

Conceptually, an array stores elements in an ordered sequence.

For example:

```text
[10, 20, 30, 40]
```

You can visualize it as:

```text
Index
  ↓
+----+----+----+----+
| 10 | 20 | 30 | 40 |
+----+----+----+----+
  0    1    2    3
```

This makes direct index access efficient.

---

# 30. Why Access Is O(1)

When we write:

```java
arr[3]
```

Java doesn't need to scan:

```text
arr[0]
arr[1]
arr[2]
```

It directly accesses index `3`.

Therefore:

```text
arr[index]
```

takes:

```text
O(1)
```

time.

This is one of the biggest advantages of arrays.

---

# 31. Array Bounds

If:

```java
int[] arr = {10, 20, 30};
```

valid indexes are:

```text
0
1
2
```

This is invalid:

```java
System.out.println(arr[3]);
```

Java throws:

```text
ArrayIndexOutOfBoundsException
```

Therefore always remember:

```text
0 <= index < arr.length
```

---

# 32. Common Array Loop

One of the most important patterns in DSA is:

```java
for (int i = 0; i < arr.length; i++) {
    // process arr[i]
}
```

You will see this pattern repeatedly.

For example:

```java
for (int i = 0; i < arr.length; i++) {

    if (arr[i] > 0) {
        System.out.println(arr[i]);
    }
}
```

This pattern is the foundation for many array problems.

---

# 33. Find Even Numbers

```java
static void printEvenNumbers(int[] arr) {

    for (int value : arr) {

        if (value % 2 == 0) {
            System.out.println(value);
        }
    }
}
```

Given:

```text
[1, 2, 3, 4, 5, 6]
```

Output:

```text
2
4
6
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 34. Find the Average

Given:

```text
[10, 20, 30]
```

Average:

```text
(10 + 20 + 30) / 3
= 20
```

Code:

```java
static double average(int[] arr) {

    int sum = 0;

    for (int value : arr) {
        sum += value;
    }

    return (double) sum / arr.length;
}
```

---

# 35. Check if Array Is Sorted

An array is sorted in ascending order if:

```text
arr[i] <= arr[i + 1]
```

for every valid `i`.

Example:

```text
[10, 20, 30, 40]
```

is sorted.

But:

```text
[10, 30, 20, 40]
```

is not sorted.

Code:

```java
static boolean isSorted(int[] arr) {

    for (int i = 0; i < arr.length - 1; i++) {

        if (arr[i] > arr[i + 1]) {
            return false;
        }
    }

    return true;
}
```

### Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 36. Find the First Occurrence

Given:

```text
[10, 20, 10, 30, 10]
```

Target:

```text
10
```

First occurrence:

```text
index 0
```

Code:

```java
static int firstOccurrence(int[] arr, int target) {

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            return i;
        }
    }

    return -1;
}
```

This is the same basic idea as linear search.

---

# 37. Find the Last Occurrence

We can traverse from the end.

```java
static int lastOccurrence(int[] arr, int target) {

    for (int i = arr.length - 1; i >= 0; i--) {

        if (arr[i] == target) {
            return i;
        }
    }

    return -1;
}
```

For:

```text
[10, 20, 10, 30, 10]
```

target:

```text
10
```

Result:

```text
4
```

---

# 38. Move All Zeros to the End

Given:

```text
[0, 1, 0, 3, 12]
```

Expected:

```text
[1, 3, 12, 0, 0]
```

One efficient approach uses a pointer.

```java
static void moveZeros(int[] arr) {

    int index = 0;

    for (int value : arr) {

        if (value != 0) {
            arr[index] = value;
            index++;
        }
    }

    while (index < arr.length) {
        arr[index] = 0;
        index++;
    }
}
```

### Complexity

Time:

```text
O(n)
```

Space:

```text
O(1)
```

This is an example of an **in-place array algorithm**.

---

# 39. What Does In-Place Mean?

An algorithm is called **in-place** when it modifies the original data structure instead of creating another large data structure.

For example:

```java
reverse(arr);
```

modifies:

```text
arr
```

directly.

It does not create:

```text
newArr
```

with the same size.

Therefore reverse can use:

```text
O(1)
```

extra space.

---

# 40. Prefix of an Array

A prefix means elements from the beginning up to a certain index.

Given:

```text
[10, 20, 30, 40, 50]
```

Prefixes include:

```text
[10]
[10, 20]
[10, 20, 30]
[10, 20, 30, 40]
[10, 20, 30, 40, 50]
```

Prefix concepts become very important when we learn:

```text
02-arrays/prefix-sum.md
```

---

# 41. Subarray

A **subarray** is a contiguous part of an array.

Given:

```text
[1, 2, 3, 4]
```

Valid subarrays include:

```text
[1]
[2]
[3]
[4]

[1, 2]
[2, 3]
[3, 4]

[1, 2, 3]
[2, 3, 4]

[1, 2, 3, 4]
```

But:

```text
[1, 3]
```

is not a subarray because the elements are not contiguous.

Subarrays are extremely important in:

- Sliding Window
- Prefix Sum
- Kadane's Algorithm
- Two Pointers

---

# 42. Subsequence vs Subarray

This distinction is important in interviews.

### Subarray

Elements must be contiguous.

```text
[1, 2, 3, 4]

[2, 3] → subarray
```

---

### Subsequence

Elements do not need to be contiguous, but their order is preserved.

For:

```text
[1, 2, 3, 4]
```

this is a subsequence:

```text
[1, 3, 4]
```

because the order is preserved.

But:

```text
[3, 1]
```

is not a subsequence because the order changed.

---

# 43. Array vs ArrayList

Java provides both arrays and `ArrayList`.

### Array

```java
int[] arr = new int[5];
```

Size is fixed.

### ArrayList

```java
ArrayList<Integer> list = new ArrayList<>();
```

Size can grow or shrink.

| Feature | Array | ArrayList |
|---|---|---|
| Size | Fixed | Dynamic |
| Primitive types | Yes | No, uses wrapper types |
| `length` | Yes | No |
| `size()` | No | Yes |
| Random access | O(1) | O(1) |
| Insert middle | O(n) | O(n) |
| Delete middle | O(n) | O(n) |

For DSA problems, arrays are extremely common.

---

# 44. Common Mistakes

## Mistake 1 — Off-by-One Error

Wrong:

```java
for (int i = 0; i <= arr.length; i++) {
    System.out.println(arr[i]);
}
```

When `i == arr.length`, the index is invalid.

Correct:

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

---

## Mistake 2 — Starting at Index 1

Wrong when you want to process every element:

```java
for (int i = 1; i < arr.length; i++)
```

This skips:

```text
arr[0]
```

Sometimes this is intentional, such as when comparing an element with its previous element.

---

## Mistake 3 — Using arr.length()

Wrong:

```java
arr.length()
```

Correct:

```java
arr.length
```

---

## Mistake 4 — Accessing Invalid Index

```java
arr[arr.length]
```

is invalid.

The last valid index is:

```text
arr.length - 1
```

---

# 45. Array Problem-Solving Pattern

When you receive an array problem, first ask:

### Question 1

Do I need to visit every element?

If yes, probably:

```text
O(n)
```

---

### Question 2

Do I need the index?

If yes, use:

```java
for (int i = 0; i < arr.length; i++)
```

---

### Question 3

Do I need to compare two elements?

You may need:

```text
arr[i]
arr[i + 1]
```

or:

```text
arr[left]
arr[right]
```

This leads to patterns such as **Two Pointers**.

---

### Question 4

Is the array sorted?

If yes, you may be able to use:

- Binary Search
- Two Pointers

---

### Question 5

Does the problem ask about a contiguous range?

Think about:

- Sliding Window
- Prefix Sum
- Kadane's Algorithm

---

### Question 6

Does the problem ask about frequencies?

Think about:

- HashMap
- HashSet

---

# 46. Important Array Patterns

You will later study these patterns in detail.

```text
Arrays
│
├── Traversal
│
├── Linear Search
│
├── Two Pointers
│
├── Sliding Window
│
├── Prefix Sum
│
├── Sorting
│
├── Binary Search
│
├── Frequency Counting
│
└── Kadane's Algorithm
```

---

# 47. Interview Questions

## Q1. What is an array?

An array is a collection of elements stored in an ordered sequence and accessed using indexes.

---

## Q2. What is the first index of an array?

```text
0
```

---

## Q3. What is the time complexity of accessing an array element?

```text
O(1)
```

---

## Q4. Why is array access O(1)?

Because the element can be accessed directly using its index.

---

## Q5. What is the time complexity of searching an unsorted array?

```text
O(n)
```

using linear search.

---

## Q6. What is the time complexity of updating an array element?

```text
O(1)
```

if the index is known.

---

## Q7. What is the last valid index?

```text
arr.length - 1
```

---

## Q8. What happens if you access an invalid index in Java?

Java throws:

```text
ArrayIndexOutOfBoundsException
```

---

## Q9. Can the size of a Java array be changed after creation?

No.

Java arrays have a fixed size.

---

## Q10. What is an in-place algorithm?

An algorithm that modifies the original data structure using little or no additional data structure.

---

## Q11. What is a subarray?

A contiguous portion of an array.

---

## Q12. What is the difference between a subarray and subsequence?

A subarray must be contiguous.

A subsequence does not need to be contiguous, but the order of elements is maintained.

---

# 48. Practice Problems — Easy

Try solving these without looking at the solution.

### 1. Print all elements

```text
Input:
[10, 20, 30, 40]

Output:
10 20 30 40
```

---

### 2. Find the sum

```text
Input:
[1, 2, 3, 4, 5]

Output:
15
```

---

### 3. Find maximum

```text
Input:
[10, 50, 20, 40]

Output:
50
```

---

### 4. Find minimum

```text
Input:
[10, 5, 20, 3]

Output:
3
```

---

### 5. Search an element

```text
Input:
[10, 20, 30, 40]
target = 30

Output:
2
```

---

### 6. Count occurrences

```text
Input:
[1, 2, 2, 3, 2]
target = 2

Output:
3
```

---

### 7. Check if an element exists

```text
Input:
[10, 20, 30]
target = 40

Output:
false
```

---

### 8. Reverse an array

```text
Input:
[1, 2, 3, 4, 5]

Output:
[5, 4, 3, 2, 1]
```

---

# 49. Practice Problems — Medium

### 9. Find the second largest element

```text
Input:
[10, 20, 40, 30, 50]

Output:
40
```

---

### 10. Check if array is sorted

```text
Input:
[1, 2, 3, 4, 5]

Output:
true
```

---

### 11. Move zeros to the end

```text
Input:
[0, 1, 0, 3, 12]

Output:
[1, 3, 12, 0, 0]
```

---

### 12. Find first occurrence

```text
Input:
[5, 1, 5, 3, 5]
target = 5

Output:
0
```

---

### 13. Find last occurrence

```text
Input:
[5, 1, 5, 3, 5]
target = 5

Output:
4
```

---

### 14. Find duplicate elements

Example:

```text
Input:
[1, 2, 3, 2, 4, 1]

Output:
1, 2
```

This problem will introduce the importance of hashing.

---

### 15. Find the missing number

Given numbers from `1` to `n`, one number is missing.

```text
Input:
[1, 2, 4, 5]

Output:
3
```

There are multiple ways to solve this.

---

# 50. Practice Problems — Important Interview Problems

After completing the basic problems, practice these:

### 16. Two Sum

```text
Input:
[2, 7, 11, 15]
target = 9

Output:
[0, 1]
```

This problem introduces:

- Brute force
- HashMap
- Time-space tradeoff

---

### 17. Maximum Subarray

```text
Input:
[-2,1,-3,4,-1,2,1,-5,4]

Output:
6
```

This introduces:

**Kadane's Algorithm**

---

### 18. Best Time to Buy and Sell Stock

```text
Input:
[7,1,5,3,6,4]

Output:
5
```

This introduces a one-pass array pattern.

---

### 19. Rotate Array

```text
Input:
[1,2,3,4,5]
k = 2

Output:
[4,5,1,2,3]
```

---

### 20. Majority Element

Find the element appearing more than `n / 2` times.

```text
Input:
[2,2,1,1,1,2,2]

Output:
2
```

This introduces:

**Boyer-Moore Voting Algorithm**

---

# 51. Array Complexity Cheat Sheet

| Operation | Complexity |
|---|---:|
| Access | O(1) |
| Update | O(1) |
| Traversal | O(n) |
| Linear Search | O(n) |
| Find Min | O(n) |
| Find Max | O(n) |
| Reverse | O(n) |
| Insert at Beginning | O(n) |
| Insert at Middle | O(n) |
| Delete at Beginning | O(n) |
| Delete at Middle | O(n) |

Remember:

```text
Access by index → O(1)
Search → O(n)
Traversal → O(n)
```

---

# 52. Quick Revision

### Array

Collection of elements accessed using indexes.

### First Index

```text
0
```

### Last Index

```text
arr.length - 1
```

### Access

```text
arr[index]
```

Complexity:

```text
O(1)
```

### Traversal

```java
for (int i = 0; i < arr.length; i++)
```

Complexity:

```text
O(n)
```

### Search

Unsorted array:

```text
O(n)
```

### Array Size

Fixed after creation.

### In-Place

Modify the original array instead of creating another large array.

### Subarray

Contiguous elements.

### Subsequence

Order preserved, but elements don't have to be contiguous.

---

# 53. What You Should Be Able to Do

Before moving to the next array topic, make sure you can:

- [ ] Create an array
- [ ] Access elements
- [ ] Update elements
- [ ] Traverse an array
- [ ] Traverse backwards
- [ ] Find sum
- [ ] Find minimum
- [ ] Find maximum
- [ ] Perform linear search
- [ ] Count occurrences
- [ ] Find first occurrence
- [ ] Find last occurrence
- [ ] Reverse an array
- [ ] Find second largest
- [ ] Check if an array is sorted
- [ ] Move zeros to the end
- [ ] Understand in-place operations
- [ ] Understand subarray
- [ ] Understand subsequence
- [ ] Analyze array time complexity

---

# 54. Final Mental Model

When you see an array problem, don't immediately start coding.

First ask:

```text
Is the array sorted?
        │
        ├── Yes → Binary Search / Two Pointers may help
        │
        └── No
             │
             ├── Need every element?
             │      → Traversal
             │
             ├── Need frequencies?
             │      → HashMap / HashSet
             │
             ├── Need contiguous elements?
             │      → Sliding Window / Prefix Sum
             │
             ├── Need pair of elements?
             │      → Two Pointers / HashMap
             │
             └── Need maximum subarray?
                    → Kadane's Algorithm
```

This way of **identifying the pattern before coding** is one of the most important skills you will develop while learning DSA.

---

# Next Topic

After this chapter, continue with:

```text
02-arrays/
├── arrays.md          ✅
├── two-pointers.md    ⏳
├── sliding-window.md  ⏳
└── prefix-sum.md      ⏳
```

The next chapter is:

**`02-arrays/two-pointers.md`**

In that chapter, we will learn the two-pointer pattern from scratch, including:

- What two pointers mean
- Opposite-direction pointers
- Same-direction pointers
- Reversing arrays
- Pair sum
- Two Sum on sorted arrays
- Removing duplicates
- Moving zeros
- Palindrome
- Container With Most Water
- 3Sum
- Detailed dry runs
- Time and space complexity
- Interview questions
- Practice problems