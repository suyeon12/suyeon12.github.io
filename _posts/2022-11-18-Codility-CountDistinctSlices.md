---

layout: post

title: Codility CountDistinctSlices

---

## Lesson 15. Caterpillar method - CountDistinctSlices

### 문제

An integer M and a non-empty array A consisting of N non-negative integers are given. All integers in array A are less than or equal to M.

A pair of integers (P, Q), such that 0 ≤ P ≤ Q < N, is called a *slice* of array A. The slice consists of the elements A[P], A[P + 1], ..., A[Q]. A *distinct slice* is a slice consisting of only unique numbers. That is, no individual number occurs more than once in the slice.

For example, consider integer M = 6 and array A such that:

A[0] = 3
 A[1] = 4
 A[2] = 5
 A[3] = 5
 A[4] = 2

There are exactly nine distinct slices: (0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2), (3, 3), (3, 4) and (4, 4).

The goal is to calculate the number of distinct slices.

Write a function:

> def solution(M, A)

that, given an integer M and a non-empty array A consisting of N integers, returns the number of distinct slices.

If the number of distinct slices is greater than 1,000,000,000, the function should return 1,000,000,000.

For example, given integer M = 6 and array A such that:

A[0] = 3
 A[1] = 4
 A[2] = 5
 A[3] = 5
 A[4] = 2

the function should return 9, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - M is an integer within the range [0..100,000];
> - each element of array A is an integer within the range [0..M].



<br/>

### 코드 작성

```python
def solution(M, A):
    check = [False] * (M + 1)
    front, back, answer = 0, 0, 0
    
    while back < len(A):
        if front < len(A) and not check[A[front]]:
            answer += front - back + 1
            #print("answer", answer)
            #print("front", front)
            #print("A[front]", A[front])
            check[A[front]] = True
            front += 1
        else:
            #print("back", back)
            #print("A[back]", A[back])
            check[A[back]] = False
            back += 1
        if answer > 1000000000:
            return 1000000000
    return answer
```

구글링 함

- 이해한 부분
  
  1. 배열 A의 모든 원소 <= M이라 `check[]` 크기를 M+1로
  
  2. `caterpillar method`에서는 `front`와 `back` 위치를 조절해서 개수를 구함
  
  3. 추가되는 순서쌍 개수는 front - back + 1



- 이해 못 한 부분
  
  1. check 배열 구조..



디버그 하면

| answer | front | A[front] | back | A[back] |
| ------ | ----- | -------- | ---- | ------- |
| 3      | 1     | 4        |      |         |
| 6      | 2     | 5        |      |         |
|        |       |          | 0    | 3       |
|        |       |          | 1    | 4       |
|        |       |          | 2    | 5       |
| 7      | 3     | 5        |      |         |
| 9      | 4     | 2        |      |         |
|        |       |          | 4    | 2       |



check[]

| 0   | 1   | 2   | 3      | 4      | 5           | 6   |
| --- | --- |:---:|:------:|:------:|:-----------:| --- |
|     |     | T   | T -> F | T -> F | T -> F -> T |     |



<br/>



### 다른 코드

```python
def solution(M, A):
    appearance = [False] * (M + 1)
    N = len(A)
    front = 0 
    slices = 0
    for back in range(N):
        while front < N and appearance[A[front]] == 0:
            appearance[A[front]] += 1
            slices += front - back +1
            front += 1
        appearance[A[back]] -= 1
        if slices >= 1000000000: return 1000000000
    return slices
```

`appearance[A[back]] -= 1` 이 부분이 다름

<br/>

### 느낀점

하 진짜 너무 어렵다 ^ㅡㅜ,,
