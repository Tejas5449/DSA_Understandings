# HashMap and HashSet in Java

## 1. Introduction to Hashing

Hashing is a technique used to store and retrieve data efficiently.

Instead of searching through every element, hashing uses a **hash function** to calculate where data should be stored.

For example, suppose we want to store student details:

| Roll Number | Name |
|---|---|
| 101 | Rahul |
| 102 | Amit |
| 103 | Priya |

If we want to find the student with roll number `102`, a normal list might require checking elements one by one.

Using a HashMap, we can directly retrieve the value associated with key `102` in average O(1) time.

### Why do we need hashing?

- Fast searching.
- Fast insertion and deletion.
- Efficient frequency counting.
- Detecting duplicates.
- Solving problems involving pairs and unique elements.

---

## 2. What Is a Hash Function?

A hash function converts a key into an integer called a hash code.

Java provides the `hashCode()` method for this purpose.

Example:

```java
String name = "Java";

int hash = name.hashCode();

System.out.println(hash);
```

The hash code helps determine the bucket where an element should be stored.

### How hashing works

Suppose we insert the following keys:

```text
Apple
Banana
Mango
Orange
```

The HashMap performs these steps:

1. Calculates the key's hash code.
2. Uses the hash to identify a bucket.
3. Stores the key-value pair in that bucket.
4. When searching, calculates the hash again and checks the appropriate bucket.

**Important:** Different keys can produce the same hash code or map to the same bucket. This is called a collision.

---

## 3. What Is HashMap?

A `HashMap` stores data in key-value pairs.

Each key must be unique, but values can be duplicated.

### Syntax

```java
HashMap<KeyType, ValueType> map = new HashMap<>();
```

### Example

```java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {

        HashMap<Integer, String> students = new HashMap<>();

        students.put(101, "Rahul");
        students.put(102, "Amit");
        students.put(103, "Priya");

        System.out.println(students);
    }
}
```

Possible output:

```text
{101=Rahul, 102=Amit, 103=Priya}
```

The iteration order is not guaranteed.

### Important HashMap methods

| Method | Description |
|---|---|
| `put(key, value)` | Inserts or updates a key-value pair |
| `get(key)` | Returns the value associated with a key |
| `getOrDefault(key, defaultValue)` | Returns the value or a default |
| `containsKey(key)` | Checks whether a key exists |
| `containsValue(value)` | Checks whether a value exists |
| `remove(key)` | Removes a key-value pair |
| `putIfAbsent(key, value)` | Inserts only if the key is absent |
| `size()` | Returns the number of entries |
| `isEmpty()` | Checks whether the map is empty |
| `clear()` | Removes all entries |

### Example: Basic operations

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        HashMap<Integer, String> map = new HashMap<>();

        // Insert
        map.put(1, "Java");
        map.put(2, "Python");
        map.put(3, "Angular");

        // Retrieve
        System.out.println(map.get(1));

        // Check key
        System.out.println(map.containsKey(2));

        // Check value
        System.out.println(map.containsValue("Java"));

        // Update
        map.put(1, "Spring Boot");

        // Default value
        System.out.println(map.getOrDefault(4, "Not Found"));

        // Insert only if key is absent
        map.putIfAbsent(4, "JavaScript");

        // Remove
        map.remove(3);

        System.out.println(map);
    }
}
```

---

## 4. Iterating Through a HashMap

There are different ways to iterate through a HashMap.

### 4.1 Iterate using entrySet()

This is generally the preferred approach when both keys and values are needed.

```java
HashMap<Integer, String> map = new HashMap<>();

map.put(1, "Java");
map.put(2, "Python");
map.put(3, "Angular");

for (Map.Entry<Integer, String> entry : map.entrySet()) {

    System.out.println(
        entry.getKey() + " : " + entry.getValue()
    );
}
```

### 4.2 Iterate through keys

```java
for (Integer key : map.keySet()) {
    System.out.println(key);
}
```

### 4.3 Iterate through values

```java
for (String value : map.values()) {
    System.out.println(value);
}
```

### 4.4 Using forEach()

```java
map.forEach((key, value) -> {
    System.out.println(key + " : " + value);
});
```

---

## 5. What Is HashSet?

A `HashSet` stores unique elements.

It does not store duplicate values.

Internally, Java implements HashSet using a HashMap.

### Syntax

```java
HashSet<DataType> set = new HashSet<>();
```

### Example

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        HashSet<Integer> set = new HashSet<>();

        set.add(10);
        set.add(20);
        set.add(30);
        set.add(20);

        System.out.println(set);
    }
}
```

