# Add Two Numbers

[링크](https://leetcode.com/problems/add-two-numbers/)

<br>

## Description

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

<br>

### Example 1:

![example1](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

### Example 2:

```
Input: l1 = [0], l2 = [0]
Output: [0]

```

### Example 3:

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

<br>

## Constraints: 

- The number of nodes in each linked list is in the range [1, 100].
- 0 <= Node.val <= 9
- It is guaranteed that the list represents a number that does not have leading zeros.

<br>
<br>
<br>

# Solution

각 숫자에 대해 자리수에 해당하는 노드의 수를 더하고 10으로 나누어 나머지, 즉 첫 번째 자리 숫자만 해당 노드에 할당하고 자릿수를 넘어가는 숫자(몫)는 다음 노드로 넘겨준다.
이 때 각 숫자별로 노드의 길이가 다를 수 있으므로 예외를 처리해주어야 한다.

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:

        tmp, add = divmod(l1.val+l2.val, 10)
        l1 = l1.next
        l2 = l2.next
        answer = ListNode(add)
        answer_parent = answer
        
        while l1 or l2 or tmp:
            v1 = l1.val if l1 is not None else 0
            v2 = l2.val if l2 is not None else 0
            l1 = l1.next if l1 is not None else None
            l2 = l2.next if l2 is not None else None
            tmp, add = divmod(v1+v2+tmp, 10)
            answer_parent.next = ListNode(add)
            answer_parent = answer_parent.next

        return answer
```
![Imgur](https://i.imgur.com/E3se9dW.png)
![Imgur](https://i.imgur.com/oJ9bMiF.png)