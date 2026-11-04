# Strings

Strings are one of the most frequently used data types in coding interviews.

String problems commonly involve:

- Character manipulation
- Searching
- Comparing strings
- Reversing
- Palindromes
- Frequency counting
- Anagrams
- Substrings
- String patterns
- Two Pointers
- Sliding Window
- HashMap / HashSet
- StringBuilder

In Java, strings have some important characteristics that are different from arrays.

---

# 1. What is a String?

A String is a sequence of characters.

Example:

```text
"hello"
```

Characters:

```text
h e l l o
```

Indexes:

```text
0 1 2 3 4
```

So:

```java
String s = "hello";

System.out.println(s.charAt(0));
```

Output:

```text
h
```

---

# 2. Creating a String in Java

There are two common ways.

## String Literal

```java
String s = "hello";
```

## Using `new`

```java
String s = new String("hello");
```

Usually, prefer:

```java
String s = "hello";
```

because Java can reuse string literals through the String Pool.

---

# 3. String Immutability

One of the most important Java String concepts is:

> **Strings are immutable.**

Immutable means:

> Once a String object is created, its contents cannot be changed.

Example:

```java
String s = "hello";

s.concat(" world");

System.out.println(s);
```

Output:

```text
hello
```

The original String did not change.

---

# 4. Why Didn't the String Change?

This:

```java
s.concat(" world");
```

creates a new String.

It does not modify the existing String.

To store the result:

```java
s = s.concat(" world");
```

Now:

```java
System.out.println(s);
```

Output:

```text
hello world
```

---

# 5. Example of Immutability

```java
String s = "Java";

s = s + " Programming";
```

Conceptually, Java creates a new String:

```text
"Java Programming"
```

The original:

```text
"Java"
```

was not modified.

The variable `s` now refers to the new String.

---

# 6. Why Are Strings Immutable?

String immutability provides several benefits:

### Security

Strings are commonly used for:

```text
file paths
URLs
database connections
class names
credentials
```

Immutable values are safer to share.

### String Pool

Java can safely reuse immutable String objects.

### Thread Safety

Immutable objects can be shared between threads without modification concerns.

### Hashing

Strings are commonly used as keys in:

```java
HashMap<String, Integer>
```

Their contents cannot unexpectedly change after being used as keys.

---

# 7. String Length

Use:

```java
String s = "hello";

System.out.println(s.length());
```

Output:

```text
5
```

Important:

For arrays:

```java
arr.length
```

For Strings:

```java
s.length()
```

---

# 8. Accessing Characters

Use:

```java
char c = s.charAt(index);
```

Example:

```java
String s = "hello";

System.out.println(s.charAt(0));
System.out.println(s.charAt(3));
```

Output:

```text
h
l
```

---

# 9. Iterating Through a String

## Using a for Loop

```java
String s = "hello";

for (int i = 0; i < s.length(); i++) {
    System.out.println(s.charAt(i));
}
```

Output:

```text
h
e
l
l
o
```

---

## Using Enhanced for Loop

Java does not directly allow:

```java
for (char c : s)
```

because String is not an array.

You can use:

```java
for (char c : s.toCharArray()) {
    System.out.println(c);
}
```

---

# 10. Convert String to Character Array

```java
String s = "hello";

char[] chars = s.toCharArray();
```

Result:

```text
['h', 'e', 'l', 'l', 'o']
```

Now we can modify the characters:

```java
chars[0] = 'H';
```

Convert back:

```java
String result = new String(chars);
```

Result:

```text
Hello
```

---

# 11. Comparing Strings

This is a very important Java interview topic.

Do not use:

```java
==
```

to compare String contents.

Use:

```java
.equals()
```

---

## Incorrect

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Why?

Because `==` compares object references.

---

## Correct

```java
System.out.println(a.equals(b));
```

Output:

```text
true
```

Because `.equals()` compares the String contents.

---

# 12. `==` vs `.equals()`

