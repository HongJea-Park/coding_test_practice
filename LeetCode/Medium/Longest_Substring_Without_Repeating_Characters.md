# Longest Substring Without Repeating Characters

[링크](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

<br>

## Description

Given a string `s`, find the length of the **longest substring** without repeating characters.

<br>

### Example 1:

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### Example 2:

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3:

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Example 4:

```
Input: s = ""
Output: 0
```

<br>

## Constraints: 

- 0 <= `s.length` <= 5 * ![img](https://bit.ly/2ONoznv)
- `s` consists of English letters, digits, symbols and spaces.


<br>
<br>
<br>

# Solution

이 문제는 문자열 내에 연속되지 않는 서로 다른 알파벳으로 구성된 substring을 찾아야하는 문제이다.
Example 3과 같이 subsequence가 아닌 substring을 찾아야 한다.

가장 먼저 생각해볼 수 있는 풀이법은 1부터 문자열의 길이까지 window 사이즈를 차례로 변화시켜가며 문자열을 탐색하는 방법이나, 이 방법은 당연히 computing cost가 큰 것을 알 수 있다.

효율적으로 문제를 풀 수 있는 방법은 다음과 같다. 우선 기준점의 인덱스를 0으로 시작하며 주어진 string을 차례로 살펴본다. 이 때 순서대로 탐색하는 char의 인덱스 정보를 dictionary에 계속 업데이트한다. 순서대로 탐색을 진행하며 이전에 탐색한 char가 다시 등장했을 때, 기준점의 인덱스를 char가 마지막으로 관측한(현재시점 이전) 지점으로 옮기며 substring의 최대길이를 계속해서 업데이트한다. 마지막으로 이러한 작업을 진행한 뒤 char의 인덱스 정보를 업데이트한다. 이러한 방식으로 접근하면 O(n)으로 문제를 해결할 수 있다.

<br>

```python
class Solution:
    def lengthOfLongestSubstring(self, string: str) -> int:
        basis, maxLength = 0, 0
        charDict = {}
        stringLength = len(string)
        for i, s in enumerate(string):
            idx = charDict.setdefault(s, -1)
            if idx != -1 and basis <= charDict[s]:
                basis = charDict[s]+1
            else:
                maxLength = max(maxLength, i-basis+1)
            charDict[s] = i
        return maxLength
```

![detail](https://i.imgur.com/zZL7Hjs.png)
![memory](https://i.imgur.com/pxTtsJl.png)