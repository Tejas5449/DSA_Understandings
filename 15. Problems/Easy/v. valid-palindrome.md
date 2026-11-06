# Valid Palindrome

**Difficulty:** Easy  
**Topics:** String, Two Pointers, Character Checking  
**Commonly Asked In:** Coding interviews

---

## 1. Problem Statement

Given a string `s`, determine whether it is a palindrome.

A palindrome is a string that reads the same forward and backward.

For this problem:

- Convert uppercase letters into lowercase.
- Ignore all characters that are not letters or numbers.
- Return `true` if the resulting string is a palindrome.
- Otherwise, return `false`.

### Example 1

**Input:**
```java
s = "A man, a plan, a canal: Panama"
```

**Output:**
```java
true
```

**Explanation:**

After converting to lowercase and removing spaces and punctuation:

```text
amanaplanacanalpanama
```

The resulting string is a palindrome.

### Example 2

**Input:**
```java
s = "race a car"
```

**Output:**
```java
false
```

**Explanation:**

After removing spaces:

```text
raceacar
```

It is not a palindrome.

### Example 3

**Input:**
```java
s = " "
```

**Output:**
```java
true
```

**Explanation:**

After removing non-alphanumeric characters, the string is empty. An empty string is considered a palindrome.

---

## 2. Approach 1: Reverse the String

### Concept

The simplest approach is:

1. Convert the string into lowercase.
2. Remove all characters except letters and numbers.
3. Reverse the resulting string.
4. Compare the original cleaned string with the reversed string.

### Java Code

```java
public class ValidPalindromeReverse {

    public static boolean isPalindrome(String s) {

        String cleaned = s
                .toLowerCase()
                .replaceAll("[^a-z0-9]", "");

        String reversed = new StringBuilder(cleaned)
                .reverse()
                .toString();

        return cleaned.equals(reversed);
    }

    public static void main(String[] args) {

        String s = "A man, a plan, a canal: Panama";

        System.out.println(isPalindrome(s));
    }
}
```

### Understanding replaceAll()

```java
.replaceAll("[^a-z0-9]", "")
```

This regular expression removes everything except:

- `a-z`: lowercase English letters.
- `0-9`: digits.

The `^` inside the character class means NOT.

Example:

```text
Input:  A man, a plan! 123
Output: amanaplan123
```

### Complexity Analysis

Let `n` be the length of the original string.

- **Time Complexity:** O(n), because cleaning and reversing require linear processing.
- **Space Complexity:** O(n), because cleaned and reversed strings require additional storage.

---

## 3. Approach 2: Two Pointers

### Concept

Instead of creating another reversed string, we can compare characters directly from both ends.

Use two pointers:

- `left` starts at index `0`.
- `right` starts at the last index.

Follow these steps:

1. Skip non-alphanumeric characters from the left.
2. Skip non-alphanumeric characters from the right.
3. Convert both characters to lowercase.
4. Compare them.
5. If they are different, return `false`.
6. Otherwise, move both pointers toward the center.
7. If all comparisons match, return `true`.

### Visual Example

```text
String: A man, a plan, a canal: Panama

Left  -> A
Right -> a

Compare: a == a

Move pointers inward.
```

Continue comparing until the pointers meet or cross.

### Java Code

```java
public class ValidPalindromeTwoPointers {

    public static boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {

            // Skip non-alphanumeric characters from the left.
            while (left < right &&
                    !Character.isLetterOrDigit(s.charAt(left))) {

                left++;
            }

            // Skip non-alphanumeric characters from the right.
            while (left < right &&
                    !Character.isLetterOrDigit(s.charAt(right))) {

                right--;
            }

            // Compare characters without case sensitivity.
            char leftChar = Character.toLowerCase(s.charAt(left));

            char rightChar = Character.toLowerCase(s.charAt(right));

            if (leftChar != rightChar) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }

    public static void main(String[] args) {

        String s = "A man, a plan, a canal: Panama";

        System.out.println(isPalindrome(s));
    }
}
```

---

## 4. Detailed Dry Run

**Input:**

```java
s = "A man, a plan, a canal: Panama"
```

Initial values:

```java
left = 0;
right = s.length() - 1;
```

The algorithm skips punctuation and spaces and compares the characters from both ends.

| Step | Left Character | Right Character | Comparison | Result |
|---|---|---|---|---|
| 1 | A | a | a == a | Match |
| 2 | m | m | m == m | Match |
| 3 | a | a | a == a | Match |
| 4 | n | n | n == n | Match |
| 5 | a | a | a == a | Match |
| 6 | p | p | p == p | Match |
| 7 | l | l | l == l | Match |
| 8 | a | a | a == a | Match |
| 9 | n | n | n == n | Match |
| 10 | a | a | a == a | Match |

The pointers meet or cross without finding a mismatch.

Final output:

```java
true
```

### Example With a Mismatch

Input:

```java
s = "race a car"
```

After skipping spaces:

```text
r a c e a c a r
```

The first comparison is:

```text
r != r? 
```

Actually, the first and last characters are both `r`, so they match.

The next comparison is:

```text
a != a?
```

They also match.

The next comparison is:

```text
c != c?
```

They match.

The next comparison is:

```text
e != a
```

A mismatch is found, so the method returns `false`.

