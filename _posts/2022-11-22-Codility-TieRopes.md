---

layout: post

title: Codility TieRopes

summary: enumerate()

---

## Lesson 16. Greedy algorithms - TieRopes

### 문제

There are N ropes numbered from 0 to N − 1, whose lengths are given in an array A, lying on the floor in a line. For each I (0 ≤ I < N), the length of rope I on the line is A[I].

We say that two ropes I and I + 1 are *adjacent*. Two adjacent ropes can be tied together with a knot, and the length of the tied rope is the sum of lengths of both ropes. The resulting new rope can then be tied again.

For a given integer K, the goal is to tie the ropes in such a way that the number of ropes whose length is greater than or equal to K is maximal.

For example, consider K = 4 and array A such that:

A[0] = 1
 A[1] = 2
 A[2] = 3
 A[3] = 4
 A[4] = 1
 A[5] = 1
 A[6] = 3

The ropes are shown in the figure below.

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/tie_ropes/static/images/auto/f13a51b17fba1ea9b8ea7fd37006f767.png)

We can tie:

> - rope 1 with rope 2 to produce a rope of length A[1] + A[2] = 5;
> - rope 4 with rope 5 with rope 6 to produce a rope of length A[4] + A[5] + A[6] = 5.

After that, there will be three ropes whose lengths are greater than or equal to K = 4. It is not possible to produce four such ropes.

Write a function:

> def solution(K, A)

that, given an integer K and a non-empty array A of N integers, returns the maximum number of ropes of length greater than or equal to K that can be created.

For example, given K = 4 and array A such that:

A[0] = 1
 A[1] = 2
 A[2] = 3
 A[3] = 4
 A[4] = 1
 A[5] = 1
 A[6] = 3

the function should return 3, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - K is an integer within the range [1..1,000,000,000];
> - each element of array A is an integer within the range [1..1,000,000,000].



<br/>

### 코드 작성

```python
def solution(K, A):
    count = 0
    length = 0
    for i in A:
        length += i
        if length >= K:
            count += 1
            length = 0
    return count
```

반복문동안 length 길이를 키우고 만약 length가 K보다 클 경우 count를 1씩 더하고 length를 초기화해준다

<br/>

### 다른 코드

```python
def solution(K, A):
    if len(A)==1 :
        if A[0]<K: return 0
        else: return 1

    ropes = []
    prev = None
    for i, a in enumerate(A):

        if a>=K:

            ropes.append(i)
            prev = None

        else:

            if prev and prev+a>=K:
                ropes.append(i)
                prev = None
            elif prev:
                prev += a
            else: 
                prev = a


    # write your code in Python 3.6
    return len(ropes)
```

enumerate와 배열 사용

<br/>

### 함수 정리

`enumerate()` 인자로 넘어온 목록을 기준으로 인덱스와 원소를 차례대로 접근하게 해주는 반복자 객체를 반환해주는 함수. [다음 블로그](https://www.daleseo.com/python-enumerate/)에서 자세한 내용을 확인할 수 있다



<br/>

### 느낀점

greedy 문제를 다 풀긴 했는데 아직 잘 모르겠어 ^ㅡ^,, 다음 DP하면 코딜리티도 끝이네 너무 충격적이야..