Possible output:

```text
[20, 10, 30]
```

Notice that `20` appears only once. The order is not guaranteed.

### Important HashSet methods

| Method | Description |
|---|---|
| `add(value)` | Adds an element |
| `remove(value)` | Removes an element |
| `contains(value)` | Checks whether an element exists |
| `size()` | Returns the number of elements |
| `isEmpty()` | Checks whether the set is empty |
| `clear()` | Removes all elements |

### Example

```java
HashSet<String> languages = new HashSet<>();

languages.add("Java");
languages.add("Python");
languages.add("JavaScript");

System.out.println(languages.contains("Java"));

languages.remove("Python");

System.out.println(languages.size());
```

---

## 6. HashMap vs HashSet

| Feature | HashMap | HashSet |
|---|---|---|
| Stores | Key-value pairs | Unique elements |
| Duplicate keys | Not allowed | Not applicable |
| Duplicate values | Allowed | Not allowed |
| Main method | `put()` | `add()` |
| Retrieve data | `get(key)` | `contains(value)` |
| Internal implementation | Hash table | Backed by HashMap |
| Null support | One null key; multiple null values | One null element |

### When should you use them?

Use **HashMap** when:
- You need to store a relationship between two values.
- You need frequency counts.
- You need to find a value using a key.
- You need to remember an element's index.

Use **HashSet** when:
- You only need unique elements.
- You need to detect duplicates.
- You need to check whether an element already exists.

---

## 7. Important Problem: Frequency Counting

Frequency counting is one of the most common HashMap patterns in DSA.

### Problem

Given an array, count how many times each number appears.

Input:

```text
[1, 2, 2, 3, 1, 1, 4]
```

Output:

```text
1 -> 3
2 -> 2
3 -> 1
4 -> 1
```

### Java solution

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 3, 1, 1, 4};

        HashMap<Integer, Integer> frequency = new HashMap<>();

        for (int num : arr) {

            frequency.put(
                num,
                frequency.getOrDefault(num, 0) + 1
            );
        }

        System.out.println(frequency);
    }
}
```

### Dry run

| Number | Previous frequency | Updated frequency |
|---|---:|---:|
| 1 | 0 | 1 |
| 2 | 0 | 1 |
| 2 | 1 | 2 |
| 3 | 0 | 1 |
| 1 | 1 | 2 |
| 1 | 2 | 3 |
| 4 | 0 | 1 |

**Time Complexity:** O(n) average.

**Space Complexity:** O(k), where k is the number of distinct elements.

---

## 8. Important Problem: Find Duplicates

### Problem

Given an array, determine whether it contains any duplicate element.

Input:

```text
[1, 2, 3, 4, 2]
```

Output:

```text
true
```

### Java solution using HashSet

```java
import java.util.*;

public class Main {

    static boolean containsDuplicate(int[] nums) {

        HashSet<Integer> seen = new HashSet<>();

        for (int num : nums) {

            if (seen.contains(num)) {
                return true;
            }

            seen.add(num);
        }

        return false;
    }

    public static void main(String[] args) {

        int[] nums = {1, 2, 3, 4, 2};

        System.out.println(containsDuplicate(nums));
    }
}
```

### Better approach using add()

The `add()` method returns `false` if the element already exists.

```java
static boolean containsDuplicate(int[] nums) {

    Set<Integer> seen = new HashSet<>();

    for (int num : nums) {

        if (!seen.add(num)) {
            return true;
        }
    }

    return false;
}
```

**Time Complexity:** O(n) average.

**Space Complexity:** O(n).

---

## 9. Important Problem: Two Sum

This is a very common interview question.

### Problem

Given an integer array and a target value, find the indices of two numbers whose sum equals the target.

Input:

```text
nums = [2, 7, 11, 15]
target = 9
```

Output:

```text
[0, 1]
```

Because:

```text
nums[0] + nums[1] = 2 + 7 = 9
```

### Approach using HashMap

Instead of checking every possible pair, store previously visited numbers and their indices.

For each number:

1. Calculate the required number: `target - currentNumber`.
2. Check whether the required number already exists in the map.
3. If it exists, return both indices.
4. Otherwise, store the current number and its index.

### Java solution

```java
import java.util.*;

public class Main {

    static int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int required = target - nums[i];

            if (map.containsKey(required)) {

                return new int[] {
                    map.get(required),
                    i
                };
            }