| Operator | Checks |
|---|---|
| `==` | Object references |
| `.equals()` | Content |

Example:

```java
String a = "hello";
String b = "hello";

System.out.println(a == b);
System.out.println(a.equals(b));
```

Because of String Pooling, the first may be:

```text
true
```

But don't rely on `==` for String content comparison.

Always use:

```java
a.equals(b)
```

---

# 13. Case-Insensitive Comparison

Use:

```java
equalsIgnoreCase()
```

Example:

```java
String a = "Java";
String b = "java";

System.out.println(a.equalsIgnoreCase(b));
```

Output:

```text
true
```

---

# 14. String Comparison

You can use:

```java
compareTo()
```

Example:

```java
String a = "apple";
String b = "banana";

System.out.println(a.compareTo(b));
```

A negative value means:

```text
a comes before b
```

A positive value means:

```text
a comes after b
```

Zero means:

```text
both are equal
```

---

# 15. Common String Methods

These methods are frequently used in DSA problems.

| Method | Purpose |
|---|---|
| `length()` | String length |
| `charAt(i)` | Character at index |
| `substring()` | Extract substring |
| `indexOf()` | Find position |
| `contains()` | Check existence |
| `equals()` | Compare strings |
| `equalsIgnoreCase()` | Case-insensitive comparison |
| `toLowerCase()` | Convert to lowercase |
| `toUpperCase()` | Convert to uppercase |
| `trim()` | Remove surrounding whitespace |
| `replace()` | Replace characters/text |
| `split()` | Split into parts |
| `toCharArray()` | Convert to char array |

---

# 16. `substring()`

Example:

```java
String s = "abcdef";

String result = s.substring(1, 4);

System.out.println(result);
```

Output:

```text
bcd
```

Important:

The starting index is inclusive.

The ending index is exclusive.

```text
substring(start, end)
```

means:

```text
start <= index < end
```

---

# 17. Substring Example

```text
String:
abcdef

Indexes:
012345
```

```java
s.substring(2, 5)
```

returns:

```text
cde
```

Because indexes:

```text
2 → c
3 → d
4 → e
```

Index `5` is excluded.

---

# 18. `indexOf()`

Find the position of a character or substring.

```java
String s = "hello";

System.out.println(s.indexOf('l'));
```

Output:

```text
2
```

For a substring:

```java
System.out.println(s.indexOf("ll"));
```

Output:

```text
2
```

If it doesn't exist:

```java
System.out.println(s.indexOf('x'));
```

Output:

```text
-1
```

---

# 19. `contains()`

```java
String s = "hello world";

System.out.println(s.contains("world"));
```

Output:

```text
true
```

---

# 20. Converting Case

```java
String s = "Hello";

System.out.println(s.toLowerCase());
System.out.println(s.toUpperCase());
```

Output:

```text
hello
HELLO
```

---

# 21. `replace()`

```java
String s = "hello";

String result = s.replace('l', 'x');

System.out.println(result);
```

Output:

```text
hexxo
```

Remember that String is immutable, so:

```java
s.replace('l', 'x');
```

does not modify `s`.

You need:

```java
s = s.replace('l', 'x');
```

if you want to store the result.

---

# 22. StringBuilder

Because Strings are immutable, repeatedly concatenating Strings can be inefficient.

For example:

```java
String result = "";

for (int i = 0; i < 10000; i++) {
    result += i;
}
```

This can create many intermediate String objects.

Use `StringBuilder` when repeatedly modifying a string.

```java
StringBuilder sb = new StringBuilder();

sb.append("Hello");
sb.append(" ");
sb.append("World");

System.out.println(sb);
```

Output:

```text
Hello World
```

---

# 23. Important StringBuilder Methods

```java
StringBuilder sb = new StringBuilder();
```

### Append

```java
sb.append("Java");
```

### Insert

```java
sb.insert(0, "Hello ");
```

### Delete

```java
sb.delete(0, 6);
```

### Reverse

