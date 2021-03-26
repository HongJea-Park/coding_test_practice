# Reorder Data in Log Files

[링크](https://leetcode.com/problems/reorder-data-in-log-files/)

<br>

## Description

You are given an array of `logs`. Each log is a space-delimited string of words, where the first word is the **identifier**.

There are two types of logs:

- **Letter-logs**: All words (except the identifier) consist of lowercase English letters.
- **Digit-logs**: All words (except the identifier) consist of digits.

Reorder these logs so that:

1. The **letter-logs** come before all **digit-logs**.
2. The **letter-logs** are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
3. The **digit-logs** maintain their relative ordering.

Return the final order of the logs.

<br>

### Example 1:

```
Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
Explanation:
The letter-log contents are all different, so their ordering is "art can", "art zero", "own kit dig".
The digit-logs have a relative order of "dig1 8 1 5 1", "dig2 3 6".
```

<br>

### Example 2:

```
Input: logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
Output: ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
```

<br>

## Constraints:

- `1 <= logs.length <= 100`
- `3 <= logs[i].length <= 100`
- All the tokens of `logs[i]` are separated by a **single** space.
- `logs[i]` is guaranteed to have an identifier and at least one word after the identifier.

<br>
<br>
<br>

# Solution

`파이썬 알고리즘 인터뷰` 에서 소개하는 방법은 두 개의 list를 구성하여 각 타입별로 저장한 뒤, sort 후 다시 병합하는 방법이다.

```python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        letter, digit = [], []
        for log in logs:
            if log.split()[1].isalpha():
                letter.append(log)
            else:
                digit.append(log)
        letter.sort(key=lambda x: (x.split()[1:], x.split()[0]))
        return letter + digit
```

<br>

하지만 이런 방법은 memory 측면에서 매우 비효율적일 수 있다. 내장함수 sorted의 argument인 key에 조건에 맞는 함수를 넣어줌으로써 추가적인 리스트를 따로 저장하지 않고 바로 정렬할 수 있다.

```python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:

        def sortBy(log):
            identifier, words = log.split(" ", maxsplit=1)
            return (0, words, identifier) if words[0].isalpha() else (1, )

        return sorted(logs, key=sortBy)
```

두 방법 사이에 속도 차이는 없는 것으로 보이나 메모리 측면에서는 확연한 차이를 보여준다.

![memory](https://i.imgur.com/QUj392d.png)
![memory](https://i.imgur.com/t9OT4U0.png)