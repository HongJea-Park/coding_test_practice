# Valid Palindrome

[링크](https://leetcode.com/problems/valid-palindrome/)

<br>

## Description

Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases. 

<br>

### Example 1:

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

<br>

### Example 2:

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
``` 

<br>

## Constraints:

- `1 <= s.length <= 2 * 10^5`
- `s` consists only of printable ASCII characters.

<br>
<br>
<br>

# Solution

`파이썬 알고리즘 인터뷰` 교재에 나와있는 list의 pop을 활용하는 방법, deque를 활용하는 방법, regular expression(`re` 모듈)을 활용하는 방법이 소개되어 있는데, 그 외에도 내장함수 `filter` 를 활용하는 방법도 있다.

<br>

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = ''.join(filter(str.isalnum, s)).lower()
        return s == s[::-1]
```

<br>

![detail](https://i.imgur.com/7MWgujy.png)
![memory](https://i.imgur.com/MNHJabN.png)