```java
sb.reverse();
```

### Set Character

```java
sb.setCharAt(0, 'J');
```

### Length

```java
sb.length();
```

### Convert to String

```java
String result = sb.toString();
```

---

# 24. Reverse a String

A common interview problem.

Example:

```text
Input:
hello

Output:
olleh
```

Using `StringBuilder`:

```java
static String reverse(String s) {

    return new StringBuilder(s)
            .reverse()
            .toString();
}
```

---

# 25. Reverse Without StringBuilder

Using a character array:

```java
static String reverse(String s) {

    char[] chars = s.toCharArray();

    int left = 0;
    int right = chars.length - 1;

    while (left < right) {

        char temp = chars[left];
        chars[left] = chars[right];
        chars[right] = temp;

        left++;
        right--;
    }

    return new String(chars);
}
```

This is also a good example of the **Two Pointers** technique.

---

# 26. Dry Run: Reverse String

Input:

```text
"hello"
```

Characters:

```text
h e l l o
```

Pointers:

```text
↑       ↑
L       R
```

Swap:

```text
o e l l h
```

Move:

```text
  ↑   ↑
  L   R
```

Swap:

```text
o l l e h
```

Continue until:

```text
o l l e h
```

Answer:

```text
"olleh"
```

---

# 27. Check Palindrome

A palindrome reads the same from both directions.

Examples:

```text
madam
racecar
level
```

Non-palindrome:

```text
hello
```

Use Two Pointers.

```java
static boolean isPalindrome(String s) {

    int left = 0;
    int right = s.length() - 1;

    while (left < right) {

        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

---

# 28. Palindrome Dry Run

Input:

```text
"level"
```

Compare:

```text
l == l
```

Then:

```text
e == e
```

Middle:

```text
v
```

All characters match.

Answer:

```text
true
```

---

# 29. Character Frequency

One of the most common String techniques is counting character frequency.

Example:

```text
"banana"
```

Frequency:

```text
b → 1
a → 3
n → 2
```

---

# 30. Frequency Using HashMap

```java
import java.util.HashMap;
import java.util.Map;

static Map<Character, Integer> frequency(String s) {

    Map<Character, Integer> map = new HashMap<>();

    for (char c : s.toCharArray()) {

        map.put(
            c,
            map.getOrDefault(c, 0) + 1
        );
    }

    return map;
}
```

---

# 31. Frequency Using an Array

If the input contains only lowercase English letters:

```text
a-z
```

we can use:

```java
int[] frequency = new int[26];
```

Example:

```java
static int[] frequency(String s) {

    int[] freq = new int[26];

    for (char c : s.toCharArray()) {
        freq[c - 'a']++;
    }

    return freq;
}
```

---

# 32. Why `c - 'a'`?

Characters have numeric values.

For lowercase letters:

```text
a → 0
b → 1
c → 2
...
z → 25
```

So:

```java
c - 'a'
```

converts the character into an array index.

Example:

```text
c - a = 2
```

Therefore:

```java
freq[2]++;
```

represents character `c`.

---

# 33. Check if Two Strings Are Anagrams

Two strings are anagrams if they contain the same characters with the same frequencies.

Example:

```text
listen
silent
```

Both contain:

```text
l → 1
i → 1
s → 1
t → 1
e → 1
n → 1
```

Therefore:

```text
true
```

---

# 34. Anagram Using Frequency Array

```java
static boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    int[] freq = new int[26];

    for (char c : s.toCharArray()) {
        freq[c - 'a']++;
    }

    for (char c : t.toCharArray()) {
        freq[c - 'a']--;
    }

    for (int count : freq) {
        if (count != 0) {
            return false;
        }
    }

    return true;
}
```

---

# 35. Dry Run: Anagram

Input:

```text
s = "listen"
t = "silent"
```

Process `listen`:

```text
l +1
i +1
s +1
t +1
e +1
n +1
```

Process `silent`:

```text
s -1
i -1
l -1
e -1
n -1
t -1
```

Everything becomes:

```text
0
```

Therefore:

```text
true
```

---

# 36. Remove Spaces

Example:

```text
"hello world"
```

Remove spaces:

```text
"helloworld"
```

Using:

```java
String result = s.replace(" ", "");
```

---

# 37. Remove All Whitespace

For all whitespace characters:

```java
String result = s.replaceAll("\\s+", "");
```

Note:

`replaceAll()` uses regular expressions.

---

# 38. Count Vowels

```java
static int countVowels(String s) {

    int count = 0;

    for (char c : s.toLowerCase().toCharArray()) {

        if (c == 'a' ||
            c == 'e' ||
            c == 'i' ||
            c == 'o' ||
            c == 'u') {

            count++;
        }
    }

    return count;
}
```

Example:

```text
Input:
"hello"