            map.put(nums[i], i);
        }

        return new int[] {-1, -1};
    }

    public static void main(String[] args) {

        int[] nums = {2, 7, 11, 15};

        int target = 9;

        System.out.println(
            Arrays.toString(twoSum(nums, target))
        );
    }
}
```

### Dry run

Input:

```text
nums = [2, 7, 11, 15]
target = 9
```

| Index | Current number | Required number | Map before checking | Result |
|---:|---:|---:|---|---|
| 0 | 2 | 7 | `{}` | Store `2 → 0` |
| 1 | 7 | 2 | `{2=0}` | Found! Return `[0, 1]` |

**Time Complexity:** O(n) average.

**Space Complexity:** O(n).

**Important:** Check for the required number before inserting the current number. This prevents using the same array element twice.

---

## 10. Important Problem: First Non-Repeating Character

### Problem

Find the first character that appears only once in a string.

Input:

```text
swiss
```

Output:

```text
w
```

Explanation:

- `s` appears three times.
- `w` appears once.
- `i` appears once.
- The first non-repeating character is `w`.

### Java solution

```java
import java.util.*;

public class Main {

    static Character firstNonRepeating(String str) {

        HashMap<Character, Integer> frequency = new HashMap<>();

        // Count character frequency
        for (char ch : str.toCharArray()) {

            frequency.put(
                ch,
                frequency.getOrDefault(ch, 0) + 1
            );
        }

        // Find the first character with frequency 1
        for (char ch : str.toCharArray()) {

            if (frequency.get(ch) == 1) {
                return ch;
            }
        }

        return null;
    }

    public static void main(String[] args) {

        String str = "swiss";

        System.out.println(firstNonRepeating(str));
    }
}
```

**Time Complexity:** O(n) average.

**Space Complexity:** O(k), where k is the number of distinct characters.

---

## 11. Important Problem: Valid Anagram

### Problem

Determine whether two strings contain the same characters with the same frequencies.

Input:

```text
str1 = "listen"
str2 = "silent"
```

Output:

```text
true
```

### Java solution using HashMap

```java
import java.util.*;

public class Main {

    static boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Integer> map = new HashMap<>();

        for (char ch : s.toCharArray()) {

            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        for (char ch : t.toCharArray()) {

            if (!map.containsKey(ch)) {
                return false;
            }

            map.put(ch, map.get(ch) - 1);

            if (map.get(ch) == 0) {
                map.remove(ch);
            }
        }

        return map.isEmpty();
    }

    public static void main(String[] args) {

        System.out.println(isAnagram("listen", "silent"));
        System.out.println(isAnagram("hello", "world"));
    }
}
```

**Time Complexity:** O(n) average.

**Space Complexity:** O(k), where k is the number of distinct characters.

---

## 12. HashSet Operations

HashSet can also be used to perform common set operations.

Consider:

```java
Set<Integer> set1 = new HashSet<>(
    Arrays.asList(1, 2, 3, 4)
);

Set<Integer> set2 = new HashSet<>(
    Arrays.asList(3, 4, 5, 6)
);
```

### 12.1 Union

All unique elements from both sets.

```java
Set<Integer> union = new HashSet<>(set1);

union.addAll(set2);

System.out.println(union);
```

Possible output:

```text
[1, 2, 3, 4, 5, 6]
```

### 12.2 Intersection

Elements common to both sets.

```java
Set<Integer> intersection = new HashSet<>(set1);

intersection.retainAll(set2);

System.out.println(intersection);
```

Output:

```text
[3, 4]
```

### 12.3 Difference

Elements present in the first set but not in the second.

```java
Set<Integer> difference = new HashSet<>(set1);

difference.removeAll(set2);

