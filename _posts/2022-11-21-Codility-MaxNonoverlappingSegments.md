---

layout: post

title: Codility MaxNonoverlappingSegments

summary: lambda

---

## Lesson 16. Greedy algorithms - MaxNonoverlappingSegments

### 문제

Located on a line are N segments, numbered from 0 to N − 1, whose positions are given in arrays A and B. For each I (0 ≤ I < N) the position of segment I is from A[I] to B[I] (inclusive). The segments are sorted by their ends, which means that B[K] ≤ B[K + 1] for K such that 0 ≤ K < N − 1.

Two segments I and J, such that I ≠ J, are *overlapping* if they share at least one common point. In other words, A[I] ≤ A[J] ≤ B[I] or A[J] ≤ A[I] ≤ B[J].

We say that the set of segments is *non-overlapping* if it contains no two overlapping segments. The goal is to find the size of a non-overlapping set containing the maximal number of segments.

For example, consider arrays A, B such that:

A[0] = 1 B[0] = 5
 A[1] = 3 B[1] = 6
 A[2] = 7 B[2] = 8
 A[3] = 9 B[3] = 9
 A[4] = 9 B[4] = 10

The segments are shown in the figure below.

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/max_nonoverlapping_segments/static/images/auto/68b279360bc48af61d9d3bdfbe1d30fe.png)

The size of a non-overlapping set containing a maximal number of segments is 3. For example, possible sets are {0, 2, 3}, {0, 2, 4}, {1, 2, 3} or {1, 2, 4}. There is no non-overlapping set with four segments.

Write a function:

> def solution(A, B)

that, given two arrays A and B consisting of N integers, returns the size of a non-overlapping set containing a maximal number of segments.

For example, given arrays A, B shown above, the function should return 3, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..30,000];
> - each element of arrays A and B is an integer within the range [0..1,000,000,000];
> - A[I] ≤ B[I], for each I (0 ≤ I < N);
> - B[K] ≤ B[K + 1], for each K (0 ≤ K < N − 1).



<br/>

### 상황 분석

서로 겹치지 않는 선분을 만들어서 그 원소의 개수를 return 하면 된다

<br/>

### 코드 작성

```python
def solution(A, B):
    end = B[0]
    count = 1

    if len(A) < 1:
        return 0

    for i in range(len(A)):
        if end < A[i]:
            count += 1
            end = B[i]
    return count
```

일단 끝점을 설정해주고 `endpoint(B[0])`보다 `startpoint(A[i])`가 크면 개수를 하나 추가해준 후 끝점을 `B[i]`로 설정한다. 근데!!

`For example, for the input ([], []) the solution terminated unexpectedly.`

이런 오류가 하하하

<br/>

```python
def solution(A, B):

    if len(A) == 0:
        return 0
    
    count = 1
    end = B[0]

    for i in range(len(A)):
        if end < A[i]:
            count += 1
            end = B[i]
    return count
```

생각해보니 len(A) 판별이 위에 가야 하겠더라구 그래서 위치를 살짝 조절해줬다. 시간복잡도는 O(N)

<br/>

### 다른 코드

```python
def solution(A, B):
    n = len(A)
    lis = []
    # 길이가 0, 1인 경우는 아래 코드에서 처리 못함
    if n == 0:
        return 0
    elif n == 1:
        return 1
    # 시작, 끝 위치를 튜플로 묶어서 리스트에 넣어줌
    for a, b in zip(A, B):
        lis.append((a, b))
    # 먼저 끝점 기준 그 다음으로 시작점 기준으로 정렬
    lis.sort(key = lambda x : (x[1], x[0]))
    # 처음 스타트 줄
    end = lis[0][1]
    count = 1
	# 끝 점이 다음 줄의 시작 점보다 작으면 조건 만족
    for i in range(1, n):
        if end < lis[i][0]:
            end = lis[i][1]
            count += 1
    
    return count
```

어제 배웠던 `lambda`가 나왔다!

<br/>

### 함수 정리

`lambda 인자 : 표현식` [다음 블로그](https://kingofbackend.tistory.com/98) 참고

<br/>

### 느낀점

greedy 처음 해보는데 이게 greedy인가? 신기하다
