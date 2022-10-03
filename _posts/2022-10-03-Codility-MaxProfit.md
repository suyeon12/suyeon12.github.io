---

layout: post

title: Codility MaxProfit

summary: append(), pop()

---

## Lesson 9. Maximum slice problem - MaxProfit

### 문제

An array A consisting of N integers is given. It contains daily prices of a stock share for a period of N consecutive days. If a single share was bought on day P and sold on day Q, where 0 ≤ P ≤ Q < N, then the *profit* of such transaction is equal to A[Q] − A[P], provided that A[Q] ≥ A[P]. Otherwise, the transaction brings *loss* of A[P] − A[Q].

For example, consider the following array A consisting of six elements such that:

A[0] = 23171
 A[1] = 21011
 A[2] = 21123
 A[3] = 21366
 A[4] = 21013
 A[5] = 21367

If a share was bought on day 0 and sold on day 2, a loss of 2048 would occur because A[2] − A[0] = 21123 − 23171 = −2048. If a share was bought on day 4 and sold on day 5, a profit of 354 would occur because A[5] − A[4] = 21367 − 21013 = 354. Maximum possible profit was 356. It would occur if a share was bought on day 1 and sold on day 5.

Write a function,

> def solution(A)

that, given an array A consisting of N integers containing daily prices of a stock share for a period of N consecutive days, returns the maximum possible profit from one transaction during this period. The function should return 0 if it was impossible to gain any profit.

For example, given array A consisting of six elements such that:

A[0] = 23171
 A[1] = 21011
 A[2] = 21123
 A[3] = 21366
 A[4] = 21013
 A[5] = 21367

the function should return 356, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..400,000];
> - each element of array A is an integer within the range [0..200,000].

<br/>

### 알고리즘

큰 거에서 작은 거 뺀다는 max라는 이름에 적합한 문제!!

<br/>

### 코드 작성

```python
def solution(A):
    min = A[0]
    result = 0
    for i in A:
        if min > i:
            min = i
            # print(min)
        if result < i - min:
            result = i - min
            # print(result)
    return result
```

17분만에 풀고 훗... 했는데 88% 성공률이 나왔다 흠;;; 알고보니 `For example, for the input [] the solution terminated unexpectedly.` 예외처리를 안 해서 그런거였음.... 재빠르게 `min`을 수정했다

<br/>

```python
def solution(A):
    if len(A) != 0:
        min = A[0]
    else:
        return 0
    result = 0
      
    for i in A:
        if min > i:
            min = i
            # print(min)
        if result < i - min:
            result = i - min
            # print(result)
    return result
```

근데 대뜸 if 시작하니까 좀 이상하긴 하다

<br/>

### 다른 코드

```python
def solution(A):
    minimum, maxprofit = float('inf'), 0
    for value in A:
        minimum = min(value, minimum)
        maxprofit = max(value - minimum, maxprofit)
    return maxprofit
```

비슷한데 `min(), max()`써서 하니까 훨씬 깔끔한 것 같다. `float('inf')`은 양의 무한대라는 말이구나 ㅋㅋ 비어있을 때 예외처리를 안해주고도 값이 올바르게 나온다는 점이 좋네

<br/>

### 함수 정리

`float('inf')` 양의 무한대  cf. `float('-inf')` 음의 무한대

<br/>

### 느낀점

이런 문제만 계속 나오면 좋겠다
