---
layout: post

title: Codility CyclicRotation

summary: list[], pop(), insert()

tags: array

---

## Lesson 2. Arrays - CyclicRotation

### 문제

An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

The goal is to rotate array A K times; that is, each element of A will be shifted to the right K times.

Write a function:

> def solution(A, K)

For example, given

A = [3, 8, 9, 7, 6]
 K = 3

the function should return [9, 7, 6, 3, 8]. Three rotations were made:

[3, 8, 9, 7, 6] -> [6, 3, 8, 9, 7]
 [6, 3, 8, 9, 7] -> [7, 6, 3, 8, 9]
 [7, 6, 3, 8, 9] -> [9, 7, 6, 3, 8]

For another example, given

A = [0, 0, 0]
 K = 1

the function should return [0, 0, 0]

Given

A = [1, 2, 3, 4]
 K = 4

the function should return [1, 2, 3, 4]

Assume that:

> - N and K are integers within the range [0..100];
> - each element of array A is an integer within the range [−1,000..1,000].

In your solution, focus on ****correctness****. The performance of your solution will not be the focus of the assessment.

An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

The goal is to rotate array A K times; that is, each element of A will be shifted to the right K times.

Write a function:

> def solution(A, K)

that, given an array A consisting of N integers and an integer K, returns the array A rotated K times.

For example, given

A = [3, 8, 9, 7, 6]
 K = 3

the function should return [9, 7, 6, 3, 8]. Three rotations were made:

[3, 8, 9, 7, 6] -> [6, 3, 8, 9, 7]
 [6, 3, 8, 9, 7] -> [7, 6, 3, 8, 9]
 [7, 6, 3, 8, 9] -> [9, 7, 6, 3, 8]

For another example, given

A = [0, 0, 0]
 K = 1

the function should return [0, 0, 0]

Given

A = [1, 2, 3, 4]
 K = 4

the function should return [1, 2, 3, 4]

Assume that:

> - N and K are integers within the range [0..100];
> - each element of array A is an integer within the range [−1,000..1,000].

In your solution, focus on ****correctness****. The performance of your solution will not be the focus of the assessment.

<br/>

### 상황 분석

1. K번 같은 상황을 반복한다.

2. j번째가 마지막이라면 가장 마지막 배열의 인덱스는 처음으로 돌아가고 그렇지 않은 경우에는 j+1이 j가 된다.

```python
    for i in K:
        for j in A[j]:
            if j = A[j]:
                A[j] = A[1]
            else:
                A[j+1] = A[j]
```

...로 하다가 실패했다. 솔직히 이건 저 `for j in A[j]`부터 그냥 틀린 코드인데 파이썬에서는 절대!!! 저렇게 안 쓴다. 그냥 A라고 쓰지 ㅎ.... (아니 그냥 첫 줄부터 틀렸다 푸하...) 생각해보고 코딩하면서 이것저것 바꿔보다가 답이 없길래 파이썬 강의 보면서 필기한 부분 중 for, if, list 부분을 한 번 더 읽어봤다. 그중 `pop()` 함수를 발견하게 되는데...

<br/>

### 코드 작성

```python
def solution(A, K):

    for i in range(K):
        last = A.pop()
        A.insert(0, last)
    return A
```

이번엔 정말 짧고 테스트 해 본 결과 맞아서 필기한 거 보긴 했으나 만족했는데 결과 페이지를 보니 "RUNTIME ERROR"가 발생했다. 

```
1. 0.036 s OK
2. 0.036 s RUNTIME ERROR,  tested program terminated with exit code 1
stderr:
Traceback (most recent call last):
  File "exec.py", line 145, in <module>
    main()
  File "exec.py", line 107, in main
    result = solution( A, K )
```

이렇게 뜨는데 왜지..? 이유를 모르겠다 ^ㅡㅜ

<br/>

### 다른 코드

```python
def solution(A, K):
    if not (A and K):
        return A

    K = K % len(A)
    return A[-K:] + A[:-K]
```

`not (A and K)`를 풀면 A도 아니거나 K도 아닌 경우인데 그런 일이 뭐가 있을까? 생각해봤는데 잘 떠오르지 않는다..

`%` 는 왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후 그 나머지를 반환하는 연산자인데 위 예시인 ([3,8,9,7,6],3)을 예로 들면 3을 5로 나누니 나머지는 3이 된다. 여기서 -3부터인 [9,7,6]과 -3까지인 [3,8]을 결합하면 같은 결과가 나온다.

<br/>

```python
def solution(A, K):
    count = 0
    temp = 0
    if len(A) == K or len(A) == 0 :
        return A
    elif len(A) != K and len(A) > 0 :
        while count != K :
            temp = A[len(A)-1]
            for i in range(len(A)):
                if i != len(A)-1:
                    A[len(A)-1-i] = A[len(A)-i-2]
                else:
                    A[0] = temp
            count +=1
        return A
```

이건 딱 내가 제일 처음 생각한 알고리즘 그대로라 가지고 왔다. 이런 과정이 필요했구나 ^^..... 어쩐지 안되더라 ㅎ........

<br/>

### 정리

`range(A)`  0부터 A-1까지의 정수 범위를 반환한다. 처음 A까지인줄 알고 `range(1,A)` 썼는데 테스트 결과가 안 맞았다. A가 아니라 A-1까지!!

`pop()`  리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제

`a[start : end : step]` 슬라이싱을 시작할 위치, 끝낼 위치이나 end는 포함하지 않음, 몇 개씩 끊어서 가져올지와 방향을 정함(양수일경우 앞에서부터 0을 시작으로 번호를 뒤쪽으로 증가, 음수일경우 뒤에서부터 -1을 시작으로 번호를 앞쪽으로 감소)

`%` 왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후 그 나머지를 반환하는 연산자

<br/>

### 느낀점

알고리즘 문제를 풀면서 자료구조 + 파이썬에 익숙해지자! 가장 슬플 때는 테스트 오류나는게 아니다.. 아예 실행조차 안 될 때^^.... 함수 사용법도 확실히 습득하자(pop(A) 적어놓고 왜 안되냐고 말한 사람,, `A.pop()`입니다~
