# Linear Search in Java

## 1. What is Searching?

Searching is the process of finding a specific element in a collection of data.

For example, consider this array:

```java
int[] arr = {10, 20, 30, 40, 50};
```

If we want to find `30`, we need to determine whether it exists and, if it does, return its index.

There are two commonly used searching algorithms:

1. Linear Search
2. Binary Search

In this chapter, we will learn Linear Search.

---

## 2. What is Linear Search?

**Linear Search** is a simple searching algorithm that checks each element one by one until:

- The target element is found, or
- All elements have been checked.

It starts from the first element and moves sequentially through the array.

### Example

Input:

```java
int[] arr = {10, 20, 30, 40, 50};

int target = 30;
```

Searching process:

```text
Array:  [10, 20, 30, 40, 50]
         |
         10 != 30

Array:  [10, 20, 30, 40, 50]
             |
             20 != 30

Array:  [10, 20, 30, 40, 50]
                 |
                 30 == 30
```

Target found at index `2`.

Output:

```text
Element found at index: 2
```

---

## 3. Linear Search Algorithm

Follow these steps:

1. Start from index `0`.
2. Compare the current element with the target.
3. If both are equal, return the current index.
4. Otherwise, move to the next element.
5. If the loop finishes without finding the target, return `-1`.

### Pseudocode

```text
linearSearch(arr, target):

    for i = 0 to arr.length - 1:

        if arr[i] == target:
            return i

    return -1
```

---

## 4. Linear Search Implementation in Java

```java
public class LinearSearch {

    public static int linearSearch(int[] arr, int target) {

        for (int i = 0; i < arr.length; i++) {

            if (arr[i] == target) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50};

        int target = 30;

        int result = linearSearch(arr, target);

        if (result != -1) {
            System.out.println("Element found at index: " + result);
        } else {
            System.out.println("Element not found");
        }
    }
}
```

Output:

```text
Element found at index: 2
```

### Understanding the Code

```java
for (int i = 0; i < arr.length; i++)
```

Iterates through every array element, starting from index `0`.

```java
if (arr[i] == target)
```

Checks whether the current element matches the target.

```java
return i;
```

Returns the index immediately when the element is found.

```java
return -1;
```

Indicates that the target does not exist in the array.

**Important:** In Java, array indexes start from `0`. Returning `-1` is a common convention to indicate that an element was not found.

---

## 5. Dry Run of Linear Search

Consider:

```java
int[] arr = {5, 15, 25, 35, 45};

int target = 35;
```

### Step-by-Step Execution

| Iteration | Index | Current Element | Target | Comparison |
|---|---:|---:|---:|---|
| 1 | 0 | 5 | 35 | Not equal |
| 2 | 1 | 15 | 35 | Not equal |
| 3 | 2 | 25 | 35 | Not equal |
| 4 | 3 | 35 | 35 | Found |

Result:

```text
Element found at index: 3
```

The algorithm stops immediately after finding the target.

---

## 6. Searching for an Element That Does Not Exist

Consider:

```java
int[] arr = {5, 15, 25, 35, 45};

int target = 100;
```

The algorithm checks every element.

| Index | Element | Comparison |
|---:|---:|---|
| 0 | 5 | Not equal |
| 1 | 15 | Not equal |
| 2 | 25 | Not equal |
| 3 | 35 | Not equal |
| 4 | 45 | Not equal |

Since the target is not found, the method returns:

```java
return -1;
```

Output:

```text
Element not found
```

---

## 7. Linear Search with Duplicate Elements

What happens if the target appears multiple times?

Example:

```java
int[] arr = {10, 20, 30, 20, 40, 20};

int target = 20;
```

The basic Linear Search implementation returns the **first occurrence** because it stops when it finds the first match.

Output:

```text
Element found at index: 1
```

### Find All Occurrences

If we want to find every occurrence, we should not return immediately after finding a match.

```java
import java.util.ArrayList;
import java.util.List;

public class LinearSearchAll {

    public static List<Integer> findAllOccurrences(
            int[] arr, int target) {

        List<Integer> indexes = new ArrayList<>();

        for (int i = 0; i < arr.length; i++) {

            if (arr[i] == target) {
                indexes.add(i);
            }
        }

        return indexes;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 20, 40, 20};

        int target = 20;

        List<Integer> result =
                findAllOccurrences(arr, target);

        System.out.println(result);
    }
}
```

Output:

```text
[1, 3, 5]
```

---

## 8. Find the First and Last Occurrence

We can use Linear Search to find both the first and last occurrence of an element.