---

## 5. Understanding Character Methods in Java

Java provides useful methods through the `Character` class.

| Method | Description |
|---|---|
| `Character.isLetter(ch)` | Checks whether a character is a letter |
| `Character.isDigit(ch)` | Checks whether a character is a digit |
| `Character.isLetterOrDigit(ch)` | Checks whether a character is a letter or digit |
| `Character.toLowerCase(ch)` | Converts a character to lowercase |
| `Character.toUpperCase(ch)` | Converts a character to uppercase |

Example:

```java
public class CharacterExample {

    public static void main(String[] args) {

        char ch = 'A';

        System.out.println(Character.isLetter(ch)); 
        // true

        System.out.println(Character.isDigit(ch)); 
        // false

        System.out.println(Character.isLetterOrDigit(ch)); 
        // true

        System.out.println(Character.toLowerCase(ch)); 
        // a
    }
}
```

---

## 6. Understanding the Two-Pointer Pattern

Two pointers are commonly used when we need to process data from opposite ends or compare elements efficiently.

In this problem:

```java
int left = 0;
int right = s.length() - 1;
```

The pointers move toward each other:

```java
left++;
right--;
```

The loop condition is:

```java
while (left < right)
```

Why?

- When `left` is smaller than `right`, there are still characters to compare.
- When both pointers meet or cross, all required comparisons are complete.

### Important Two-Pointer Patterns

| Pattern | Example |
|---|---|
| Opposite-direction pointers | Valid Palindrome |
| Same-direction pointers | Remove Duplicates from Sorted Array |
| Fast and slow pointers | Linked List Cycle |
| Two pointers on sorted arrays | Two Sum II |

---

## 7. Compare Both Approaches

| Comparison | Reverse String | Two Pointers |
|---|---|---|
| Time Complexity | O(n) | O(n) |
| Extra Space | O(n) | O(1) auxiliary space |
| Creates reversed string | Yes | No |
| Easy to understand | Yes | Yes |
| More space efficient | No | Yes |

**Recommended interview solution:** Two Pointers.

It avoids creating an additional cleaned or reversed string and uses constant auxiliary space.

---

## 8. Edge Cases

| Input | Expected Output | Explanation |
|---|---|---|
| `"A man, a plan, a canal: Panama"` | `true` | Valid palindrome |
| `"race a car"` | `false` | Characters do not match |
| `" "` | `true` | Empty after filtering |
| `"a"` | `true` | Single character |
| `"aa"` | `true` | Same characters |
| `"ab"` | `false` | Different characters |
| `"12321"` | `true` | Numeric palindrome |
| `"0P"` | `false` | `0` and `p` differ |

---

## 9. Common Interview Mistakes

1. Forgetting to ignore spaces and punctuation.
2. Forgetting case-insensitive comparison.
3. Using `left <= right` unnecessarily instead of `left < right`.
4. Forgetting to check pointer boundaries while skipping characters.
5. Comparing characters without converting their case.
6. Creating multiple strings when a two-pointer solution is sufficient.

**Important:** Use `Character.isLetterOrDigit()` when the problem expects letters and digits beyond basic English ASCII characters. The regular expression approach shown earlier specifically keeps only lowercase English letters and digits.

---

## 10. Interview Questions

**Q1. What is a palindrome?**

A string that reads the same forward and backward.

**Q2. Why do we use two pointers?**

To compare characters from both ends without creating another reversed string.

**Q3. What is the time complexity of the two-pointer approach?**

O(n), because each pointer moves through the string at most once.

**Q4. What is the auxiliary space complexity?**

O(1), because we only use a fixed number of variables.

**Q5. Why do we use Character.isLetterOrDigit()?**

To skip spaces, punctuation, and other non-alphanumeric characters.

**Q6. Why do we convert characters to lowercase?**

Because uppercase and lowercase versions of the same letter should be treated as equal.

**Q7. What happens when the string is empty?**

The loop never executes, and the method returns `true`.

---

## 11. Practice Tasks

Try these variations:

- [ ] Check whether a string is a palindrome without using `StringBuilder.reverse()`.
- [ ] Check whether a number is a palindrome.
- [ ] Reverse a string using two pointers.
- [ ] Remove non-alphanumeric characters from a string.
- [ ] Find the longest palindromic substring.
- [ ] Check whether a string can become a palindrome after deleting at most one character.

---

## 12. Key Takeaways

- A palindrome reads the same forward and backward.
- Two pointers can compare characters from both ends.
- Use `Character.isLetterOrDigit()` to skip unwanted characters.
- Use `Character.toLowerCase()` for case-insensitive comparison.
- Two pointers provide O(n) time and O(1) auxiliary space.

**Most important pattern to remember:**

```java
int left = 0;
int right = s.length() - 1;

while (left < right) {

    while (left < right &&
            !Character.isLetterOrDigit(s.charAt(left))) {
        left++;
    }

    while (left < right &&
            !Character.isLetterOrDigit(s.charAt(right))) {
        right--;
    }

    if (Character.toLowerCase(s.charAt(left)) !=
            Character.toLowerCase(s.charAt(right))) {
        return false;
    }

    left++;
    right--;
}

return true;
```

This opposite-direction two-pointer pattern is useful in many array and string interview problems.