# Contains Duplicate

**Difficulty:** Easy  
**Topics:** Array, HashSet, HashMap, Sorting  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array.

Return `false` if every element is distinct.

### Example 1

**Input:**
```java
nums = [1, 2, 3, 1]
```

**Output:**
```java
true
```

**Explanation:**

The number `1` appears twice.

### Example 2

**Input:**
```java
nums = [1, 2, 3, 4]
```

**Output:**
```java
false
```

**Explanation:**

All elements are unique.

### Example 3

**Input:**
```java
nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
```

**Output:**
```java
true
```

**Explanation:**

Multiple elements appear more than once.

---

## 2. Approach 1: Brute Force

### Concept

Compare every element with all the elements that come after it.

If two elements are equal, a duplicate exists.

### Java Code

```java
public class ContainsDuplicateBruteForce {

    public static boolean containsDuplicate(int[] nums) {

        for (int i = 0; i < nums.length; i++) {

            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }

        return false;
    }

    public static void main(String[] args) {

        int[] nums = {1, 2, 3, 1};

        System.out.println(containsDuplicate(nums));
    }
}
```

### Dry Run

Input:

```java
nums = [1, 2, 3, 1]
```

| i | j | nums[i] | nums[j] | Result |
|---|---|---:|---:|---|
| 0 | 1 | 1 | 2 | Not equal |
| 0 | 2 | 1 | 3 | Not equal |
| 0 | 3 | 1 | 1 | Duplicate found |

The method returns `true`.

### Complexity Analysis

- **Time Complexity:** O(n²) because of the nested loops.
- **Space Complexity:** O(1) because no additional data structure is used.

---

## 3. Approach 2: Optimized Using HashSet

### Concept

A HashSet stores unique elements.

It does not allow duplicate values.

We can use this property to identify duplicates efficiently.

For every element:

1. Check whether the element already exists in the HashSet.
2. If it exists, return `true`.
3. Otherwise, add the element to the HashSet.
4. If the loop completes, return `false`.

### Java Code

```java
import java.util.HashSet;
import java.util.Set;

public class ContainsDuplicateOptimized {

    public static boolean containsDuplicate(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (set.contains(num)) {
                return true;
            }

            set.add(num);
        }

        return false;
    }

    public static void main(String[] args) {

        int[] nums = {1, 2, 3, 1};

        System.out.println(containsDuplicate(nums));
    }
}
```

---

## 4. Detailed Dry Run

Input:

```java
nums = [1, 2, 3, 1]
```

Initially:

```java
Set<Integer> set = new HashSet<>();
```

The HashSet is empty.

| Iteration | Current Element | Set Before Check | Action | Set After Action |
|---|---:|---|---|---|
| 1 | 1 | `{}` | Add 1 | `{1}` |
| 2 | 2 | `{1}` | Add 2 | `{1, 2}` |
| 3 | 3 | `{1, 2}` | Add 3 | `{1, 2, 3}` |
| 4 | 1 | `{1, 2, 3}` | Duplicate found | `{1, 2, 3}` |

At iteration 4, the element `1` already exists.

Therefore:

```java
return true;
```

---

## 5. Understanding HashSet in Java

A HashSet is a collection that stores unique elements.

### Declaration

```java
Set<Integer> set = new HashSet<>();
```

### Important Methods

| Method | Description |
|---|---|
| `add(value)` | Adds an element |
| `contains(value)` | Checks whether an element exists |
| `remove(value)` | Removes an element |
| `size()` | Returns the number of elements |
| `isEmpty()` | Checks whether the set is empty |
| `clear()` | Removes all elements |

### Example

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample {

    public static void main(String[] args) {

        Set<Integer> set = new HashSet<>();

        set.add(10);
        set.add(20);
        set.add(30);
        set.add(10);

        System.out.println(set);

        System.out.println(set.contains(20)); // true

        System.out.println(set.size());       // 3

        set.remove(20);

        System.out.println(set.contains(20)); // false
    }
}
```

**Important points:**

- HashSet does not store duplicate elements.
- HashSet does not guarantee insertion order.
- HashSet allows one `null` element.
- `add()` returns `false` if the element is already present.
- Average lookup and insertion time is O(1).

---

## 6. Alternative Optimized Solution Using add()

We can simplify the HashSet solution further.

The `add()` method returns:

- `true` if the element was added successfully.
- `false` if the element already existed.

### Java Code

```java
import java.util.HashSet;
import java.util.Set;