Output:
2
```

---

# 39. Count Words

Simple approach:

```java
static int countWords(String s) {

    s = s.trim();

    if (s.isEmpty()) {
        return 0;
    }

    return s.split("\\s+").length;
}
```

Example:

```text
"Java is powerful"
```

Result:

```text
3
```

---

# 40. StringBuilder vs String

| Feature | String | StringBuilder |
|---|---|---|
| Mutable | No | Yes |
| Modification | Creates new object | Modifies existing object |
| Good for repeated changes | No | Yes |
| Common use | Read-only text | Building strings |
| Thread-safe | Immutable | No |

---

# 41. StringBuffer

`StringBuffer` is similar to `StringBuilder` but its methods are synchronized.

```java
StringBuffer sb = new StringBuffer();
```

Generally:

```text
StringBuilder
```

is preferred in normal single-threaded DSA problems.

Use:

```text
StringBuffer
```

when synchronized/thread-safe mutable string behavior is specifically needed.

---

# 42. String vs Character Array

Example:

```java
String s = "hello";
```

String:

```text
immutable
```

Character array:

```java
char[] chars = {'h', 'e', 'l', 'l', 'o'};
```

Character array:

```text
mutable
```

You can do:

```java
chars[0] = 'H';
```

but you cannot directly do:

```java
s[0] = 'H';
```

in Java.

---

# 43. String to Integer

Use:

```java
int number = Integer.parseInt("123");
```

Result:

```text
123
```

---

# 44. Integer to String

Use:

```java
String s = String.valueOf(123);
```

or:

```java
String s = Integer.toString(123);
```

---

# 45. Character to Integer

If:

```java
char c = '7';
```

Then:

```java
int digit = c - '0';
```

Result:

```text
7
```

This is very common in DSA.

---

# 46. Integer Digit to Character

If:

```java
int digit = 7;
```

Use:

```java
char c = (char) ('0' + digit);
```

Result:

```text
'7'
```

---

# 47. Important Character Checks

Java provides:

```java
Character.isLetter(c)
Character.isDigit(c)
Character.isWhitespace(c)
Character.isUpperCase(c)
Character.isLowerCase(c)
```

Example:

```java
char c = '7';

if (Character.isDigit(c)) {
    System.out.println("Digit");
}
```

---

# 48. Reverse Words in a String

Example:

```text
Input:
"hello world"

