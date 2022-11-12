---

layout: post

title: Codility Ladder

summary: pow(), &, <<

---

## Lesson 13. Fibonacci numbers - Ladder

### 문제

You have to climb up a ladder. The ladder has exactly N rungs, numbered from 1 to N. With each step, you can ascend by one or two rungs. More precisely:

> - with your first step you can stand on rung 1 or 2,
> - if you are on rung K, you can move to rungs K + 1 or K + 2,
> - finally you have to stand on rung N.

Your task is to count the number of different ways of climbing to the top of the ladder.

For example, given N = 4, you have five different ways of climbing, ascending by:

> - 1, 1, 1 and 1 rung,
> - 1, 1 and 2 rungs,
> - 1, 2 and 1 rung,
> - 2, 1 and 1 rungs, and
> - 2 and 2 rungs.

Given N = 5, you have eight different ways of climbing, ascending by:

> - 1, 1, 1, 1 and 1 rung,
> - 1, 1, 1 and 2 rungs,
> - 1, 1, 2 and 1 rung,
> - 1, 2, 1 and 1 rung,
> - 1, 2 and 2 rungs,
> - 2, 1, 1 and 1 rungs,
> - 2, 1 and 2 rungs, and
> - 2, 2 and 1 rung.

The number of different ways can be very large, so it is sufficient to return the result modulo 2P, for a given integer P.

Write a function:

> def solution(A, B)

that, given two non-empty arrays A and B of L integers, returns an array consisting of L integers specifying the consecutive answers; position I should contain the number of different ways of climbing the ladder with A[I] rungs modulo 2B[I].

For example, given L = 5 and:

A[0] = 4 B[0] = 3
 A[1] = 4 B[1] = 2
 A[2] = 5 B[2] = 4
 A[3] = 5 B[3] = 3
 A[4] = 1 B[4] = 1

the function should return the sequence [5, 1, 8, 0, 1], as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - L is an integer within the range [1..50,000];
> - each element of array A is an integer within the range [1..L];
> - each element of array B is an integer within the range [1..30].



<br/>

### 상황 분석

이거 처음에 문제 해석을 이상하게 해서 ㅡㅡ; 파파고 번역 돌려서 제곱이 곱하기로 나와서 대체 왜... 이런 결과가 나오는지 이해하느라 오래걸림...

A[i]에서 나온 결과를 2^B[i]로 나눈 나머지로 return

<br/>

### 코드 작성

```python
def fibonacci(n):
    return fibonacci(n-1) + fibonacci(n-2) if n >= 3 else n

def solution(A, B):
    answer = []
    for i in range(len(A)):
        #fibonacci(A[i])
        answer.append(fibonacci(A[i])  % pow(2, B[i]))
        #print(answer)
    return answer
```

결과는 맞았는데 시간 복잡도에서 털렸다 ㅋㅋ~ 몇 퍼 인지 기억도 안남

<br/>

```python
def solution(A, B):
    answer = []
    max_value = pow(2, 30)
    fib = [1] * (len(A) + 1)

    for i in range(2, len(A)+1):
        fib[i] = (fib[i-1] + fib[i-2]) % max_value

    #print(fib)

    for i in range(len(A)):
        answer.append(fib[A[i]] % pow(2, B[i]))
        #print(answer)
    return answer
```

뭐가 문제인지 생각해봤는데 

1. `def fibonacci(n)`으로 함수를 따로 만들어서 n이 올때마다 계산해서 느린건가?하고 메인에 다 넣어버림. 애초에 피보나치 수열을 만들어놓고 저기에서 i를 가져다쓰도록 했다

2. 그리고 [이 블로그](https://sustainable-dev.tistory.com/40)를 참고해서 `max_value` 값을 추가해줬다. 이게 핵심인듯!!

<br/>

### 다른 코드

```python
def fib_numbers(num):
    """Generate num Fibonacci numbers as dict."""
    first, second = 0, 1
    result = {0: first, 1: second}
    for i in range(2, num + 1):
        result[i] = first + second
        first, second = second, first + second
    return result

def get_modulo(num, div):
    """Compute modulus of num % div bitwise."""
    return num & (div - 1)

def solution(ladders, exponents):
    """Solution method implementation."""
    # initializations and pre-computations
    result = []
    fib_seq = fib_numbers(max(ladders) + 1)
    # iterate through the list
    for i, _ in enumerate(ladders):
        dividend = fib_seq[ladders[i] + 1]
        divisor = 2 ** exponents[i]
        result.append(get_modulo(dividend, divisor))
    
    return result
```

`def fib_numbers(num)` 이걸 따로 했구나 이건 별로 상관 없는건가..? ㅋㅋㅋ



<br/>

```python
def solution(A, B):
    fibo = [0] * 50001
    mod = pow(2, 30)
    fibo[1] = 1
    fibo[2] = 2
    for i in range(3, 50001):
        fibo[i] = (fibo[i - 1] % mod + fibo[i - 2] % mod) % mod
    
    ret = []
    for i in range(len(A)):
        ret.append(fibo[A[i]] % pow(2, B[i]))

    return ret
```

내가 한 `max_value` 이걸 자주 사용했다 ㅇ.ㅇbb

<br/>

```python
def solution(A, B):
    L = len(A)
    fib = [0] * (L + 2)
    fib[1] = 1
    for i in range(2, L+2):
        fib[i] = fib[i-1] + fib[i-2]
    
    sol = []
    for i in range(L):
        sol.append( fib[A[i] + 1] & ((1 << B[i]) - 1) )
    return sol
```

`<<` 연산자 사용

<br/>

### 함수 정리

`pow(x, y)` (=x^y =`x**y`)

`&` and 연산

`<< n` n비트만큼 왼쪽으로 비트를 이동

<br/>

### 느낀점

<img width="574" alt="스크린샷 2022-11-12 오후 9 33 41" src="https://user-images.githubusercontent.com/72901045/201474118-e9ba94d5-3c3e-4f35-b18e-4bf218c76724.png">

O(L)은 len()이라는 뜻일까? ㅋㅋ 어제 푼 것 보다는 정말 정말 정말 괜찮았다~
