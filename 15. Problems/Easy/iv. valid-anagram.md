# Valid Anagram

**Difficulty:** Easy  
**Topics:** Strings, Array, HashMap, Frequency Counting  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An anagram is a word or phrase formed by rearranging the characters of another word or phrase, using all the original characters exactly once.

In simple terms:

- Both strings must contain the same characters.
- Each character must appear the same number of times.
- The order of characters does not matter.
- Both strings must have the same length.

### Example 1

**Input:**
```java
s = "anagram"
t = "nagaram"
```

**Output:**
```java
true
```

**Explanation:**

Both strings contain the same characters with the same frequencies.

### Example 2

**Input:**
```java
s = "rat"
t = "car"
```

**Output:**
```java
false
```

**Explanation:**

The strings contain different characters.

### Example 3

**Input:**
```java
s = "aacc"
t = "ccac"
```

**Output:**
```java
false
```

**Explanation:**

Both strings contain the same distinct characters, but their frequencies are different.

---

## 2. Approach 1: Sorting

### Concept

If two strings are anagrams, sorting their characters will produce identical strings.

For example:

```text
s = "anagram"
t = "nagaram"

After sorting:

s = "aaagmnr"
t = "aaagmnr"
```

If both sorted strings are equal, they are anagrams.

### Java Code

```java
import java.util.Arrays;

public class ValidAnagramSorting {

    public static boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        char[] arr1 = s.toCharArray();
        char[] arr2 = t.toCharArray();

        Arrays.sort(arr1);
        Arrays.sort(arr2);

        return Arrays.equals(arr1, arr2);
    }

    public static void main(String[] args) {

        String s = "anagram";
        String t = "nagaram";

        System.out.println(isAnagram(s, t));
    }
}
```

### Dry Run

Input:

```java
s = "anagram"
t = "nagaram"
```

Character arrays before sorting:

```text
s = [a, n, a, g, r, a, m]
t = [n, a, g, a, r, a, m]
```

After sorting:

```text
s = [a, a, a, g, m, n, r]
t = [a, a, a, g, m, n, r]
```

Both arrays are equal.

Output:

```java
true
```

### Complexity Analysis

- **Time Complexity:** O(n log n), because both character arrays are sorted.
- **Space Complexity:** O(n), for the character arrays created from the strings. Sorting may also use implementation-dependent auxiliary space.

---

## 3. Approach 2: Frequency Counting Using an Array

### Concept

For lowercase English letters (`a` to `z`), we can use an integer array of size 26.

Each array index represents one character.

Mapping:

| Character | Index |
|---|---:|
| a | 0 |
| b | 1 |
| c | 2 |
| d | 3 |
| ... | ... |
| z | 25 |

Formula:

```java
index = character - 'a';
```

For example:

```java
'a' - 'a' = 0
'b' - 'a' = 1
'c' - 'a' = 2
```

### Algorithm

1. Check whether both strings have the same length.
2. Create an integer array of size 26.
3. Increase the frequency for every character in the first string.
4. Decrease the frequency for every character in the second string.
5. If all frequencies are zero, both strings are anagrams.

### Java Code

```java
public class ValidAnagramFrequency {

    public static boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        int[] frequency = new int[26];

        for (int i = 0; i < s.length(); i++) {

            frequency[s.charAt(i) - 'a']++;

            frequency[t.charAt(i) - 'a']--;
        }

        for (int count : frequency) {

            if (count != 0) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {

        String s = "anagram";
        String t = "nagaram";

        System.out.println(isAnagram(s, t));
    }
}
```

**Important:** This implementation assumes that both strings contain only lowercase English letters (`a` to `z`). It is not suitable for arbitrary Unicode characters, uppercase letters, or punctuation without additional handling.

---

## 4. Detailed Dry Run

Input:

```java
s = "anagram"
t = "nagaram"
```

Initially:

```java
int[] frequency = new int[26];
```

All values are zero.

For each position, increment the frequency of the character in `s` and decrement the frequency of the character in `t`.

| i | s.charAt(i) | t.charAt(i) | Frequency Change |
|---|---|---|---|
| 0 | a | n | a +1, n -1 |
| 1 | n | a | n +1, a -1 |
| 2 | a | g | a +1, g -1 |
| 3 | g | a | g +1, a -1 |
| 4 | r | r | r +1, r -1 |
| 5 | a | a | a +1, a -1 |
| 6 | m | m | m +1, m -1 |

After processing all characters:

```text
a = 0
g = 0
m = 0
n = 0
r = 0
```

All other character frequencies are also zero.

Therefore:

```java
return true;
```

### Why Do We Increment and Decrement?

The first string increases the frequency of each character.

The second string decreases the frequency of each character.

If both strings contain exactly the same characters with the same frequencies, all counts become zero.

If any character is missing or appears too many times, at least one count will be non-zero.

---

## 5. Approach 3: Using HashMap

### Concept