Output:
"world hello"
```

One approach:

```java
static String reverseWords(String s) {

    String[] words = s.trim().split("\\s+");

    StringBuilder result = new StringBuilder();

    for (int i = words.length - 1; i >= 0; i--) {

        result.append(words[i]);

        if (i != 0) {
            result.append(" ");
        }
    }

    return result.toString();
}
```

---

# 49. Example

Input:

```text
"the sky is blue"
```

Split:

```text
["the", "sky", "is", "blue"]
```

Read backwards:

```text
blue
is
sky
the
```

Output:

```text
"blue is sky the"
```

---

# 50. String Patterns in DSA

Many String problems use techniques you've already learned.

### Two Pointers

Palindrome:

```text
left →       ← right
```

### Sliding Window

Longest substring:

```text
[left ........ right]
```

### HashMap

Character frequency:

```text
character → count
```

### HashSet

Unique characters:

```text
Set<Character>
```

### Prefix Sum / Frequency

Useful for character-count or substring-related problems.

---

# 51. Common String Problem Categories

When solving String problems, identify the category.

## Category 1: Reverse

Examples:

```text
Reverse String
Reverse Words
Reverse Vowels
```

Think:

```text
Two Pointers
StringBuilder
```

---

## Category 2: Palindrome

Examples:

```text
Valid Palindrome
Longest Palindromic Substring
Palindrome Permutation
```

Think:

```text
Two Pointers
```

or more advanced techniques depending on the problem.

---

## Category 3: Frequency

Examples:

```text
Valid Anagram
First Unique Character
Character Frequency
```

Think:

```text
HashMap
int[26]
```

---

## Category 4: Substring

Examples:

```text
Longest Substring Without Repeating Characters
Minimum Window Substring
Longest Substring With K Distinct Characters
```

Think:

```text
Sliding Window
HashMap
HashSet
```

---

## Category 5: String Construction

Examples:

```text
Add Strings
Build String
Remove Characters
Replace Characters
```

Think:

```text
StringBuilder
```

---

# 52. Time Complexity of Common Operations

For a String of length `n`:

| Operation | Typical Complexity |
|---|---:|
| `length()` | O(1) |
| `charAt(i)` | O(1) |
| `substring()` | Depends on Java version/usage; treat resulting processing as proportional to copied length when applicable |
| `indexOf()` | O(n) |
| `equals()` | O(n) worst case |
| `toCharArray()` | O(n) |
| `toLowerCase()` | O(n) |
| `replace()` | O(n) or more depending on replacement |
| `StringBuilder.append()` | Amortized O(1) |
| `StringBuilder.reverse()` | O(n) |

For interviews, always consider the length of the string when analyzing operations that scan or copy characters.

---

# 53. Common Mistakes

## Mistake 1: Using `==`

Incorrect:

```java
if (s1 == s2)
```

Correct:

```java
if (s1.equals(s2))
```

---

## Mistake 2: Forgetting Immutability

Incorrect assumption:

```java
s.toUpperCase();
```

changes `s`.

Correct:

```java
s = s.toUpperCase();
```

---

## Mistake 3: Repeated String Concatenation

Avoid:

```java
String result = "";

for (...) {
    result += value;
}
```

Prefer:

```java
StringBuilder result = new StringBuilder();

for (...) {
    result.append(value);
}
```

---

## Mistake 4: Off-by-One Errors

Remember:

```java
s.substring(start, end)
```

where:

```text
start = included
end = excluded
```

---

## Mistake 5: Character vs String

These are different:

```java
'a'
```

and:

```java
"a"
```

The first is:

```text
char
```

The second is:

```text
String
```

---

# 54. Interview Questions

### Q1. Why is String immutable in Java?

String immutability improves security, enables String Pooling, provides safe sharing, and makes Strings reliable as HashMap keys.

---

### Q2. Difference between `==` and `.equals()`?

```text
==        → reference comparison
equals()  → content comparison
```

---

### Q3. String vs StringBuilder?

```text
String:
Immutable

