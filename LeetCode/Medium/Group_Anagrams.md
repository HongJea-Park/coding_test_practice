# Group Anagrams

[링크](https://leetcode.com/problems/group-anagrams/)

<br>

## Description

Given an array of strings `strs`, group the **anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

<br>

### Example 1:

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

<br>

### Example 2:

```
Input: strs = [""]
Output: [[""]]
```

<br>

### Example 3:

```
Input: strs = ["a"]
Output: [["a"]]
```

<br>

## Constraints:

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lower-case English letters.

<br>
<br>
<br>

# Solution

해시 테이블 자료형인 dictionary를 활용하여 정렬된 문자열을 key로, 문자열 원본을 리스트로 묶어 해당 key에 대한 value로 저장하면 쉽게 풀 수 있다.

```python
from collections import defaultdict


class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groupDict = defaultdict(list)
        for s in strs:
            groupDict[''.join(sorted(s))].append(s)
        return groupDict.values()
```