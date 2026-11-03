# String Patterns

String problems are one of the most common categories in DSA interviews.

Many string questions look different on the surface, but they are often solved using a small number of reusable patterns:

- Two Pointers
- Sliding Window
- Frequency Counting
- HashMap / HashSet
- StringBuilder
- Character comparison
- Sorting
- Prefix/Suffix techniques
- Pattern matching

The most important skill is not memorizing individual solutions.

It is recognizing **which pattern fits the problem**.

---

# 1. What Are String Patterns?

A string pattern is a reusable approach for solving a particular type of string problem.

For example:

> "Check whether a string is a palindrome."

This immediately suggests:

```text
Two Pointers
```

Another example:

> "Find the longest substring without repeating characters."

This suggests:

```text
Sliding Window + HashSet
```

Another:

> "Check whether two strings are anagrams."

This suggests:

```text
Frequency Counting
```

Understanding these patterns makes string problems much easier.

---

# 2. Common String Problem Patterns

| Problem Type | Common Pattern |
|---|---|
| Reverse a string | Two Pointers |
| Check palindrome | Two Pointers |
| Compare characters from both ends | Two Pointers |
| Count characters | Frequency Counting |
| Check anagram | Frequency Counting |
| First unique character | HashMap |
| Duplicate characters | HashMap / HashSet |
| Longest substring | Sliding Window |
| Smallest substring satisfying condition | Sliding Window |
| Remove duplicate characters | HashSet |
| Group anagrams | HashMap |
| Reverse words | String / StringBuilder |
| Check valid parentheses | Stack |
| Prefix matching | Prefix technique |
| Pattern searching | String matching |

---

# 3. Pattern 1 — Two Pointers

Two pointers are commonly used when we need to compare characters from opposite sides of a string.

Example:

```text
"madam"
```

We compare:

```text
m a d a m
↑       ↑

m == m
```

Then:

```text
m a d a m
  ↑   ↑

a == a
```

Then:

```text
m a d a m
    ↑

middle
```

The string is a palindrome.

---

## Example: Check Palindrome

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

### Dry Run

Input:

```text
madam
```

Initial:

```text
left = 0
right = 4
```

Compare:

```text
m == m
```

Move:

```text
left = 1
right = 3
```

Compare:

```text
a == a
```

Move:

```text
left = 2
right = 2
```

Stop.

Return:

```text
true
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# 4. Two Pointers With Ignoring Non-Alphanumeric Characters

A common interview variation is:

> Check whether a sentence is a palindrome while ignoring spaces, punctuation and case.

Example:

```text
"A man, a plan, a canal: Panama"
```

Output:

```text
true
```

We can use `Character.isLetterOrDigit()`.

```java
static boolean isPalindrome(String s) {

    int left = 0;
    int right = s.length() - 1;

    while (left < right) {

        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }

        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }

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
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# 5. Pattern 2 — Frequency Counting

Frequency counting is extremely important for string problems.

The idea is:

> Count how many times each character appears.

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

Using `HashMap`:

```java
static Map<Character, Integer> countCharacters(String s) {

    Map<Character, Integer> frequency = new HashMap<>();

    for (char c : s.toCharArray()) {
        frequency.put(
            c,
            frequency.getOrDefault(c, 0) + 1
        );
    }

    return frequency;
}
```

---

# 6. Frequency Counting With int[]

If the input contains only lowercase English letters, we can use:

```java
int[26]
```

Example:

```java
static int[] countCharacters(String s) {

    int[] frequency = new int[26];

    for (char c : s.toCharArray()) {
        frequency[c - 'a']++;
    }

    return frequency;
}
```

Why?

```text
'a' - 'a' = 0
'b' - 'a' = 1
'c' - 'a' = 2
...
'z' - 'a' = 25
```

This is usually faster and simpler than a `HashMap` when the character range is known.

---

# 7. Example — Check Anagram

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

Implementation:

```java
static boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    int[] frequency = new int[26];

    for (char c : s.toCharArray()) {
        frequency[c - 'a']++;
    }

    for (char c : t.toCharArray()) {
        frequency[c - 'a']--;
    }

    for (int count : frequency) {
        if (count != 0) {
            return false;
        }
    }

    return true;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

Because the frequency array always has 26 elements.

---

# 8. Pattern 3 — Sliding Window

Sliding Window is one of the most important string patterns.

It is useful when the problem talks about:

- substring
- longest substring
- shortest substring
- contiguous characters
- window
- at most K characters
- exactly K characters
- without repeating characters

---

# 9. What Is a Window?

Suppose:

```text
abcdef
```

A window could be:

```text
abc
```

Then move it:

```text
bcd
```

Then:

```text
cde
```

Then:

```text
def
```

Instead of repeatedly creating new substrings, we maintain two indexes:

```text
left
right
```

---

# 10. Longest Substring Without Repeating Characters

Problem:

> Find the length of the longest substring without duplicate characters.

Example:

```text
abcabcbb
```

Answer:

```text
3
```

Because:

```text
abc
```

is the longest substring without repetition.

---

## Brute Force Idea

Generate every substring and check whether it contains duplicates.

This is inefficient.

We can do better using:

```text
Sliding Window + HashSet
```

---

## Optimized Solution

```java
static int longestUniqueSubstring(String s) {

    Set<Character> set = new HashSet<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        while (set.contains(s.charAt(right))) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(s.charAt(right));

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

---

# 11. Dry Run — Sliding Window

Input:

```text
abcabcbb
```

Start:

```text
left = 0
```

Add:

```text
a
```

Window:

```text
a
```

Length:

```text
1
```

Add `b`:

```text
ab
```

Length:

```text
2
```

Add `c`:

```text
abc
```

Length:

```text
3
```

Next character:

```text
a
```

But `a` already exists.

Remove from the left:

```text
a
```

Now window becomes:

```text
bc
```

Add new `a`:

```text
bca
```

Length:

```text
3
```

Continue the same process.

Final answer:

```text
3
```

---

# 12. Sliding Window Template

This is an important template to remember.

```java
int left = 0;

for (int right = 0; right < s.length(); right++) {

    // Add s.charAt(right)

    while (/* window is invalid */) {

        // Remove s.charAt(left)

        left++;
    }

    // Update answer
}
```

The general idea is:

```text
Expand → Check → Shrink → Update
```

---

# 13. Pattern 4 — HashSet

A `HashSet` is useful when we only care about whether something exists.

For example:

> Does the string contain duplicate characters?

```java
static boolean hasDuplicateCharacters(String s) {

    Set<Character> set = new HashSet<>();

    for (char c : s.toCharArray()) {

        if (set.contains(c)) {
            return true;
        }

        set.add(c);
    }

    return false;
}
```

Example:

```text
"hello"
```

Process:

```text
h → add
e → add
l → add
l → already exists
```

Return:

```text
true
```

---

# 14. HashSet vs HashMap

This distinction is very important.

## HashSet

Use when you need:

```text
Does it exist?
```

Example:

```java
Set<Character> set = new HashSet<>();
```

---

## HashMap

Use when you need:

```text
How many times?
```

Example:

```java
Map<Character, Integer> map = new HashMap<>();
```

---

## Simple Rule

```text
Existence → HashSet

Frequency → HashMap
```

---

# 15. Pattern 5 — First Non-Repeating Character

Problem:

> Find the first character that appears only once.

Example:

```text
leetcode
```

Frequencies:

```text
l → 1
e → 3
t → 1
c → 1
o → 1
d → 1
```

The first character with frequency `1` is:

```text
l
```

---

## Solution

```java
static char firstUniqueChar(String s) {

    Map<Character, Integer> frequency = new HashMap<>();

    for (char c : s.toCharArray()) {
        frequency.put(
            c,
            frequency.getOrDefault(c, 0) + 1
        );
    }

    for (char c : s.toCharArray()) {

        if (frequency.get(c) == 1) {
            return c;
        }
    }

    return '\0';
}
```

Notice that we traverse the string twice.

That is still:

```text
O(n)
```

because:

```text
O(n) + O(n) = O(n)
```

---

# 16. Pattern 6 — Most Frequent Character

Problem:

> Find the character that occurs most frequently.

Example:

```text
banana
```

Frequency:

```text
b → 1
a → 3
n → 2
```

Answer:

```text
a
```

Implementation:

```java
static char mostFrequentChar(String s) {

    Map<Character, Integer> frequency = new HashMap<>();

    for (char c : s.toCharArray()) {
        frequency.put(
            c,
            frequency.getOrDefault(c, 0) + 1
        );
    }

    char result = '\0';
    int maxFrequency = 0;

    for (Map.Entry<Character, Integer> entry : frequency.entrySet()) {

        if (entry.getValue() > maxFrequency) {
            maxFrequency = entry.getValue();
            result = entry.getKey();
        }
    }

    return result;
}
```

---

# 17. Pattern 7 — Remove Duplicate Characters

Problem:

> Remove duplicate characters while keeping the first occurrence.

Example:

```text
programming
```

Possible result:

```text
progamin
```

Use a `HashSet`.

```java
static String removeDuplicates(String s) {

    Set<Character> seen = new HashSet<>();
    StringBuilder result = new StringBuilder();

    for (char c : s.toCharArray()) {

        if (!seen.contains(c)) {

            seen.add(c);
            result.append(c);
        }
    }

    return result.toString();
}
```

### Why StringBuilder?

Using:

```java
result += c;
```

inside a loop can create many temporary `String` objects.

`StringBuilder` is more efficient for repeated modifications.

---

# 18. Pattern 8 — Group Anagrams

Suppose we have:

```text
["eat", "tea", "tan", "ate", "nat", "bat"]
```

We want:

```text
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

Anagrams have the same sorted characters.

For example:

```text
eat → aet
tea → aet
ate → aet
```

Therefore:

```text
aet
```

can be used as the key.

---

## Solution

```java
static List<List<String>> groupAnagrams(String[] words) {

    Map<String, List<String>> groups = new HashMap<>();

    for (String word : words) {

        char[] chars = word.toCharArray();

        Arrays.sort(chars);

        String key = new String(chars);

        groups
            .computeIfAbsent(key, k -> new ArrayList<>())
            .add(word);
    }

    return new ArrayList<>(groups.values());
}
```

---

# 19. Group Anagrams — Complexity

Suppose:

```text
n = number of words
k = average word length
```

For every word, we sort its characters.

Sorting one word:

```text
O(k log k)
```

For `n` words:

```text
O(n × k log k)
```

Space:

```text
O(n × k)
```

because we store the grouped strings.

---

# 20. Better Group Anagrams Using Frequency

Instead of sorting every word, we can build a frequency signature.

For example:

```text
eat
```

has:

```text
a → 1
e → 1
t → 1
```

We can create a key representing those counts.

For lowercase English letters:

```java
static List<List<String>> groupAnagrams(String[] words) {

    Map<String, List<String>> groups = new HashMap<>();

    for (String word : words) {

        int[] frequency = new int[26];

        for (char c : word.toCharArray()) {
            frequency[c - 'a']++;
        }

        StringBuilder key = new StringBuilder();

        for (int count : frequency) {
            key.append('#').append(count);
        }

        groups
            .computeIfAbsent(key.toString(), k -> new ArrayList<>())
            .add(word);
    }

    return new ArrayList<>(groups.values());
}
```

This avoids sorting.

Complexity:

```text
Time: O(n × k)
```

instead of:

```text
O(n × k log k)
```

---

# 21. Pattern 9 — Reverse Words

Example:

```text
"I love Java"
```

Output:

```text
"Java love I"
```

A simple approach:

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

### Important

The regex:

```java
"\\s+"
```

means:

```text
one or more whitespace characters
```

So it handles multiple spaces.

---

# 22. Pattern 10 — Character Comparison

Sometimes a problem can be solved simply by comparing characters directly.

Example:

> Check whether two strings are equal ignoring case.

```java
static boolean sameIgnoreCase(String s, String t) {
    return s.equalsIgnoreCase(t);
}
```

Or manually:

```java
static boolean sameIgnoreCase(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    for (int i = 0; i < s.length(); i++) {

        char a = Character.toLowerCase(s.charAt(i));
        char b = Character.toLowerCase(t.charAt(i));

        if (a != b) {
            return false;
        }
    }

    return true;
}
```

---

# 23. Pattern 11 — Prefix Problems

A prefix is the beginning portion of a string.

For:

```text
flower
```

Prefixes include:

```text
f
fl
flo
flow
flowe
flower
```

---

# 24. Longest Common Prefix

Example:

```text
["flower", "flow", "flight"]
```

Longest common prefix:

```text
"fl"
```

---

## Solution

```java
static String longestCommonPrefix(String[] words) {

    if (words == null || words.length == 0) {
        return "";
    }

    String prefix = words[0];

    for (int i = 1; i < words.length; i++) {

        while (!words[i].startsWith(prefix)) {

            prefix = prefix.substring(0, prefix.length() - 1);

            if (prefix.isEmpty()) {
                return "";
            }
        }
    }

    return prefix;
}
```

---

# 25. Better Way to Think About Prefix Problems

Given:

```text
flower
flow
flight
```

Compare character by character:

```text
f == f
l == l
o != i
```

Therefore:

```text
fl
```

is the answer.

The key pattern is:

```text
Compare the same index across strings.
```

---

# 26. Pattern 12 — Substring Search

A substring is a continuous portion of a string.

Example:

```text
String: "hello world"

Substring: "world"
```

Java provides:

```java
contains()
```

Example:

```java
String s = "hello world";

System.out.println(s.contains("world"));
```

Output:

```text
true
```

---

# 27. Finding a Substring Position

Use:

```java
indexOf()
```

Example:

```java
String s = "hello world";

int index = s.indexOf("world");

System.out.println(index);
```

Output:

```text
6
```

If the substring does not exist:

```text
-1
```

---

# 28. Pattern 13 — Sliding Window + Frequency Map

A more advanced and very common pattern is:

```text
Sliding Window + Frequency Counting
```

This is useful when the problem says:

> Find the longest/shortest substring satisfying a frequency condition.

For example:

> Find the longest substring containing at most 2 distinct characters.

We can maintain:

```text
HashMap<Character, Integer>
```

for the current window.

---

## Template

```java
Map<Character, Integer> frequency = new HashMap<>();

int left = 0;

for (int right = 0; right < s.length(); right++) {

    char current = s.charAt(right);

    frequency.put(
        current,
        frequency.getOrDefault(current, 0) + 1
    );

    while (frequency.size() > 2) {

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

    // Update answer
}
```

This template is extremely useful in interviews.

---

# 29. Pattern 14 — Stack With Strings

Some string problems are actually stack problems.

Example:

```text
"()[]{}"
```

We need to check whether brackets are balanced.

Use:

```text
Stack
```

In modern Java:

```java
Deque<Character> stack = new ArrayDeque<>();
```

Example:

```java
static boolean isValid(String s) {

    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {

        if (c == '(' || c == '[' || c == '{') {

            stack.push(c);

        } else {

            if (stack.isEmpty()) {
                return false;
            }

            char top = stack.pop();

            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {

                return false;
            }
        }
    }

    return stack.isEmpty();
}
```

This belongs conceptually to the **Stack** topic, but it is useful to recognize it while solving string problems.

---

# 30. Pattern Recognition — Most Important Skill

When you see a string problem, ask these questions.

### Question 1

Does the problem compare both ends?

Think:

```text
Two Pointers
```

---

### Question 2

Does the problem ask for frequency?

Think:

```text
HashMap
```

or:

```text
int[]
```

---

### Question 3

Does it ask whether something exists?

Think:

```text
HashSet
```

---

### Question 4

Does it ask for longest/shortest substring?

Think:

```text
Sliding Window
```

---

### Question 5

Does it involve matching brackets?

Think:

```text
Stack
```

---

### Question 6

Does it ask about anagrams?

Think:

```text
Frequency Counting
```

or:

```text
Sorting
```

---

### Question 7

Does it ask about a common beginning?

Think:

```text
Prefix
```

---

# 31. Common String Patterns Cheat Sheet

| Problem | Pattern |
|---|---|
| Reverse string | Two Pointers |
| Palindrome | Two Pointers |
| Valid palindrome | Two Pointers |
| Character frequency | HashMap |
| Lowercase frequency | `int[26]` |
| First unique character | HashMap |
| Most frequent character | HashMap |
| Duplicate characters | HashSet |
| Remove duplicates | HashSet + StringBuilder |
| Anagram | Frequency Count |
| Group anagrams | HashMap |
| Longest unique substring | Sliding Window |
| Longest substring with K distinct | Sliding Window + HashMap |
| Minimum window | Sliding Window + HashMap |
| Reverse words | String / StringBuilder |
| Longest common prefix | Prefix |
| Bracket validation | Stack |
| Substring search | `indexOf()` / pattern matching |

---

# 32. Brute Force vs Optimized

One of the most important DSA skills is learning to improve a brute-force solution.

For example:

## Problem

Find the longest substring without repeating characters.

### Brute Force

Generate every substring.

For each substring:

```text
Check duplicates
```

This can become:

```text
O(n²)
```

or worse depending on the duplicate-checking implementation.

### Optimized

Use:

```text
Sliding Window + HashSet
```

Complexity:

```text
O(n)
```

The key improvement is that we don't restart the entire search for every substring.

---

# 33. Why Sliding Window Is O(n)

At first, the nested loop may look like:

```java
for (...) {

    while (...) {
        ...
    }
}
```

This does **not automatically mean O(n²)**.

Why?

Because `left` only moves forward.

For a string of length `n`:

```text
right → moves at most n times
left  → moves at most n times
```

Therefore total operations are approximately:

```text
n + n
```

which is:

```text
O(n)
```

This is called **amortized analysis**.

---

# 34. Important Edge Cases

Always consider these cases in string problems.

### Empty string

```text
""
```

---

### Single character

```text
"a"
```

---

### All characters same

```text
"aaaaa"
```

---

### All characters unique

```text
"abcdef"
```

---

### Spaces

```text
"hello world"
```

---

### Leading/trailing spaces

```text
"  hello  "
```

---

### Uppercase/lowercase

```text
"Hello"
```

---

### Special characters

```text
"a@b#c"
```

---

### Unicode characters

```text
"你好"
```

Do not blindly assume every string contains only lowercase English letters.

---

# 35. Choosing HashMap vs int[]

Use:

```java
HashMap<Character, Integer>
```

when:

- Character range is unknown
- Input can contain many different characters
- You want readable/general code

Use:

```java
int[26]
```

when:

- Only lowercase English letters are allowed

Use:

```java
int[128]
```

or:

```java
int[256]
```

only when the problem explicitly defines a suitable ASCII range.

---

# 36. Common Mistakes

## Mistake 1 — Using `==` for String content

Wrong:

```java
if (s1 == s2)
```

Correct:

```java
if (s1.equals(s2))
```

---

## Mistake 2 — Forgetting Case Sensitivity

These are different:

```text
"Java"
"java"
```

Use:

```java
equalsIgnoreCase()
```

when appropriate.

---

## Mistake 3 — Assuming Only Lowercase Characters

This can fail:

```java
frequency[c - 'a']++;
```

if `c` is:

```text
'A'
'1'
'@'
```

Only use this technique when the constraints guarantee lowercase letters.

---

## Mistake 4 — Creating Too Many Strings

Repeatedly doing:

```java
result += c;
```

inside a large loop can be inefficient.

Prefer:

```java
StringBuilder
```

for repeated modifications.

---

## Mistake 5 — Forgetting Window Shrinking

In sliding-window problems, when the condition becomes invalid, you usually need:

```java
while (condition) {
    // remove left character
    left++;
}
```

---

# 37. Interview Questions

## Beginner

### 1. How do you reverse a string?

Use:

```text
Two Pointers
```

or:

```text
StringBuilder.reverse()
```

---

### 2. How do you check whether a string is a palindrome?

Use two pointers:

```text
left
right
```

Compare characters and move inward.

---

### 3. How do you count character frequencies?

Use:

```text
HashMap<Character, Integer>
```

or:

```text
int[26]
```

---

### 4. How do you check if two strings are anagrams?

Compare their character frequencies.

---

### 5. HashMap vs HashSet?

```text
HashMap → key-value / frequency

HashSet → unique values / existence
```

---

## Intermediate

### 6. Find the first non-repeating character.

Use:

```text
HashMap + second traversal
```

---

### 7. Find the longest substring without repeating characters.

Use:

```text
Sliding Window + HashSet
```

---

### 8. Group anagrams.

Use:

```text
HashMap
```

with either:

```text
sorted string
```

or:

```text
frequency signature
```

as the key.

---

### 9. Find the longest common prefix.

Compare characters at the same positions across strings.

---

### 10. Why is sliding window often O(n)?

Because the left and right pointers each move forward at most `n` times.

---

# 38. Practice Problems

## Easy

1. Reverse a string.
2. Check palindrome.
3. Count vowels.
4. Count character frequency.
5. Find first non-repeating character.
6. Find duplicate characters.
7. Remove duplicate characters.
8. Check whether two strings are anagrams.
9. Reverse words in a sentence.
10. Find longest common prefix.

---

## Medium

1. Longest substring without repeating characters.
2. Longest substring with at most K distinct characters.
3. Group anagrams.
4. Minimum window substring.
5. Longest repeating character replacement.
6. Permutation in string.
7. Find all anagrams in a string.
8. Longest palindromic substring.
9. String compression.
10. Decode a string.

---

## Hard

1. Minimum Window Substring.
2. Regular Expression Matching.
3. Wildcard Matching.
4. Edit Distance.
5. Word Break II.
6. Palindrome Partitioning II.
7. Implement substring search efficiently.
8. Advanced pattern matching algorithms.

---

# 39. String Pattern Recognition Guide

When solving a new problem, use this mental checklist:

```text
String Problem
      |
      ├── Compare both ends?
      |       → Two Pointers
      |
      ├── Need frequency?
      |       → HashMap / Frequency Array
      |
      ├── Need existence?
      |       → HashSet
      |
      ├── Longest/shortest substring?
      |       → Sliding Window
      |
      ├── Anagram?
      |       → Frequency Counting
      |
      ├── Common beginning?
      |       → Prefix
      |
      ├── Brackets?
      |       → Stack
      |
      └── Repeated modification?
              → StringBuilder
```

---

# 40. Important Java Methods

Keep these methods in mind during interviews.

```java
s.length()
s.charAt(i)
s.toCharArray()
s.substring(start)
s.substring(start, end)
s.indexOf("abc")
s.lastIndexOf("abc")
s.contains("abc")
s.startsWith("abc")
s.endsWith("abc")
s.equals(t)
s.equalsIgnoreCase(t)
s.toLowerCase()
s.toUpperCase()
s.trim()
s.split("\\s+")
s.replace("a", "b")
```

For `StringBuilder`:

```java
sb.append(...)
sb.delete(...)
sb.insert(...)
sb.reverse()
sb.toString()
```

---

# 41. Quick Complexity Reference

| Operation / Pattern | Typical Complexity |
|---|---:|
| Traverse string | O(n) |
| Reverse string | O(n) |
| Palindrome | O(n) |
| Frequency count | O(n) |
| HashSet lookup | O(1) average |
| HashMap lookup | O(1) average |
| Sliding Window | O(n) |
| Sort characters | O(n log n) |
| Group anagrams using sorting | O(n × k log k) |
| Group anagrams using frequency | O(n × k) |

---

# 42. Final Takeaway

The most important thing to learn from string problems is **pattern recognition**.

Don't try to memorize 100 individual solutions.

Instead, remember:

```text
Palindrome
    ↓
Two Pointers
```

```text
Frequency
    ↓
HashMap / int[]
```

```text
Existence
    ↓
HashSet
```

```text
Longest / Shortest Substring
    ↓
Sliding Window
```

```text
Anagram
    ↓
Frequency Counting
```

```text
Common Prefix
    ↓
Prefix Comparison
```

```text
Brackets
    ↓
Stack
```

```text
Repeated String Modification
    ↓
StringBuilder
```

Once these patterns become familiar, many string interview questions become variations of problems you already know.

---

# 43. Checklist

Before moving to the next topic, make sure you understand:

- [ ] What two pointers are
- [ ] How to check a palindrome
- [ ] What frequency counting means
- [ ] HashMap vs HashSet
- [ ] `int[26]` frequency array
- [ ] How to find the first unique character
- [ ] How to find duplicate characters
- [ ] How to check anagrams
- [ ] How to group anagrams
- [ ] What sliding window means
- [ ] How to find the longest unique substring
- [ ] Sliding Window + HashMap
- [ ] How to reverse words
- [ ] Longest common prefix
- [ ] When Stack can be used for strings
- [ ] Common string edge cases
- [ ] How to identify the correct pattern from a problem statement
- [ ] Time and space complexity of common string patterns

---

# Next Topic

The next file in the DSA roadmap is:

```text
04-linked-list/linked-list.md
```

This will introduce **Linked Lists**, including nodes, references, traversal, insertion, deletion, and the core concepts needed before moving to singly and doubly linked lists.