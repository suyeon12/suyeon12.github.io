---

layout: post

title: Codility NumberSolitaire

summary: sys.maxsize

---

## Lesson 17. Dynamic programming - NumberSolitaire

### 문제

A game for one player is played on a board consisting of N consecutive squares, numbered from 0 to N − 1. There is a number written on each square. A non-empty array A of N integers contains the numbers written on the squares. Moreover, some squares can be marked during the game.

At the beginning of the game, there is a pebble on square number 0 and this is the only square on the board which is marked. The goal of the game is to move the pebble to square number N − 1.

During each turn we throw a six-sided die, with numbers from 1 to 6 on its faces, and consider the number K, which shows on the upper face after the die comes to rest. Then we move the pebble standing on square number I to square number I + K, providing that square number I + K exists. If square number I + K does not exist, we throw the die again until we obtain a valid move. Finally, we mark square number I + K.

After the game finishes (when the pebble is standing on square number N − 1), we calculate the result. The result of the game is the sum of the numbers written on all marked squares.

For example, given the following array:

A[0] = 1
 A[1] = -2
 A[2] = 0
 A[3] = 9
 A[4] = -1
 A[5] = -2

one possible game could be as follows:

> - the pebble is on square number 0, which is marked;
> - we throw 3; the pebble moves from square number 0 to square number 3; we mark square number 3;
> - we throw 5; the pebble does not move, since there is no square number 8 on the board;
> - we throw 2; the pebble moves to square number 5; we mark this square and the game ends.

The marked squares are 0, 3 and 5, so the result of the game is 1 + 9 + (−2) = 8. This is the maximal possible result that can be achieved on this board.

Write a function:

> def solution(A)

that, given a non-empty array A of N integers, returns the maximal result that can be achieved on the board represented by array A.

For example, given the array

A[0] = 1
 A[1] = -2
 A[2] = 0
 A[3] = 9
 A[4] = -1
 A[5] = -2

the function should return 8, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [2..100,000];
> - each element of array A is an integer within the range [−10,000..10,000].



<br/>

### 상황 분석

주사위를 던져 나온 수만큼 이동하고 마지막 인덱스에 도착하면 게임이 끝난다. 인덱스 총합의 최댓값은?

<br/>

### 코드 작성

```python
def solution(A):
    answer = [-100000000] * len(A)
    answer[0] = A[0]
    #print(answer)
    for i in range(1, len(A)):
        max_score = A[-1]
        for j in range(1,7):
            if i - j >= 0:
                max_score = max(answer[i-j]+A[i], max_score)
        answer[i] = max_score
    return answer[-1]
```

괜찮겠지? 하고 제출했는데 `For example, for the input [-4, -10, -7] the solution returned a wrong answer (got -7 expected -11).`  ㅎㅎ.. 아니 왜 max_score를 A[-1]로 한 거지? 원래 answer[-1]하려 했는데...

<br/>

```python
import sys

def solution(A):
    answer = [0] * len(A)
    answer[0] = A[0]
    #print(answer)
    for i in range(1, len(A)):
        max_score = (-sys.maxsize) - 1
        for j in range(1,7):
            if i - j >= 0:
                max_score = max(answer[i-j]+A[i], max_score)
        answer[i] = max_score
    return answer[-1]
```

그래서 그냥 아예 최솟값으로 바꿔버렸다. 1부터 6까지 반복하며 최고점을 갱신해줌

<br/>

### 다른 코드

```python
def solution(A):
    N = len(A)
    answer = [A[0]] * (N + 6)

    for i in range(1, N):
        answer[i + 6] = max(answer[i : i + 6]) + A[i]

    return answer[-1]
```

간단하네

<br/>

```python
def solution(A):
    # write your code in Python 3.6
    maxSum = [-1000000001 for i in range(len(A))]
    maxSum[len(A)-1] = A[-1]

    for i in range(len(A)-2, -1, -1) :
        for j in range(1, 7) :
            if i+j < len(A) :
                maxSum[i] = max(maxSum[i], A[i] + maxSum[i+j])
            else :
                break

    return maxSum[0]
```

여기서는 끝에서부터 계산

<br/>

### 함수 정리

`sys.maxsize` int 최댓값



<br/>

### 느낀점

DP에 관해 더 알아봐야겠다 사실 예전에 조사해둔게 있긴 하지만.. ㅋㅋㅋ
