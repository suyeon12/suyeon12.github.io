---

layout: post

title: Codility CountSemiprimes

---

## Lesson 11. Sieve of Eratosthenes - CountSemiprimes

### 문제

A *prime* is a positive integer X that has exactly two distinct divisors: 1 and X. The first few prime integers are 2, 3, 5, 7, 11 and 13.

A *semiprime* is a natural number that is the product of two (not necessarily distinct) prime numbers. The first few semiprimes are 4, 6, 9, 10, 14, 15, 21, 22, 25, 26.

You are given two non-empty arrays P and Q, each consisting of M integers. These arrays represent queries about the number of semiprimes within specified ranges.

Query K requires you to find the number of semiprimes within the range (P[K], Q[K]), where 1 ≤ P[K] ≤ Q[K] ≤ N.

For example, consider an integer N = 26 and arrays P, Q such that:

P[0] = 1 Q[0] = 26
 P[1] = 4 Q[1] = 10
 P[2] = 16 Q[2] = 20

The number of semiprimes within each of these ranges is as follows:

> - (1, 26) is 10,
> - (4, 10) is 4,
> - (16, 20) is 0.

Write a function:

> def solution(N, P, Q)

that, given an integer N and two non-empty arrays P and Q consisting of M integers, returns an array consisting of M elements specifying the consecutive answers to all the queries.

For example, given an integer N = 26 and arrays P, Q such that:

P[0] = 1 Q[0] = 26
 P[1] = 4 Q[1] = 10
 P[2] = 16 Q[2] = 20

the function should return the values [10, 4, 0], as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..50,000];
> - M is an integer within the range [1..30,000];
> - each element of arrays P and Q is an integer within the range [1..N];
> - P[i] ≤ Q[i].



<br/>

### 상황 분석

반소수라는 개념이 등장한다. 이는 두 소수의 곱을 의미한다. 여기서 P[i]와 Q[i] 사이의 반소수의 개수를 배열로 리턴한다.

<br/>

### 알고리즘

```python
def solution(N):
    prime = [True] * (N+1)
    prime[0] = False
    prime[1] = False
    primenumber = []

    for i in range(2, int(math.sqrt(N))+1):
        if prime[i] == True:
            j = 2
            
            while (i*j) <= N:
                prime[i*j] = False
                j += 1
    
    for i in range(N):
        if prime[i] == True:
            primenumber.append(i)
    
    return primenumber
```

해당 알고리즘은 에라토스테네스의 체이다. 먼저 2부터 N의 제곱근까지 범위의 배열을 만든 후 i의 배수를 지운다. 이후 i 다음 작은 수의 배수를 배열에서 지운다. 이를 N의 제곱근까지 반복한다.

<br/>

### 코드 작성

```python
import math

def solution(N, P, Q):
    prime = [True] * (N+1)
    prime[0] = False
    prime[1] = False
    primenumber = []
    semiprime = [0] * (N+1)
    result = []
  
    for i in range(2, int(math.sqrt(N))+1):
        if prime[i] == True:
            j = 2
            
            while (i*j) <= N:
                prime[i*j] = False
                j += 1
    
    for i in range(N):
        if prime[i] == True:
            primenumber.append(i)
    # print(primenumber)
    
    for i in primenumber:
        for j in primenumber:
            if i * j > N:
                break
            semiprime[i*j] = 1
    # print(semiprime)

    for i in range(1, len(semiprime)):
        semiprime[i] += semiprime[i - 1]
    # print(semiprime)

    for i in range(len(P)):
        result.append(semiprime[Q[i]]-semiprime[P[i]-1])
    
    return result
```

먼저 소수를 구한 후(`primenumber[]`) 반소수의 개수를 구한다(`semiprime[]`). 범위가 P[i]가 포함되므로 마지막 semiprime에서 1을 빼주어야 한다. O(N * log(log(N)) + M)의 시간 복잡도로 통과하였다.

참고로 주석처리한 print문의 결과는 다음과 같다.

```
[2, 3, 5, 7, 11, 13, 17, 19, 23]
[0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1]
[0, 0, 0, 0, 1, 1, 2, 2, 2, 3, 4, 4, 4, 4, 5, 6, 6, 6, 6, 6, 6, 7, 8, 8, 8, 9, 10]
```

<br/>

### 다른 코드

```python
def is_semi_prime(num):
    """Check for semi-primes."""
    cnt = 0
    var_x = num
    # successive division of number
    # until 2 factors have been found or 
    # until we ran out of factors
    for i in range(2, int(var_x ** 0.5) + 1):
        while var_x % i == 0:
            var_x //= i
            cnt += 1
        if cnt >= 2:
            break
    # if our number's still gt 1, increment factor counter
    cnt += 1 if var_x > 1 else 0
    # counter is 2: number is semiprime
    return cnt == 2

def solution(num, beg, end):
    """Solution method implementation."""
    
    # initialization
    list_len = len(beg)
    start = min(beg)
    finish = num
    dic = {}
    result = {}
    # build lookup dictionary
    for i in range(start, finish + 1):
        old = 0 if i == start else dic[i - 1]
        dic[i] = old + 1 if is_semi_prime(i) else old
    # iterate through queries
    for i in range(list_len):
        result[i] = dic.get(end[i], 0) - dic.get(beg[i] - 1, 0)
    # build result list and return it
    return [result[i] for i in range(list_len)]
```



비슷한데 여긴 딕셔너리를 이용해서 풀었다



<br/>

### 느낀점

<img width="574" alt="스크린샷 2022-11-07 오전 11 08 05" src="https://user-images.githubusercontent.com/72901045/200212163-6e805897-2fb1-469b-8591-8058a3d9a165.png">

핫핫 뿌듯하군아,, 이번 챕터는 정말 알고리즘 문제같다 여러모로!!
