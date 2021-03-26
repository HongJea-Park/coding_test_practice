# Longest Palindromic Substring

[링크](https://leetcode.com/problems/longest-palindromic-substring/)

<br>

## Description

Given a string `s`, return the longest palindromic substring in `s`.

<br>

### Example 1:

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

<br>

### Example 2:

```
Input: s = "cbbd"
Output: "bb"
```

<br>

### Example 3:

```
Input: s = "a"
Output: "a"
```

<br>

### Example 4:

```
Input: s = "ac"
Output: "a"
```

<br>

## Constraints:

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters (lower-case and/or upper-case),

<br>
<br>
<br>

# Solution

이 문제는 Programmers Level 3 [가장 긴 팰린드롬](https://github.com/HongJea-Park/coding_test_practice/blob/master/Programmers/level3/%EA%B0%80%EC%9E%A5_%EA%B8%B4_%ED%8C%B0%EB%A6%B0%EB%93%9C%EB%A1%AC.md) 문제와 동일한 문제다.
Programmers 문제와 마찬가지로 두 가지 풀이가 가능하다.

1. 가능한 모든 문자열의 길이를 순회하며 필린드롬 여부를 체크하는 방법
2. 기준점을 정의하고 기준점으로부터 투 포인터를 활용하여 해당 기준점에서 가장 긴 길이의 팰린드롬 길이를 체크하는 방법

당연하게도 첫 번째 방법은 매우 비효율적이며 시간이 상당히 소모된다.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        sLength = len(s)
        for length in range(sLength, 0, -1):
            for idx in range(sLength-length+1):
                substr = s[idx:idx+length]
                if substr == substr[::-1]:
                    return substr
```
![1-detail](https://i.imgur.com/PSSazLP.png)

<br>

두 번째 방법인 투 포인터는 이러한 문제에서 매우 효율적임을 알 수 있다.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        sLength = len(s)
        def expand(left, right):
            while left >= 0 and right < sLength and s[left] == s[right]:
                left -= 1
                right += 1
            return s[left+1:right]
        
        if sLength < 2 or s == s[::-1]:
            return s
        
        answer = ''
        for i in range(sLength-1):
            answer = max(answer, expand(i, i+1), expand(i, i+2), key=len)
        
        return answer
```
![2-detail](https://i.imgur.com/DHtPKGi.png)