```java
public class FirstLastOccurrence {

    public static void findOccurrences(
            int[] arr, int target) {

        int first = -1;
        int last = -1;

        for (int i = 0; i < arr.length; i++) {

            if (arr[i] == target) {

                if (first == -1) {
                    first = i;
                }

                last = i;
            }
        }

        System.out.println("First occurrence: " + first);
        System.out.println("Last occurrence: " + last);
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 20, 40, 20};

        findOccurrences(arr, 20);
    }
}
```

Output:

```text
First occurrence: 1
Last occurrence: 5
```

If the target does not exist, both values remain `-1`.

---

## 9. Linear Search Using Enhanced For Loop

We can also implement Linear Search using Java's enhanced `for` loop.

```java
public class LinearSearchEnhanced {

    public static boolean contains(int[] arr, int target) {

        for (int num : arr) {

            if (num == target) {
                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50};

        System.out.println(contains(arr, 30));
        System.out.println(contains(arr, 100));
    }
}
```

Output:

```text
true
false
```

### Difference Between the Two Implementations

| Normal For Loop | Enhanced For Loop |
|---|---|
| Provides array index | Does not directly provide index |
| Can access elements by index | Directly provides each element |
| Useful when index is required | Useful when only element checking is needed |

---

## 10. Linear Search in a String Array

Linear Search can also be used with strings.

Example:

```java
public class StringLinearSearch {

    public static int search(String[] arr, String target) {

        for (int i = 0; i < arr.length; i++) {

            if (arr[i].equals(target)) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        String[] names = {
            "Rahul",
            "Amit",
            "Tejas",
            "Priya"
        };

        String target = "Tejas";

        int result = search(names, target);

        System.out.println("Index: " + result);
    }
}
```

Output:

```text
Index: 2
```

### Important: Use `.equals()` for String Comparison

Incorrect:

```java
if (arr[i] == target)
```

Correct:

```java
if (arr[i].equals(target))
```

The `==` operator compares object references, while `.equals()` compares string contents.

For a case-insensitive search, use:

```java
if (arr[i].equalsIgnoreCase(target))
```

---

## 11. Linear Search in an ArrayList

Linear Search also works with Java collections such as `ArrayList`.

```java
import java.util.ArrayList;
import java.util.Arrays;

public class ArrayListLinearSearch {

    public static int search(
            ArrayList<Integer> list, int target) {

        for (int i = 0; i < list.size(); i++) {

            if (list.get(i) == target) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        ArrayList<Integer> list =
                new ArrayList<>(
                        Arrays.asList(10, 20, 30, 40, 50)
                );

        int result = search(list, 30);

        System.out.println("Index: " + result);
    }
}
```

Output:

```text
Index: 2
```

For an `ArrayList<Integer>`, `list.get(i) == target` works here because `target` is a primitive `int`, so Java unboxes the `Integer` value for comparison. Using `.equals()` is also a clear alternative:

```java
if (list.get(i).equals(target))
```

---

## 12. Time Complexity of Linear Search

Linear Search checks elements sequentially.

Its performance depends on where the target is located.

### Best Case: O(1)

