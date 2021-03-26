# Most Common Word

[링크](https://leetcode.com/problems/most-common-word/)

<br>

## Description

Given a string `paragraph` and a string array of the banned words `banned`, return the most frequent word that is not banned. It is **guaranteed** there is **at least one word** that is not banned, and that the answer is **unique**.

The words in `paragraph` are **case-insensitive** and the answer should be returned in **lowercase**.

<br>

### Example 1:

```
Input: paragraph = "Bob hit a ball, the hit BALL flew far after it was hit.", banned = ["hit"]
Output: "ball"
Explanation: 
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.
```

<br>

### Example 2:

```
Input: paragraph = "a.", banned = []
Output: "a"
```

<br>

## Constraints:

- `1 <= paragraph.length <= 1000`
- `paragraph` consists of English letters, space `' '`, or one of the symbols: `"!?',;."`.
- `0 <= banned.length <= 100`
- `1 <= banned[i].length <= 10`
- `banned[i]` consists of only lowercase English letters.


<br>
<br>
<br>

# Solution

```python
import re
from collections import Counter


class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        wordList = [w for w in re.sub('[^a-z]', ' ', paragraph.lower()).split() 
                        if w not in banned]
        return Counter(wordList).most_common(1)[0][0]
```