System.out.println(difference);
```

Output:

```text
[1, 2]
```

---

## 13. How Does HashMap Work Internally?

Understanding the internal working of HashMap is important for Java interviews.

A HashMap uses a hash table to store key-value pairs.

### Internal working

When we execute:

```java
map.put("Java", 100);
```

Java performs approximately these steps:

1. Calls the key's `hashCode()` method.
2. Processes the hash value to calculate a bucket index.
3. Finds the corresponding bucket.
4. If the bucket is empty, inserts the entry.
5. If the bucket already contains entries, handles the collision by comparing keys.
6. If an equal key exists, updates its value; otherwise, adds another entry.

When we execute:

```java
map.get("Java");
```

Java calculates the hash again, locates the bucket, and checks the keys to find the matching entry.

### What is a collision?

A collision occurs when different keys map to the same bucket.

For example:

```text
Key A → Bucket 2
Key B → Bucket 2
```

Both keys need to be stored in the same bucket.

Java handles collisions using linked structures and, under certain conditions, tree-based bins.

In Java 8 and later, a heavily populated bucket can be converted into a balanced tree when the relevant thresholds and table-capacity conditions are met.

### What is load factor?

Load factor determines how full the HashMap can become before resizing.

Default load factor:

```text
0.75
```

For example:

```java
HashMap<Integer, String> map = new HashMap<>(16, 0.75f);
```

With an initial capacity of 16 and the default load factor, the resize threshold is initially 12 entries.

When the threshold is exceeded, HashMap generally resizes its internal table.

---

## 14. Importance of equals() and hashCode()

HashMap uses both `hashCode()` and `equals()` to identify keys.

### Important contract

1. If two objects are equal according to `equals()`, they must return the same hash code.
2. Two objects with the same hash code are not necessarily equal.
3. If `equals()` is overridden, `hashCode()` should also be overridden consistently.

### Example

```java
class Student {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student)) {
            return false;
        }

        Student other = (Student) obj;

        return this.id == other.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

Now two Student objects with the same ID are considered equal.

```java
HashMap<Student, String> map = new HashMap<>();

Student s1 = new Student(101, "Rahul");
Student s2 = new Student(101, "Amit");

map.put(s1, "Developer");

System.out.println(map.get(s2));
```

Output:

```text
Developer
```

Even though `s1` and `s2` are different objects, they are equal according to the implemented `equals()` and `hashCode()` methods.

**Important:** Avoid changing fields used by `equals()` or `hashCode()` after inserting an object as a HashMap key. Doing so can make the entry difficult to retrieve.

---

## 15. HashMap vs LinkedHashMap vs TreeMap

Java provides different Map implementations.

| Feature | HashMap | LinkedHashMap | TreeMap |
|---|---|---|---|
| Ordering | No guaranteed order | Insertion order by default | Sorted by keys |
| Average lookup | O(1) | O(1) | O(log n) |
| Average insertion | O(1) | O(1) | O(log n) |
| Internal structure | Hash table | Hash table + linked ordering | Red-black tree |
| Null key | One allowed | One allowed | Generally not allowed with natural ordering |

### Example

```java
Map<Integer, String> hashMap = new HashMap<>();

Map<Integer, String> linkedHashMap = new LinkedHashMap<>();

Map<Integer, String> treeMap = new TreeMap<>();
```

Use:
- HashMap when ordering is not required.
- LinkedHashMap when insertion order should be preserved.
- TreeMap when keys need to remain sorted.

---

## 16. Time and Space Complexity

Average-case complexities:

| Operation | HashMap | HashSet |
|---|---:|---:|
| Insert | O(1) | O(1) |
| Search | O(1) | O(1) |
| Delete | O(1) | O(1) |
| Iterate | O(n) | O(n) |

These are average-case expectations, not unconditional guarantees.

Hash collisions can make operations slower. Java's tree-based bins can improve lookup behavior in heavily collided buckets under the required conditions.

Space complexity:

- HashMap: O(n) for n entries.
- HashSet: O(n) for n elements.

---

## 17. Important Java Hashing Points

1. HashMap does not guarantee iteration order.
2. HashMap allows one null key.
3. HashMap allows multiple null values.
4. HashSet stores only unique elements.
5. HashSet allows one null element.
6. HashSet is internally backed by a HashMap.
7. HashMap is not synchronized by default.
8. HashMap keys must follow the `equals()` and `hashCode()` contract.
9. Changing a key's hash-relevant fields after insertion can cause lookup problems.
10. `getOrDefault()` is useful for frequency counting.
11. `entrySet()` is useful when both keys and values are required.
12. HashMap and HashSet generally provide O(1) average-case operations.

---

## 18. Common Mistakes

### Mistake 1: Using get() without checking whether the key exists

```java
int count = map.get(key);
```

If the key does not exist, `get()` returns `null`. Unboxing that null into an `int` can cause a NullPointerException.

Better:

```java
int count = map.getOrDefault(key, 0);
```

### Mistake 2: Assuming HashMap maintains insertion order

HashMap does not guarantee insertion order.

Use LinkedHashMap if insertion order is required.

### Mistake 3: Forgetting to override hashCode()

If you override `equals()` for a custom key, implement a consistent `hashCode()` too.

### Mistake 4: Using HashMap when only uniqueness is needed

If you only need to check whether an element exists, HashSet is usually simpler.

