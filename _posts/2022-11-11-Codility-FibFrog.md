---

layout: post

title: Codility FibFrog

summary: dictionary.get(key, value)

---

## Lesson 13. Fibonacci numbers - FibFrog

### 문제

The Fibonacci sequence is defined using the following recursive formula:

F(0) = 0
 F(1) = 1
 F(M) = F(M - 1) + F(M - 2) if M >= 2

A small frog wants to get to the other side of a river. The frog is initially located at one bank of the river (position −1) and wants to get to the other bank (position N). The frog can jump over any distance F(K), where F(K) is the K-th Fibonacci number. Luckily, there are many leaves on the river, and the frog can jump between the leaves, but only in the direction of the bank at position N.

The leaves on the river are represented in an array A consisting of N integers. Consecutive elements of array A represent consecutive positions from 0 to N − 1 on the river. Array A contains only 0s and/or 1s:

> - 0 represents a position without a leaf;
> - 1 represents a position containing a leaf.

The goal is to count the minimum number of jumps in which the frog can get to the other side of the river (from position −1 to position N). The frog can jump between positions −1 and N (the banks of the river) and every position containing a leaf.

For example, consider array A such that:

A[0] = 0
 A[1] = 0
 A[2] = 0
 A[3] = 1
 A[4] = 1
 A[5] = 0
 A[6] = 1
 A[7] = 0
 A[8] = 0
 A[9] = 0
 A[10] = 0

The frog can make three jumps of length F(5) = 5, F(3) = 2 and F(5) = 5.

Write a function:

> def solution(A)

that, given an array A consisting of N integers, returns the minimum number of jumps by which the frog can get to the other side of the river. If the frog cannot reach the other side of the river, the function should return −1.

For example, given:

A[0] = 0
 A[1] = 0
 A[2] = 0
 A[3] = 1
 A[4] = 1
 A[5] = 0
 A[6] = 1
 A[7] = 0
 A[8] = 0
 A[9] = 0
 A[10] = 0

the function should return 3, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - each element of array A is an integer that can have one of the following values: 0, 1.



<br/>

### 상황 분석

개구리는 A 배열 속 1이 있는 곳에 피보나치 수열만큼 이동할 수 있다. 이때 최소 이동 횟수를 return하면 된다.

<br>

### 코드 작성

```python
def solution(A):
    A.append(1)
    fib = fibonacci(len(A))
    #print(fib)
    dp = {-1: 0}

    for i in range(len(A)):
        if A[i] == 0: continue
        temp_result = []

        for jump in fib:
            if jump > i + 1: break
            if i - jump in dp:
                temp_result.append(dp[i - jump] + 1)
        #print(temp_result)
        if temp_result: 
            dp[i] = min(temp_result)
            #print(dp)
    return dp.get(len(A) - 1, -1)

def fibonacci(n):
    fib = [1, 1]
    while fib[-1] < n:
        fib.append(fib[-1] + fib[-2])
    return fib
```

와 진짜 도저히 모르겠어서 다른 코드 참고함... 딕셔너리를 이용해서 풀었는데 `temp_result.append(dp[i - jump] + 1)` 이 부분이 핵심같다. 시간복잡도는 O(N * log(N))

참고로 프린트 주석 해제하면 이렇게 뜬다

```
[1, 1, 2, 3, 5, 8, 13]
[]
[1]
{-1: 0, 4: 1}
[2]
{-1: 0, 4: 1, 6: 2}
[3]
{-1: 0, 4: 1, 6: 2, 11: 3}
```

<br/>

### 다른 코드

찾아봤는데 가장 다양한 방법들이 있었다

<br/>

```python
def solution(A):
    A.append(1)
    N = len(A)
    fib = [0] * 27
    fib[1] = 1
    for i in range(2, 27):
        fib[i] = fib[i-1] + fib[i-2]
        if fib[i] > N: 
            fib = fib[2:i]
            break
    
    reachable = [-1]*N
    for jump in fib:
        if A[jump-1] == 1: reachable[jump-1] = 1
    
    for i in range(N):
        if A[i] == 1 and reachable[i] < 0:
            min = N + 1
            minidx = -1
            for jump in fib:
                pre = i - jump
                if pre < 0 or reachable[pre] <0:
                    continue
                if min > reachable[pre]:
                    min = reachable[pre]
                    minidx = pre
            if minidx != -1:
                reachable[i] = min +1
    
    return reachable[-1]
```

