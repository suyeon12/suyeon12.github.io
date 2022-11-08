---

layout: post

title: Codility ChocolatesByNumbers

summary: from math import gcd

---

## Lesson 12. Euclidean algorithm - ChocolatesByNumbers

### 문제

Two positive integers N and M are given. Integer N represents the number of chocolates arranged in a circle, numbered from 0 to N − 1.

You start to eat the chocolates. After eating a chocolate you leave only a wrapper.

You begin with eating chocolate number 0. Then you omit the next M − 1 chocolates or wrappers on the circle, and eat the following one.

More precisely, if you ate chocolate number X, then you will next eat the chocolate with number (X + M) modulo N (remainder of division).

You stop eating when you encounter an empty wrapper.

For example, given integers N = 10 and M = 4. You will eat the following chocolates: 0, 4, 8, 2, 6.

The goal is to count the number of chocolates that you will eat, following the above rules.

Write a function:

> def solution(N, M)

that, given two positive integers N and M, returns the number of chocolates that you will eat.

For example, given integers N = 10 and M = 4. the function should return 5, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N and M are integers within the range [1..1,000,000,000].



<br/>

### 알고리즘

```python
def def(a, b):
    if b == 0:
        return a
    else:
        return def(b, a%b)
```

해당 알고리즘은 유클리드 호제법을 이용해 최대공약수를 구하는 것이다. 이는 a와 b의 최대공약수 = b와 a%b의 나머지의 최대공약수라는 성질을 이용해 나머지가 0이 될때까지 계산을 반복하는 것이다. 아래 그림은 유클리드 호제법을 잘 설명해주는 예시이다.

![GCD-ex2](https://seunghyum.github.io/assets/images/posts/algorithm/GCD-ex.png)

따라서 106과 16의 최대공약수는 2이다.

<br/>

### 코드 작성

```python
def solution(N, M):
    circle = [0] * N
    idx = 0

    while circle[idx] == 0:
        circle[idx] = 1
        idx = (idx + M) % N
    # print(circle)

    return circle.count(1)
```

처음 문제에 충실하게 코드를 구성했는데 시간복잡도 O(N + M)으로 timeout errors가 떠서 50% 성공률을 보였다 ^^.. 이후 챕터가 유클리드 호제법이라 여기에 대해서 알아본 후 코드를 다시 구성했다

<br/>

```python
from math import gcd

def solution(N, M):
    return int(N / gcd(N,M))
```

나는 파이썬에서 제공해주는 내장함수 썼다 ㅋㅎ 한 줄로 끝나다니 진짜 역대급 간단하군아.. 시간복잡도는 O(log(N + M))

만약 

1. N이 M의 배수라면 return 값은 N/M이다.

2. N과 M이 서로소라면 return 값은 N이다.

3. 두 경우가 아닐때(예시의 10,4처럼) return 값은 N을 N과 M의 최대공약수로 나눈 값이다.

=> 따라서 일반화하면 return 값은 N / gcd(N,M)이다.

<br/>

### 다른 코드

```python
def getGCD(a, b):
    if a < b:
        a, b = b, a
    if a % b == 0:
        return b
    return getGCD(b, a % b)

def solution(N, M):
    return N // getGCD(N, M)
```

함수를 따로 분리한 게 다른점! 그리고 내장 모듈을 안 쓰고 코드로 구현하였다

<br/>

### 함수 정리

`math 모듈` 

<br/>

### 느낀점

챕터가 유클리드 호제법이라 이걸 이용할 생각을 했는데 바로 문제를 보고 이걸 떠올릴 수 있을까..? ㅋㅋㅠ 더 연습해야겠다


