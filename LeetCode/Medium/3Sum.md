# 3Sum

[링크](https://leetcode.com/problems/3sum/)

<br>

## Description

Given an array `nums` of `n` integers, are there elements `a, b, c` in `nums` such that `a + b + c = 0`? Find all unique triplets in the array which gives the sum of zero.

Notice that the solution set must not contain duplicate triplets.

<br>

### Example 1:

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

### Example 2:

```
Input: nums = []
Output: []
```

### Example 3:

```
Input: nums = [0]
Output: []
```

<br>

## Constraints: 

- 0 <= nums.length <= 3000
- ![img](https://bit.ly/3tG38Dq) <= nums[i] <= ![img](https://bit.ly/394Bs39)

<br>
<br>
<br>

# Solution

기본적으로 `nums` 내에 존재하는 세 수의 합이 0이 되기 위해서는 최소 하나의 음수와 최소 하나의 양수는 포함되어야 한다. 때문에 주어진 `nums`는 가장 먼저 sort해두어야 한다.

이후 두 가지 풀이법을 고민해볼 수 있다.

1. sorted `nums` 에서 `a, b` 를 가져온 뒤 `a+b+c=0` 을 만족하는 `c` 가 `a, b` 사이에 존재하는지 binary search를 통해 탐색하는 방법
2. 기준점을 정의한 뒤 Two Pointer 개념을 활용하여 sublist의 범위를 좁혀나가며 `a+b+c=0` 을 만족하는 인덱스를 탐색하는 방법
3. `nums` 내에 존재하는 모든 element를 count한 뒤 positive number와 negative number로 나누고 positive number와 negative number를 pairwise로 묶은 뒤 나머지 숫자가 `nums` 내에 존재하는지 탐색하는 방법

<br>

## 첫 번째 방법

첫 번째 binary search를 활용하는 방법은 `a, b`를 pairwise로 선택해야하는데, 이 때 경우의 수는 `nC2`만큼 나오게 되어 `nums`의 길이가 길어질수록 매우 많은 경우의 수를 찾아야 한다는 단점이 있다. binary search를 활용하기 위해서는 범위 left와 right가 정해져야 하는데, 기준이 되는 변수가 2개이다 보니 많은 경우의 수가 존재할 수 밖에 없는 것이다. 역시나 이 방식은 time limit exceeded로 실패한다.

```python
from itertools import combinations


class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        self.nums = nums
        numsLength = len(self.nums)
        answer = set([])
        if numsLength < 3:
            return answer
        for left, right in combinations(range(numsLength), 2):
            mid = self.binarySearch(left, right)
            if mid != -1:
                answer.add(self.list2tuple(self.nums[left], self.nums[mid], self.nums[right]))
        return answer

    def binarySearch(self, left: int, right: int):
        leftNum, rightNum = self.nums[left], self.nums[right]
        left += 1
        right -= 1
        while left <= right:
            mid = (left+right)//2
            sum_ = leftNum + self.nums[mid] + rightNum
            if sum_ == 0:
                return mid
            elif sum_ < 0:
                left = mid+1
            else:
                right = mid-1
        return -1
    
    def list2tuple(self, a, b, c):
        return tuple([a, b, c])
```
![실패](https://i.imgur.com/xL9Cn1u.png)

<br>

## 두 번째 방법

Two Pointer를 활용하는 두 번째 방법은 이러한 문제에서 binary search보다 효율적인 모습을 보여준다. 
Two Pointer는 두 개의 index가 정의되어야 한다. 그러기 위해 우선 기준이 되는 `basis` index를 정의하고, `basis` index 우측에 `left` index와 `right` index를 정의한다. 이후 `nums[basis] + nums[left] + nums[right] = 0` 을 만족하도록 right와 left의 범위를 좁혀나간다. 이러한 방식은 `left` index와 `right` index를 순차적으로 이동시키기 때문에 binary search보다 비효율적이라고 생각하였으나, binary search는 `left` index와 `right` index에 대한 경우의 수를 고려해야하기 때문에 해당 문제에선 더 효율적인 것 같다.

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        numsLength = len(nums)
        answer = []
        if numsLength < 3:
            return answer
        for basis in range(numsLength-2):
            if basis > 0 and nums[basis] == nums[basis-1]:
                continue
            left, right = basis+1, numsLength-1
            while left < right:
                sum_ = nums[basis]+nums[left]+nums[right]
                if sum_ < 0:
                    left += 1
                elif sum_ > 0:
                    right -= 1
                else:
                    answer.append([nums[basis], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left+1]:
                        left += 1
                    while left < right and nums[right] == nums[right-1]:
                        right -= 1
                    left += 1
                    right -= 1
        return answer
```

![detail](https://i.imgur.com/zDCI4jm.png)
![memory](https://i.imgur.com/IL9ot78.png)

<br>

## 세 번째 방법

`nums` 내에 존재하는 모든 element를 count한 뒤 positive number와 negative number로 나누는 방법은 효율성 측면에서는 가장 좋은 모습을 보인다. 우선 `collections.Counter` 모듈을 사용하여 element를 count한다. `[0, 0, 0]` 인 경우가 있을 수 있으므로 예외처리를 해준다. 이후 positive number와 negative number로 나누어 list를 저장하고 pairwise로 비교한다. Binary search와 비슷하게 2중 for문을 사용해야 하지만, 2중 for문을 구성하는 list의 범위가 더 작기 때문에 훨씬 효율적으로 볼 수 있다.
이후 `-(pos+neg)` 가 nums에 있었는지 확인하고, 몇 가지 경우를 고려하여 3개의 조건문을 통해 정답에 append 해주면 효율적으로 풀리는 것을 확인할 수 있다.
이 풀이법은 속도측면에서는 효율적이나 메모리 측면에서는 비효율적이다. `Counter` 모듈의 결과를 저장해야하며 nums가 존재함에도 불구하고 positive numbers와 negative numbers를 따로 저장하고 있어야하기 때문이다.

```python
from collections import Counter


class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        count = Counter(nums)
        answer = [] if count[0] < 3 else [[0, 0, 0]]
        posNums, negNums = [], []
        for n in count.keys():
            if n > 0:
                posNums.append(n)
            elif n < 0:
                negNums.append(n)
        for pos in posNums:
            for neg in negNums:
                c = -(pos+neg)
                if c in count:
                    if c == pos and count[pos] >= 2:
                        answer.append([neg, pos, pos])
                    elif c == neg and count[neg] >= 2:
                        answer.append([neg, neg, pos])
                    elif neg < c < pos:
                        answer.append([neg, c, pos])
        return answer
```

![detail](https://i.imgur.com/hy8FwUT.png)
![memory](https://i.imgur.com/o0jnPKj.png)