StringBuilder:
Mutable
```

Use `StringBuilder` when repeatedly modifying/building text.

---

### Q4. StringBuilder vs StringBuffer?

```text
StringBuilder → generally faster, not synchronized
StringBuffer  → synchronized
```

---

### Q5. How do you reverse a String?

Using:

```java
new StringBuilder(s).reverse().toString();
```

or using Two Pointers with a character array.

---

### Q6. How do you check a palindrome?

Use two pointers:

```text
left → →
← ← right
```

Compare both characters until the pointers meet.

---

### Q7. How do you count character frequency?

For lowercase English letters:

```java
int[] freq = new int[26];
```

For arbitrary characters:

```java
Map<Character, Integer>
```

---

### Q8. How do you check whether two strings are anagrams?

Compare character frequencies.

---

### Q9. When should you use Sliding Window for strings?

When the problem asks about a contiguous substring and requires finding a longest, shortest, or otherwise constrained window.

---

# 55. Practice Problems

## Easy

1. Reverse String
2. Valid Palindrome
3. Valid Anagram
4. First Unique Character in a String
5. Reverse Words in a String
6. Length of Last Word
7. Count Vowels
8. Remove All Adjacent Duplicates

## Medium

9. Longest Substring Without Repeating Characters
10. Longest Repeating Character Replacement
11. Group Anagrams
12. String Compression
13. Permutation in String
14. Find All Anagrams in a String
15. Longest Palindromic Substring

## Hard

16. Minimum Window Substring
17. Word Break
18. Edit Distance
19. Regular Expression Matching
20. Wildcard Matching

---

# 56. String Cheat Sheet

## Length

```java
s.length();
```

## Character

```java
s.charAt(i);
```

## Character Array

```java
s.toCharArray();
```

## Substring

```java
s.substring(start, end);
```

## Compare

```java
s1.equals(s2);
```

## Ignore Case

```java
s1.equalsIgnoreCase(s2);
```

## Search

```java
s.indexOf("abc");
```

## Contains

```java
s.contains("abc");
```

## Lowercase

```java
s.toLowerCase();
```

## Uppercase

```java
s.toUpperCase();
```

## Replace

```java
s.replace("old", "new");
```

## StringBuilder

```java
StringBuilder sb = new StringBuilder();

sb.append("Hello");
sb.append(" World");

String result = sb.toString();
```

## Reverse

```java
String result = new StringBuilder(s)
        .reverse()
        .toString();
```

---

# 57. Recognition Guide

When you see:

```text
reverse string
```

Think:

```text
Two Pointers
StringBuilder
```

When you see:

```text
palindrome
```

Think:

```text
Two Pointers
```

When you see:

```text
same characters
anagram
frequency
duplicate characters
```

Think:

```text
HashMap
Frequency Array
HashSet
```

When you see:

```text
longest substring
shortest substring
substring with condition
```

Think:

```text
Sliding Window
```

When you need to repeatedly build a String:

```text
StringBuilder
```

---

# 58. Final Takeaway

For DSA, you don't need to memorize every String method.

Focus on these core concepts:

```text
String is immutable
        ↓
StringBuilder for modifications
        ↓
charAt() for character access
        ↓
toCharArray() for mutable character operations
        ↓
Two Pointers for palindrome/reversal
        ↓
HashMap/Frequency Array for character counts
        ↓
Sliding Window for substring problems
```

The most important String skills are:

```text
1. Traversing characters
2. Comparing strings
3. Building strings efficiently
4. Counting characters
5. Using Two Pointers
6. Using HashMap / HashSet
7. Using Sliding Window
```

Once these are comfortable, most basic and medium String problems become much easier to recognize.

---

# Checklist

Before moving to the next topic, make sure you can:

- [ ] Explain String immutability
- [ ] Use `length()`
- [ ] Use `charAt()`
- [ ] Convert String to `char[]`
- [ ] Compare Strings using `.equals()`
- [ ] Explain `==` vs `.equals()`
- [ ] Use `substring()`
- [ ] Use `indexOf()`
- [ ] Use `contains()`
- [ ] Use `StringBuilder`
- [ ] Reverse a String
- [ ] Check a palindrome
- [ ] Count character frequency
- [ ] Check anagrams
- [ ] Use `int[26]`
- [ ] Use `HashMap<Character, Integer>`
- [ ] Recognize Sliding Window String problems

---

# Next Topic

```text
03-strings/frequency-count.md
```

Next we'll go deeper into **Frequency Counting**, including frequency arrays, HashMap, HashSet, duplicate detection, first unique character, anagrams, and how frequency counting combines with other DSA techniques.