> 1. N보다 작은 피보나치 수를 정렬한 리스트 만들기 (fib)
> 
> 2. reachable리스트 만들기. 처음 출발지에서 한 번에 도착할 수 없는 인덱스는 모두 -1의 값을 가지고, 한번에 갈 수 있는 인덱스는 1의 값을 준다.
> 
> 3. 이후 i= 0~N에서, A[i] = 1(잎사귀가 존재)이고 reachable[i] = -1인 (아직 도착 방법을 찾지 못함) 지점을 찾아서
> 
> 한번의 점프로 i에 갈 수 있는 인덱스(pre)들 중 reachable값(지금까지 탐색된 경로 개수)이 최소인 pre를 찾아서 최소 경로 개수를 구한다.
> 
> 4. reachable[-1]을 반환하면 답

왜 27까지 인걸까??

<br/>

```python
def generate_fib_steps(num):
    """Generate Fibonacci numbers iteratively."""
    result = []
    first, second = 0, 1
    while first + second <= num:
        result.append(first + second)
        first, second = second, first + second
    return result

def solution(numbers):
    """Solution method implementation."""
    # build new list, containing start and end positions
    nums = [1] + list(numbers) + [1]
    # get length of original list
    list_len = len(nums)
    # generate fibonacci jump distances
    fib_steps = generate_fib_steps(list_len)
    # build list of jumps
    jumps = [0] + [list_len] * (list_len - 1)
    # iterate through leafs and get the minimum jump count
    for crt in [i for i in range(1, list_len) if nums[i] == 1]:
        for step in fib_steps:
            jumps[crt] = min(jumps[crt], jumps[crt - step] + 1)
    return jumps[-1] if jumps[-1] < list_len else -1
```

`for crt in [i for i in range(1, list_len) if nums[i] == 1]` 이 부분을 모르겠어

<br/>

```python
import sys


def generate_fib_steps(n):
    a, b = 1, 1
    yield 1
    while a + b <= n:
        yield a + b
        a, b = b, a + b


def solution(A):
    n = len(A) + 1

    ret = [sys.maxsize] * (n + 1)
    ret[0] = 0

    for i in range(1, n + 1):
        if (i < n and A[i - 1] == 1) or (i == n):
            ret[i] = min([ret[i - x] + 1 for x in generate_fib_steps(i)])

    return ret[-1] if ret[-1] < sys.maxsize else -1
```

> - the frog can get to the other side of the river (from position −1 to position N), therefore the real length of the way is N + 1.
> - The frog can jump over any distance F(K), where F(K) is the K-th Fibonacci number means to reach position ith, the frog could go from positions (i — F(K)) where i — F(K) ≥ 0 and there is a leave at the position i — F(K).
> - there are many leaves on the river, and the frog can jump between the leaves, thus there are some immediate positions are not permitted.
> - The goal is to count the minimum number of jumps, so OPT(n) = min(1 + OPT(n — F[K]))

`ret[i - x] + 1 for x in generate_fib_steps(i)]` 여기선 이거...

<br/>

```python
def solution(A):
    # setting fibonacci numbers
    target = len(A)
    if target == 0:
        return 1

    fibo = [0, 1]
    pre1, pre2 = fibo[-2], fibo[-1]
    while -1 + fibo[-1] <= target:
        fibo.append(pre1 + pre2)
        pre2 = fibo[-1]
        pre1 = fibo[-2]
    #print(fibo)

    ret = -1
    arrival = False
    q = [[-1, 0]]
    visit = [False] * (target + 1)
    while q:
        pos = q[0][0]
        cnt = q[0][1]
        del q[0]
        #print(pos, cnt)
        
        if arrival:
            break

        for f in fibo:
            next_pos = pos + f
            if next_pos == target:
                arrival = True
                ret = cnt + 1
                break
            if next_pos < target and not visit[next_pos] and A[next_pos] == 1:
                q.append([next_pos, cnt + 1])
                visit[next_pos] = True
    
    return ret
```

bfs를 이용한 풀이



<br/>

```python
def fib_up_to(n):
    numbers = [1]
    i = 1
    while True:
        new_num = (numbers[-2] + numbers[-1]) if i > 1 else 2
        if new_num > n:
            break
        numbers.append(new_num)
        i += 1
    return numbers

def solution(A):
    n = len(A)
    if n == 0:
        return 1
    numbers = fib_up_to(n+1)

    possible_positions = set([-1])
    for k in range(1, n+1):
        positions_after_k = set()
        for pos in possible_positions:
            for jump in numbers:
                if pos + jump == n:
                    return k
                if pos + jump < n and A[pos + jump]:
                    positions_after_k.add(pos + jump)
        possible_positions = positions_after_k
    return -1
```

> The idea is to track all possible positions of the frog after k jumps. If possible position == n, return k.

라는 언급이 있었다..

<br/>

### 함수 정리

`dictionary.get(key, value)` 

- key: 값을 가져올 key 이름

- value: key가 존재하지 않을 때 반환 값



<br/>

### 느낀점

하 진짜 너무 어렵다............ ㅜㅜ
