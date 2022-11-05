---

layout: post

title: Codility CountNonDivisible

---

## Lesson 11. Sieve of Eratosthenes - CountNonDivisible

### 문제

You are given an array A consisting of N integers.

For each number A[i] such that 0 ≤ i < N, we want to count the number of elements of the array that are not the divisors of A[i]. We say that these elements are non-divisors.

For example, consider integer N = 5 and array A such that:

A[0] = 3
 A[1] = 1
 A[2] = 2
 A[3] = 3
 A[4] = 6

For the following elements:

> - A[0] = 3, the non-divisors are: 2, 6,
> - A[1] = 1, the non-divisors are: 3, 2, 3, 6,
> - A[2] = 2, the non-divisors are: 3, 3, 6,
> - A[3] = 3, the non-divisors are: 2, 6,
> - A[4] = 6, there aren't any non-divisors.

Write a function:

> def solution(A)

that, given an array A consisting of N integers, returns a sequence of integers representing the amount of non-divisors.

Result array should be returned as an array of integers.

For example, given:

A[0] = 3
 A[1] = 1
 A[2] = 2
 A[3] = 3
 A[4] = 6

the function should return [2, 4, 3, 2, 0], as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..50,000];
> - each element of array A is an integer within the range [1..2 * N].

<br/>

### 상황 분석

배열 중 약수가 아닌 값의 개수를 return하면 된다

<br/>

### 알고리즘

```python
def solution(N):
    number = []
    for i in range(1, int(math.sqrt(N))):
        if N % i == 0:
            number.append(i)
            if N / i != i:
                number.append(N/i)
    return number
```

해당 알고리즘은 약수를 구할 때 시간복잡도를 줄이기 위해 제곱근을 사용한 예시이다. N이 100이라고 가정할 때 `math.sqrt(N)`은 10이다. 100의 약수 중 10이하의 숫자는 [1, 2, 4, 5, 10]이다. 이를 i로 하나씩 나누면

| i   | N / i  | 값   |
| --- | ------ | --- |
| 1   | 100/1  | 100 |
| 2   | 100/2  | 50  |
| 4   | 100/4  | 25  |
| 5   | 100/5  | 50  |
| 10  | 100/10 | 10  |

따라서 100의 약수는 [1, 2, 4, 5, 10, 20, 25, 50, 100] 이다.

<br/>

### 코드 작성

```python
def solution(A):

    result = [0] * len(A)
    arr = [0] * (2 * len(A) + 1)

    for i in A:
        arr[i] += 1
        # print(arr)    

    for i in range(len(A)):
        count = 0
        for j in range(1,int(A[i]**(1/2))+1):
            if A[i] % j == 0:
                count += arr[j]
                if A[i] / j != j:
                    count += arr[int(A[i]/j)]

        result[i] = len(A) - count
        # print(result)
    return result
```

그래서 다음과 같이 코드를 작성하여 풀었는데 77% timeout error가 발생했다 ^^....

1. large, random numbers, length = ~30,000: running time: 0.424 sec., time limit: 0.384 sec.

2. large, all the same values, length = 50,000: running time: 1.464 sec., time limit: 0.304 sec.



<br/>

### 다른 코드

```python
 def solution(A):

    N = len(A)
    #배열 A의 원소갯수를 담을 list, index가 원소가 됨
    element = [0]*(2*N+1)

    #배열 A에서 각 원소갯수를 카운트
    for a in A:
        element[a] += 1

    answer = [0]*N

    #A를 for문돌다가 똑같은 요소인 경우 저장해서 바로 사용하기 위함
    saved = [-1] * (2*N+1)
    for i in range(N):
        current = A[i]

        #이전에 똑같은 원소가 나온 경우
        if saved[current] != -1:
            answer[i] = saved[current]
        else :
            count = 0

            #현재 원소의 약수를 구하기 위하며, 만약 6인 경우 1,2,3,6이니 2까지만 for문을 돌고 3은 동시에 카운트
            for j in range(1, int(current**0.5) + 1):
                if current%j == 0:
                    count += element[j]

                    # current가 4인경우 j가 2인 상황을 위함. 1,2,3,6일 때 2와 3처럼 대칭인 부분이 없기때문
                    if j != current//j:
                        count += element[current//j]

            # 양수를 구한 후 전체 값을 빼면, 원하는 답이 나옴
            answer[i] = N - count
            # 원소를 저장했다가 중복되는 경우 똑같은 값으로 사용하기 위함
            saved[current] = answer[i]

    return answer
```

비슷한데 이전에 똑같은 원소가 나온 경우 저 부분이 다르다. 여기서 예외처리를 한 번 해줘야 시간 제한에 안 걸리는걸까?

<br/>

```python
def solution(A):
    cnt = dict()
    for a in A:
        cnt[a] = cnt.get(a, 0) + 1

    nondiv = dict()

    ret = []
    for a in A:
        # 이미 계산한 적이 있는 경우 계산없이 바로 저장
        if nondiv.get(a, -1) != -1:
            ret.append(nondiv[a])
            continue
        total = len(A)
        m = int(a ** 0.5) + 1
        for i in range(1, m):
            if a % i == 0:
                # a가 제곱수라면 i == a//i 가 같아지는 지점이 생긴다
                # 중복으로 세지 않게 예외처리한다
                if i == a // i:
                    total -= cnt.get(i, 0)
                else:
                    total -= cnt.get(i, 0) + cnt.get(a // i, 0)
        ret.append(total)
        # 한번 계산한 답은 딕셔너리에 저장한다
        nondiv[a] = total

    return ret
```

이건 딕셔너리를 활용한 부분인데 뭔가... 맞는 것 같아

<br/>

### 함수 정리

`**` 제곱

<br/>

### 느낀점

이번에는 약수를 구할 때 시간복잡도를 줄이면서 구하는 방법을 알았다! 

<img width="574" alt="스크린샷 2022-11-05 오후 8 55 05" src="https://user-images.githubusercontent.com/72901045/200118604-05df935c-5615-423c-9a36-52af43b132c4.png">

물론... 타임아웃 에러가 떴지만 ^ㅡㅜ,,