### Mistake 5: Inserting before checking in Two Sum

Check whether the complement exists before inserting the current number. Otherwise, an element might incorrectly match itself.

---

## 19. Common Interview Questions

**Q1. What is hashing?**

Hashing is a technique that uses a hash function to map keys to positions in a data structure for efficient storage and retrieval.

**Q2. What is HashMap?**

HashMap is a Java collection that stores data in key-value pairs and provides average O(1) insertion, retrieval, and deletion.

**Q3. What is HashSet?**

HashSet is a collection that stores unique elements without guaranteeing their iteration order.

**Q4. Can HashMap contain duplicate keys?**

No. Inserting an existing key updates its associated value.

**Q5. Can HashMap contain duplicate values?**

Yes. Multiple keys can have the same value.

**Q6. What is a collision?**

A collision occurs when different keys map to the same bucket.

**Q7. What is the default load factor of HashMap?**

0.75.

**Q8. Why are equals() and hashCode() important?**

They help HashMap identify whether two keys are logically equal and should refer to the same entry.

**Q9. Is HashMap thread-safe?**

No. HashMap is not synchronized by default. For concurrent access, consider ConcurrentHashMap.

**Q10. What is the difference between HashMap and TreeMap?**

HashMap does not guarantee key ordering and generally offers average O(1) operations. TreeMap maintains sorted keys and offers O(log n) operations.

**Q11. How does HashSet avoid duplicates?**

It uses hashing and equality checks to determine whether an element already exists.

**Q12. What happens if we insert the same key twice in HashMap?**

The previous value associated with that key is replaced.

**Q13. Can HashMap use a custom object as a key?**

Yes. The object's `equals()` and `hashCode()` methods should be implemented consistently.

**Q14. What happens when two keys have the same hash code?**

HashMap handles the collision by storing multiple entries in the same bucket and comparing keys.

---

## 20. Practice Problems

Try solving these problems using HashMap and HashSet.

### Easy

1. Contains Duplicate
2. Two Sum
3. Valid Anagram
4. First Unique Character in a String
5. Intersection of Two Arrays
6. Count Elements With Maximum Frequency

### Medium

1. Group Anagrams
2. Top K Frequent Elements
3. Longest Consecutive Sequence
4. Subarray Sum Equals K
5. Sort Characters by Frequency
6. Isomorphic Strings
7. Longest Substring Without Repeating Characters

### Hard

1. Minimum Window Substring
2. Substring with Concatenation of All Words
3. Count of Smaller Numbers After Self

**Practice tip:** For every problem, identify whether you need a key-value relationship (HashMap) or only uniqueness and membership checks (HashSet).

---

## 21. Quick Revision Cheat Sheet

| Requirement | Recommended approach |
|---|---|
| Store key-value pairs | HashMap |
| Store unique elements | HashSet |
| Count frequencies | HashMap |
| Detect duplicates | HashSet |
| Find a complement | HashMap |
| Preserve insertion order | LinkedHashMap |
| Maintain sorted keys | TreeMap |
| Check key existence | `containsKey()` |
| Check element existence | `contains()` |
| Get default value | `getOrDefault()` |
| Iterate key-value pairs | `entrySet()` |

### Useful Java syntax

```java
// HashMap
Map<Integer, Integer> map = new HashMap<>();

map.put(1, 10);
map.get(1);
map.getOrDefault(2, 0);
map.containsKey(1);
map.remove(1);

// HashSet
Set<Integer> set = new HashSet<>();

set.add(10);
set.contains(10);
set.remove(10);
set.size();
```

---

## 22. Chapter Completion Checklist

- [ ] Understand hashing and hash functions.
- [ ] Understand HashMap and key-value pairs.
- [ ] Understand HashSet and unique elements.
- [ ] Practice HashMap insertion, retrieval, and deletion.
- [ ] Practice frequency counting.
- [ ] Solve Two Sum using HashMap.
- [ ] Solve duplicate detection using HashSet.
- [ ] Understand collisions and load factor.
- [ ] Understand equals() and hashCode().
- [ ] Understand HashMap vs LinkedHashMap vs TreeMap.
- [ ] Solve at least five practice problems.

---

## Next Topic

**08-trees/binary-tree.md**

In the next chapter, we will learn:
- What is a Tree data structure?
- What is a Binary Tree?
- Root, Parent, Child, Leaf, and Height.
- Binary Tree implementation in Java.
- TreeNode structure.
- Basic tree operations.
- Tree traversal introduction.
- Interview questions and practice problems.