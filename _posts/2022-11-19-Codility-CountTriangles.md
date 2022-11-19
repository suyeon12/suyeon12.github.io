---

layout: post

title: Codility CountTriangles

---

## Lesson 15. Caterpillar method - CountTriangles

### 문제

An array A consisting of N integers is given. A triplet (P, Q, R) is *triangular* if it is possible to build a triangle with sides of lengths A[P], A[Q] and A[R]. In other words, triplet (P, Q, R) is triangular if 0 ≤ P < Q < R < N and:

> - A[P] + A[Q] > A[R],
> - A[Q] + A[R] > A[P],
> - A[R] + A[P] > A[Q].

For example, consider array A such that:

A[0] = 10 A[1] = 2 A[2] = 5
 A[3] = 1 A[4] = 8 A[5] = 12

There are four triangular triplets that can be constructed from elements of this array, namely (0, 2, 4), (0, 2, 5), (0, 4, 5), and (2, 4, 5).

Write a function:

> def solution(A)

that, given an array A consisting of N integers, returns the number of triangular triplets in this array.

For example, given array A such that:

A[0] = 10 A[1] = 2 A[2] = 5
 A[3] = 1 A[4] = 8 A[5] = 12

the function should return 4, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..1,000];
> - each element of array A is an integer within the range [1..1,000,000,000].



<br/>

### 코드 작성

```python
def solution(A):
    A.sort()
    answer = 0
    for x in range(len(A)):
        z = x + 2
        for y in range(x+1, len(A)):
            while z < len(A) and A[x] + A[y] > A[z]:
                z += 1
            answer += z - y - 1
            #print("z",z)
            #print("y",y)
    return answer
```

읽기 자료를 참고해서 코드를 풀긴 했는데 에효ㅡ,, 시간복잡도는 n**2인데 Caterpillar 이거 안 쓰면 3제곱이라고 한다..

- 이해한 부분
  
  1. 배열을 오름차순으로 나열하면 세 조건식에서 첫 번째 조건만 구하면 된다
  
  

- 이해 못 한 부분
  
  1. z - y -1... 왜?

<br/>

### 다른 코드

```python
def solution(edges):
    """Solution method implementation."""
    # initialize vars
    list_len, srt = len(edges), sorted(edges)
    result = 0
    # main loop
    for i in range(list_len - 1, 0, -1):
        # caterpillar (re)defining
        back, front = 0, i - 1
        # caterpillar shrinking loop
        while back < front:
            if srt[back] + srt[front] > srt[i]:
                result += front - back
                front -= 1
            else:
                back += 1
    return result
```

이게 어제 푼 거랑 좀 더 비슷해보인다



<br/>

### 느낀점

Caterpillar 너무 싫어요,, ㅠ


