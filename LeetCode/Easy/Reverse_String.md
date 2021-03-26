# Reverse String

[링크](https://leetcode.com/problems/reverse-string/)

<br>

## Description

Write a function that reverses a string. The input string is given as an array of characters `s`.

<br>

### Example 1:

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

<br>

### Example 2:

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

<br>

## Constraints:

- `1 <= s.length <= 10^5`
- `s[i]` is a printable ascii character.

<br>
<br>
<br>

# Solution

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        s.reverse()
```