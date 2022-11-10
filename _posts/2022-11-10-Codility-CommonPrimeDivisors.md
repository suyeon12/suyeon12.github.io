---

layout: post

title: Codility CommonPrimeDivisors

summary: while, //, zip

---

## Lesson 12. Euclidean algorithm - CommonPrimeDivisors

### 문제

A *prime* is a positive integer X that has exactly two distinct divisors: 1 and X. The first few prime integers are 2, 3, 5, 7, 11 and 13.

A prime D is called a *prime divisor* of a positive integer P if there exists a positive integer K such that D * K = P. For example, 2 and 5 are prime divisors of 20.

You are given two positive integers N and M. The goal is to check whether the sets of prime divisors of integers N and M are exactly the same.

For example, given:

> - N = 15 and M = 75, the prime divisors are the same: {3, 5};
> - N = 10 and M = 30, the prime divisors aren't the same: {2, 5} is not equal to {2, 3, 5};
> - N = 9 and M = 5, the prime divisors aren't the same: {3} is not equal to {5}.

Write a function:

> def solution(A, B)

that, given two non-empty arrays A and B of Z integers, returns the number of positions K for which the prime divisors of A[K] and B[K] are exactly the same.

For example, given:

A[0] = 15 B[0] = 75
 A[1] = 10 B[1] = 30
 A[2] = 3 B[2] = 5

the function should return 1, because only one pair (15, 75) has the same set of prime divisors.

Write an ****efficient**** algorithm for the following assumptions:

> - Z is an integer within the range [1..6,000];
> - each element of arrays A and B is an integer within the range [1..2,147,483,647].

<br/>

### 상황 분석

A[i]와 B[i]의 약수 중 소수가 같은 세트의 개수를 return하면 된다.

<br/>

### 알고리즘

1. M과 N의 gcd를 구한다

2. M / gcd의 값을 구한다.
   
   - 만약 gcd가 1이라면 통과
   
   - 1이 아니라면 (M / gcd) / gcd(gcd, M / gcd)을 계산한다.

3. N/ gcd의 값을 구한다.
   
   - 만약 gcd가 1이라면 통과
   
   - 1이 아니라면 (N / gcd) / gcd(gcd, N / gcd)을 계산한다.



<br/>

### 코드 작성

```python
def getGCD(a, b):
    if b == 0:
        return a
    else:
        return getGCD(b, a%b)

def solution(A, B):
    count = 0
    for i in range(len(A)):
        a = A[i]
        b = B[i]
        gcd = getGCD(a,b)
        # print(gcd)

        gcd_a = 0
        gcd_b = 0

        while (gcd_a, gcd_b) != (1, 1):
            gcd_a = getGCD(a, gcd)
            gcd_b = getGCD(b, gcd)
            a //= gcd_a
            b //= gcd_b
        #print(a)
        #print(b)
        #print(gcd_a)
        #print(gcd_b)
        
        if a == 1 and b == 1:
            count += 1
        #print(count)
    return count
```

가장 마지막 else count = 0 넣었다가 계속 값이 안 맞아서 시간이 오래 걸렸다 ^^......... 시간복잡도는 O(Z * log(max(A) + max(B))**2) 이렇게 걸리는데 뭔가.. 복잡하다?!



<br/>

### 다른 코드

```python
def getGCD(a, b):
    if a < b:
        a, b = b, a
    if a % b == 0:
        return b
    return getGCD(b, a % b)

def solution(A, B):
    ret = 0
    for i in range(len(A)):
        # a와 b가 공유하는 소수만 이루어여있다면 True, 아니면 False
        flag = True
        
        a, b = A[i], B[i]
        G = getGCD(a, b)

        while a != 1:
            g = getGCD(a, G)
            # a가 1이 아닌데 이미 a와 gcd가 서로소면
            # 공유하지 않는 소수가 존재한다
            if g == 1:
                flag = False
                break
            a //= g

        # 이미 a가 gcd와 공유하지 않는 소수가 존재하면 False
        # b는 더이상 관찰할 필요가 없다.
        if not flag:
            continue
        
        while b != 1:
            g = getGCD(b, G)
            # b가 1이 아닌데 이미 b와 gcd가 서로소면
            # 공유하지 않는 소수가 존재한다
            if g == 1:
                flag = False
                break
            b //= g
        
        # a와 b가 공유하는 소수만으로 이루어져있다면 답 개수 증가
        if flag:
            ret += 1

    return ret
```

T/F로 플래그를 표시해주는게 신기하군아



<br/>

```python
def solution(A, B):
    # write your code in Python 3.6
    count = 0
    for (a, b) in zip(A, B):
        ab_gcd = gcd(max(a,b), min(a,b))

        a /= ab_gcd 
        b /= ab_gcd 

        a_gcd , b_gcd = ab_gcd, ab_gcd

        while a_gcd != 1: 
            a_gcd = gcd(a, a_gcd)
            a /= a_gcd
        
        while b_gcd != 1:
            b_gcd = gcd(b, b_gcd)
            b /= b_gcd 


        if a == 1 and b == 1 :
            count += 1 

    return count 


def gcd(N, M):
    if M == 0:
        return N 
    return gcd(M, N % M)
```

`ab_gcd`라고 이름 붙인거 좋다

<br/>

### 함수 정리

`while (gcd_a, gcd_b) != (1, 1)`

`//` 몫

`zip()` 길이가 같은 리스트 등의 요소를 묶어주는 함수



<br/>

### 느낀점

<img width="609" alt="스크린샷 2022-11-10 오후 8 16 13" src="https://user-images.githubusercontent.com/72901045/201077411-05718732-c715-4d42-a1ee-906e73c52a28.png">

진짜 오랜만에 풀면서 필기한 사진 올리는듯.. ㅋㅋㅋ 암튼 만족스럽군요,,
