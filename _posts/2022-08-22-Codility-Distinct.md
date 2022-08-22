---

layout: post

title: Codility Distinct

---

## Lesson 6. Sorting - Distinct

### 문제

Write a function

> def solution(A)

that, given an array A consisting of N integers, returns the number of distinct values in array A.

For example, given array A consisting of six elements such that:

A[0] = 2 A[1] = 1 A[2] = 1
 A[3] = 2 A[4] = 3 A[5] = 1

the function should return 3, because there are 3 distinct values appearing in array A, namely 1, 2 and 3.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - each element of array A is an integer within the range [−1,000,000..1,000,000].

<br/>

### 코드 작성

```python
def solution(A):
    ans = len(set(A))
    return ans
```

n분컷이라는 거 공감 못했는데 아니... 이렇게 빨리 끝나도 되는건가? 어쨌거나 고유한 값의 개수를 return하면 되는 아주아주 쉬운 문제였다. 중복을 걸러주는 집합 개념을 사용했다. 처음에 문제 잘 못 읽어서 값 그대로 나와야되는 줄.. 시간 복잡도는 O(N*log(N)) or O(N)

<br/>

### 다른 코드

```python
def solution(A):
    tmp = {}
    for num in A:
        tmp[num] = 1
    return len(tmp)
```

딕셔너리를 활용한 코드. 딕셔너리 키로 값을 조회, 지정, 길이 조회 및 삭제 모두 O(1)의 시간복잡도를 갖는다고 한다!

<br/>

```python
def solution(A):
    if len(A) == 0:
        return 0
    A.sort()
    cnt = 1 #두 번째 원소부터 비교할 것
    for i in range(1, len(A)):
        if A[i] != A[i-1]: #직전 값과 동일하지 않을 경우에만
            cnt += 1 #카운트하겠다
    return cnt
```

sorting이라는 lesson에 충실한 코드도 찾아봤다. 재밌네...

<br/>

### 느낀점

당당하게 [링크](https://app.codility.com/demo/results/trainingZ32TQF-HA8/) 공유합니다,, 평소에도 이렇게 간단한 문제만 나왔으면 좋겠지만~~~