The target is at the first index.

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int target = 10;
```

Only one comparison is required.

### Average Case: O(n)

The target is somewhere around the middle of the array on average.

For an array of `n` elements, the average number of comparisons is approximately `(n + 1) / 2` when the target is equally likely to be at any position.

The time complexity is O(n).

### Worst Case: O(n)

Two situations:

- The target is at the last index.
- The target does not exist in the array.

All `n` elements must be checked.

### Complexity Summary

| Case | Time Complexity |
|---|---|
| Best Case | O(1) |
| Average Case | O(n) |
| Worst Case | O(n) |

### Space Complexity: O(1)

The basic Linear Search algorithm uses only a few variables, regardless of the array size.

Therefore:

```text
Space Complexity: O(1)
```

The `findAllOccurrences()` example is different: it stores matching indexes in a list, requiring O(k) additional space, where `k` is the number of matches.

---

## 13. Advantages and Disadvantages

### Advantages

1. Simple and easy to implement.
2. Works with sorted and unsorted arrays.
3. Does not require additional memory.
4. Works with arrays and other sequential collections.
5. Suitable for small datasets.

### Disadvantages

1. Slow for large datasets.
2. May require checking every element.
3. Less efficient than Binary Search for large, sorted arrays.

---

## 14. Linear Search vs Binary Search

| Feature | Linear Search | Binary Search |
|---|---|---|
| Searching technique | Sequential | Divide and Conquer |
| Sorted array required | No | Yes |
| Best Case | O(1) | O(1) |
| Average Case | O(n) | O(log n) |
| Worst Case | O(n) | O(log n) |
| Implementation | Simple | Slightly more complex |
| Suitable for | Small or unsorted data | Large sorted data |

### Example

Suppose an array contains 1,000 elements.

Linear Search may need to check up to 1,000 elements.

Binary Search needs approximately 10 comparisons in the worst case because it repeatedly divides the search range in half.

This makes Binary Search much more efficient for large, sorted arrays.

---

## 15. Common Mistakes

### Mistake 1: Using `<=` Instead of `<`

Incorrect:

```java
for (int i = 0; i <= arr.length; i++)
```

Correct:

```java
for (int i = 0; i < arr.length; i++)
```

The last valid index is `arr.length - 1`. Using `<=` can cause `ArrayIndexOutOfBoundsException`.

### Mistake 2: Forgetting to Return -1

Incorrect:

```java
public static int search(int[] arr, int target) {

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            return i;
        }
    }

    return 0;
}
```

Returning `0` is incorrect because index `0` is a valid result.

Correct:

```java
return -1;
```

### Mistake 3: Returning Too Early When Finding All Matches

Incorrect:

```java
if (arr[i] == target) {
    return i;
}
```

This returns only the first occurrence.

If all occurrences are required, store the matching indexes and continue the loop.

### Mistake 4: Comparing Strings with `==`

Incorrect:

```java
if (arr[i] == target)
```

Correct:

```java
if (arr[i].equals(target))
```

---

## 16. Frequently Asked Interview Questions

### Q1. What is Linear Search?

Linear Search is a searching algorithm that checks each element sequentially until the target is found or all elements have been checked.

### Q2. Does Linear Search require a sorted array?

No. It works with both sorted and unsorted arrays.

### Q3. What is the worst-case time complexity?

O(n), because all elements may need to be checked.

### Q4. What is the best-case time complexity?

O(1), when the target is found at the first index.

### Q5. What is the space complexity?

O(1) for the basic implementation.

### Q6. Can Linear Search find duplicate elements?

Yes. It can find the first occurrence, last occurrence, or all occurrences depending on the implementation.

### Q7. Why does Linear Search return -1?

Because `-1` is not a valid array index and is commonly used to indicate that the target was not found.

### Q8. Can we use Linear Search on a Linked List?

Yes. We can traverse the linked list node by node until the target is found or the list ends.

### Q9. Which is better: Linear Search or Binary Search?

It depends on the data:

- Linear Search works with unsorted data and is simple for small collections.
- Binary Search is more efficient for large, sorted data with efficient indexed access.

### Q10. Is Linear Search an in-place algorithm?

Yes. The basic implementation uses constant additional space and does not modify the input array.

---

## 17. Practice Problems

### Beginner

1. Search for an element in an array.
2. Find the first occurrence of a number.
3. Find the last occurrence of a number.
4. Count how many times a number appears.
5. Find all indexes of a target element.
6. Search for a name in a String array.
7. Find the maximum element in an array.
8. Find the minimum element in an array.

### Intermediate

1. Find the second-largest element in an array.
2. Find the missing number in an array containing numbers from 1 to n.
3. Find duplicate elements in an array.
4. Find the intersection of two arrays.
5. Search for an element in a 2D array.

### Coding Practice

Try these problems:

| Problem | Difficulty |
|---|---|
| [Find Numbers with Even Number of Digits](https://leetcode.com/problems/find-numbers-with-even-number-of-digits/) | Easy |
| [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | Easy |
| [Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/) | Easy |
| [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/) | Easy |

---

## 18. Quick Revision

- Linear Search checks elements one by one.
- It works on sorted and unsorted arrays.
- It starts from index `0`.
- It returns the index when the target is found.
- It returns `-1` when the target does not exist.
- Best-case time complexity: O(1).
- Average-case time complexity: O(n).
- Worst-case time complexity: O(n).
- Basic space complexity: O(1).
- It can find the first, last, or all occurrences.

---

## 19. Self-Assessment Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain Linear Search.
- [ ] Implement Linear Search in Java.
- [ ] Perform a dry run manually.
- [ ] Find the first occurrence of an element.
- [ ] Find the last occurrence of an element.
- [ ] Find all occurrences of an element.
- [ ] Search in a String array.
- [ ] Search in an ArrayList.
- [ ] Explain best, average, and worst-case complexity.
- [ ] Compare Linear Search with Binary Search.

---

## 20. What's Next?

Next, we will learn **Binary Search**.

File: `13-searching/binary-search.md`

We will cover:

- Binary Search fundamentals.
- Why the array must be sorted.
- Iterative Binary Search in Java.
- Recursive Binary Search in Java.
- Dry runs and complexity analysis.
- First and last occurrence.
- Lower Bound and Upper Bound.
- Common interview problems.