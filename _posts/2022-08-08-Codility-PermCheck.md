---

layout: post

title: Codility PermCheck

summary: sort(), list(), set()

---

## Lesson 4. Counting Elements - PermCheck

### 문제

A non-empty array A consisting of N integers is given.

A *permutation* is a sequence containing each element from 1 to N once, and only once.

For example, array A such that:

A[0] = 4
 A[1] = 1
 A[2] = 3
 A[3] = 2

is a permutation, but array A such that:

A[0] = 4
 A[1] = 1
 A[2] = 3

is not a permutation, because value 2 is missing.

The goal is to check whether array A is a permutation.

Write a function:

> def solution(A)

that, given an array A, returns 1 if array A is a permutation and 0 if it is not.

For example, given array A such that:

A[0] = 4
 A[1] = 1
 A[2] = 3
 A[3] = 2

the function should return 1.

Given array A such that:

A[0] = 4
 A[1] = 1
 A[2] = 3

the function should return 0.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [1..1,000,000,000].

<br/>

### 상황 분석

비어있으면 0을 반환하라는 소리구나,,싶었는데 이걸 전문적으로 부르는 말이 있었다. 순열 ㅋㅋㅋ 어쨌거나 순열이면 1을 반환하고 아니라면 0을 반환하면 되는 문제이다.

<br/>

### 코드 작성

```python
def solution(A):

    ans = 0

    for i in range(len(A)+1):
        ans += i

    if sum(A) == ans:
        return 1
    else:
        return 0
```

어딘가.. 싸함이 느껴지시나요? ^ㅡ^ 완성도 75.. `The following issues have been detected: wrong answers. For example, for the input [1, 4, 1] the solution returned a wrong answer (got 1 expected 0).` 라는 조언을 해줬다. 매우 맞는 말이다..! ㅋㅋㅠ

<br/>

```python
def solution(A):
    ans = set()
    for i in range(len(A)):
        ans.add(A[i])
        if A == ans:
            return 1
        else:
            return 0
```

사실 처음에는 이런 식으로 풀었는데 저 ans가 계속.. 추가가 되지 않았다. for문으로 하면... 되야 하는 거 아닌가? 진짜 모르겠다........../

<br/>

### 다른 코드

```python
def solution(A):
    B = list(set(A)) # A의 중복제거 후 리스트로 변환
    if len(B) != len(A): # A의 크기 != B의 크기 (중복 포함되어 있다는 이야기)
        return 0
    B.sort() # B 순서대로 나열(오름차순)
    count = 1
    for i in B:
        if count == i:
            result =1
            count +=1
        else :
            result = 0
            break
    return result
```

개수로 판단하는거 꽤 괜찮네.. 저번에 배운 set 개념 나도 활용하려 했었는데 ^ㅡ^..!! 중복 포함되어 있으면 바로 0으로 return하는 것도 실용적이다.

<br/>

### 함수 정리

`list()` 리스트로 변환 가능한 다른 자료형을 리스트로 바꿔줄 수 있음

<br/>

### 느낀점

알고리즘 문제 오랜만에 푸니 낯설다 ㅎ.. 좀 숙달되면 며칠 안 해도 괜찮은데 시작하는 단계에서 안 하니까 바로 티가 나구나,, 구름 알고리즘 문제도 있으니까 매일 코딜리티 문제 풀어...야 겠다^ㅡ^....!!!!