public class ContainsDuplicateUsingAdd {

    public static boolean containsDuplicate(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (!set.add(num)) {
                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {

        int[] nums = {1, 2, 3, 1};

        System.out.println(containsDuplicate(nums));
    }
}
```

### Why Does This Work?

Consider:

```java
set.add(1);
```

The first time:

```java
true
```

The second time:

```java
false
```

Because `1` is already present.

This allows us to detect duplicates without calling `contains()` separately.

---

## 7. Approach 3: Using Sorting

### Concept

Sort the array first.

After sorting, duplicate elements will appear next to each other.

For example:

```java
Original: [4, 1, 3, 2, 1]

Sorted:   [1, 1, 2, 3, 4]
```

Now compare adjacent elements.

If:

```java
nums[i] == nums[i - 1]
```

A duplicate exists.

### Java Code

```java
import java.util.Arrays;

public class ContainsDuplicateSorting {

    public static boolean containsDuplicate(int[] nums) {

        Arrays.sort(nums);

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] == nums[i - 1]) {
                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {

        int[] nums = {4, 1, 3, 2, 1};

        System.out.println(containsDuplicate(nums));
    }
}
```

**Important:** `Arrays.sort(nums)` modifies the original array.

### Complexity Analysis

- **Time Complexity:** O(n log n) for sorting, followed by O(n) traversal.
- **Space Complexity:** Depends on the sorting implementation. Java's primitive array sorting uses an implementation-dependent amount of auxiliary space; do not assume strict O(1).

---

## 8. Compare All Three Approaches

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Brute Force | O(n²) | O(1) |
| HashSet | O(n) average | O(n) |
| Sorting | O(n log n) | Depends on sorting implementation |

**Recommended interview solution:** HashSet.

It provides O(n) average time complexity and is simple to explain.

---

## 9. Edge Cases

| Input | Expected Output | Explanation |
|---|---|---|
| `[1, 2, 3, 1]` | `true` | Duplicate 1 |
| `[1, 2, 3, 4]` | `false` | All unique |
| `[1, 1]` | `true` | Duplicate pair |
| `[1]` | `false` | Only one element |
| `[]` | `false` | Empty array |
| `[-1, -2, -1]` | `true` | Duplicate negative number |

---

## 10. Interview Questions

**Q1. Why do we use HashSet for this problem?**

Because HashSet stores only unique elements and supports average O(1) lookup and insertion.

**Q2. What happens when we add a duplicate element to HashSet?**

The existing element remains unchanged. The `add()` method returns `false`.

**Q3. What is the difference between HashSet and HashMap?**

- HashSet stores unique elements.
- HashMap stores key-value pairs with unique keys.

**Q4. Does HashSet maintain insertion order?**

No. HashSet does not guarantee insertion order. Use LinkedHashSet if insertion order is required.

**Q5. Can we solve this problem without extra space?**

Yes. We can sort the array and compare adjacent elements, although sorting may require auxiliary space depending on the implementation.

**Q6. What is the average time complexity of HashSet contains()?**

O(1) average time. In unfavorable collision scenarios, operations can take longer.

---

## 11. Practice Tasks

Try these variations:

- [ ] Return the first duplicate element.
- [ ] Count how many duplicate values exist.
- [ ] Find all duplicate elements in an array.
- [ ] Find the frequency of each element using HashMap.
- [ ] Solve the problem using sorting without using HashSet.

---

## 12. Key Takeaways

- HashSet stores unique values.
- Use `contains()` to check whether an element already exists.
- Use `add()` to insert a new element.
- The `add()` method returns `false` for an existing element.
- HashSet provides O(1) average insertion and lookup.
- Sorting is another option when extra-space constraints matter.

**Most important pattern to remember:**

```java
Set<Integer> set = new HashSet<>();

for (int num : nums) {

    if (!set.add(num)) {
        return true;
    }
}

return false;
```

This HashSet pattern is useful in many duplicate detection and membership-checking problems.