# N-Queen

[링크](https://programmers.co.kr/learn/courses/30/lessons/12952)

<br>

## 문제 설명

가로, 세로 길이가 n인 정사각형으로된 체스판이 있습니다. 체스판 위의 n개의 퀸이 서로를 공격할 수 없도록 배치하고 싶습니다.

예를 들어서 n이 4인경우 다음과 같이 퀸을 배치하면 n개의 퀸은 서로를 한번에 공격 할 수 없습니다.

![img1](https://i.imgur.com/lt2zdK6.png)

![img2](https://i.imgur.com/5c5EUrq.png)

체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.

<br>

## 제한사항

- 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
- n은 12이하의 자연수 입니다.

<br>

## 입출력 예

| n	| result |
| :-: | :-: |
| 4	| 2 |

<br>

### 입출력 예 설명

1. 입출력 예 #1
    
    문제의 예시와 같습니다.

<br>
<br>
<br>

# Solution

위 문제를 보고 가장 먼저 든 생각은 DFS를 통해 탐색하는 방법이었다. 체스판에서 퀸의 위치를 좌표로 정했을 때 방문 가능한 노드들에 대해 DFS로 순차적으로 방문하며 다음 퀸이 위치할 수 있다면 또 다음 퀸의 위치를 찾아가면 되는 것이다. 이러한 방식으로 가능한 모든 경우를 추적하는 방법을 Backtracking 이라고 따로 부르는 듯 하다.

Backtracking은 좀 더 general purpose algorithm으로 볼 수 있으며, DFS는 backtracking의 specific form이라고 한다. DFS는 tree 구조에서나 적용시킬 수 있기 때문으로 보인다.

Backtracking은 다음과 같은 원리로 작동된다.

1. 특정 노드(상태)를 점검

2. 목적에 부합하지 않으면 배제(Pruning). Tree 관점에서 보면 가지를 쳐내는 의미

3. 만약 배제됐다면 부모 노드로 돌아가서(DFS와 같은 원리) 다음 자식 노드를 탐색

4. 만약 배제되지 않았다면, 해당 노드에서 자식 노드를 탐색(네트워크를 더 깊게 탐색)

Backtracking을 구현하는 방법은 2가지다. 첫 번째는 DFS와 마찬가지로 stack을 활용하여 노드들을 stack에 저장하는 방법이다. 두 번째 방법은 재귀(recursive) 함수를 활용하는 방법이다. 두 방법에서 공통적으로 필요한 부분은 해당 상태 혹은 노드가 조건을 만족하는지에 대한 여부를 확인해주는 것이다. 이 부분이 없다면 탐색 대상의 수가 훨씬 많아져 시간 초과가 발생할 수 있다.

Backtracking을 활용하는 가장 대표적인 문제가 N-Queens 문제라고 한다. N-Queens 문제를 backtracking을 통해 탐색하는 과정을 그림으로 표현하면 아래와 같다.

![backtracking](https://2.bp.blogspot.com/-ZRO-A_DQe3U/VYHDjMoS2dI/AAAAAAAB_Lk/z-a_eCiWmo8/s1600/queens4_backtrack.png)

DFS와 마찬가지로 stack을 이용한 풀이다. 문제에서 `n` 이 1도 가능하기 때문에 이에 대한 예외처리만 해주면 stack을 이용한 일반적인 DFS와 같은 풀이가 된다.

```python
def is_possible(col, board):
    row = len(board)
    for i in range(row):
        if board[i] == col or row-i == abs(board[i]-col):
            return False
    return True


def solution(n):
    count = 0
    stack = [[i] for i in range(n)]
    while stack:
        board = stack.pop()
        for col in range(n):
            if is_possible(col, board):
                tmp = board+[col]
                if len(tmp) == n:
                    count += 1
                else:
                    stack.append(tmp)
    return count
```

![stack](https://i.imgur.com/X0h7O5U.png)


두 번째 풀이법인 recursive한 함수 구조를 활용한 방법이다. 

```python
def is_possible(col, board):
    row = len(board)
    for i in range(row):
        if board[i] == col or row-i == abs(board[i]-col):
            return False
    return True


def recursiveDFS(n, board):
    if n == len(board):
        return 1
    count = 0
    for col in range(n):
        if is_possible(col, board):
            board.append(col)
            count += recursiveDFS(n, board)
            board.pop()
    return count


def solution(n):
    return recursiveDFS(n, [])
```

![recursive](https://i.imgur.com/vpXpwij.png)

두 방법에서 약간의 시간차이가 존재하는데, 이론적으로 왜 속도차이가 나는지는 잘 모르겠다.