If the input can contain a wider range of characters, a HashMap can store character frequencies.

A HashMap stores:

```text
Character -> Frequency
```

For example:

```text
a -> 3
n -> 1
g -> 1
r -> 1
m -> 1
```

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

public class ValidAnagramHashMap {

    public static boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Integer> frequency = new HashMap<>();

        for (char ch : s.toCharArray()) {

            frequency.put(
                ch,
                frequency.getOrDefault(ch, 0) + 1
            );
        }

        for (char ch : t.toCharArray()) {

            if (!frequency.containsKey(ch)) {
                return false;
            }

            int count = frequency.get(ch);

            if (count == 0) {
                return false;
            }

            frequency.put(ch, count - 1);
        }

        return true;
    }

    public static void main(String[] args) {

        String s = "anagram";
        String t = "nagaram";

        System.out.println(isAnagram(s, t));
    }
}
```

### Understanding getOrDefault()

```java
frequency.getOrDefault(ch, 0)
```

This method:

- Returns the existing value if the key is present.
- Returns the default value `0` if the key is not present.

Example:

```java
Map<Character, Integer> map = new HashMap<>();

map.put('a', 2);

System.out.println(map.getOrDefault('a', 0)); // 2
System.out.println(map.getOrDefault('b', 0)); // 0
```

---

## 6. Comparing All Three Approaches

| Approach | Time Complexity | Extra Space |
|---|---|---|
| Sorting | O(n log n) | O(n) for character arrays |
| Frequency Array | O(n) | O(1), fixed 26 entries |
| HashMap | O(n) average | O(k), distinct characters |

Where:
- `n` = length of the strings.
- `k` = number of distinct characters.

**Recommended interview solution:**

For lowercase English letters, use the frequency array because it is simple and efficient.

For a wider character set, use a HashMap or an appropriate Unicode-aware frequency representation.

---

## 7. Edge Cases

| Input s | Input t | Expected Output |
|---|---|---|
| `"anagram"` | `"nagaram"` | `true` |
| `"rat"` | `"car"` | `false` |
| `"a"` | `"a"` | `true` |
| `"a"` | `"b"` | `false` |
| `"ab"` | `"a"` | `false` |
| `"aacc"` | `"ccac"` | `false` |
| `""` | `""` | `true` |

---

## 8. Common Interview Mistakes

1. Checking only whether both strings contain the same distinct characters.
2. Ignoring character frequency.
3. Forgetting to check string lengths.
4. Using `==` instead of `.equals()` when comparing Java strings.
5. Using a 26-element array without checking the character range.
6. Forgetting that uppercase and lowercase letters are different characters.

For example:

```text
"listen" and "silent" -> true

"hello" and "world" -> false

"aabb" and "abab" -> true

"aabb" and "abbb" -> false
```

---

## 9. Interview Questions

**Q1. What is an anagram?**

Two strings are anagrams if they contain exactly the same characters with the same frequencies, regardless of order.

**Q2. Why do we use an array of size 26?**

Because there are 26 lowercase English letters. Each character can be mapped to an index between 0 and 25.

**Q3. What does `s.charAt(i) - 'a'` do?**

It converts a lowercase English character into an array index.

For example:

```java
'a' - 'a' = 0
'c' - 'a' = 2
'z' - 'a' = 25
```

**Q4. Why do we increment and decrement frequencies?**

Incrementing counts the characters in the first string. Decrementing counts the characters in the second string. Matching strings produce zero frequency differences.

**Q5. Can we solve this problem using sorting?**

Yes. Sort both strings' character arrays and compare them.

**Q6. Which approach is best?**

For lowercase English letters, frequency counting with an integer array is generally the best choice because it takes O(n) time and O(1) extra space.

**Q7. Does this solution handle uppercase letters?**

The 26-element array implementation does not. A separate mapping or HashMap is needed for uppercase letters and other character ranges.

---

## 10. Practice Tasks

Try these variations:

- [ ] Check whether two strings are anagrams using a HashMap.
- [ ] Find the frequency of every character in a string.
- [ ] Find the first non-repeating character.
- [ ] Find the first repeating character.
- [ ] Group a list of words into anagram groups.
- [ ] Check whether two strings are anagrams without sorting.

---

## 11. Key Takeaways

- Anagrams contain the same characters with identical frequencies.
- Sorting is a straightforward solution.
- Frequency counting can optimize the solution to O(n).
- A 26-element array works for lowercase English letters.
- HashMap is useful for a wider character range.
- Always check string lengths before comparing frequencies.

**Most important pattern to remember:**

```java
int[] frequency = new int[26];

for (int i = 0; i < s.length(); i++) {

    frequency[s.charAt(i) - 'a']++;

    frequency[t.charAt(i) - 'a']--;
}

for (int count : frequency) {

    if (count != 0) {
        return false;
    }
}

return true;
```

This frequency-counting pattern is useful in many string and array interview problems.