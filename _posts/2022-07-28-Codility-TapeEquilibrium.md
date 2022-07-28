---

layout: post

title: Codility TapeEquilibrium

summary: abs(), append(), float('int')

tags: Time-Complexity

---

## Lesson 3. Time Complexity - TapeEquilibrium

### 문제

A non-empty array A consisting of N integers is given. Array A represents numbers on a tape.

Any integer P, such that 0 < P < N, splits this tape into two non-empty parts: A[0], A[1], ..., A[P − 1] and A[P], A[P + 1], ..., A[N − 1].

The *difference* between the two parts is the value of: |(A[0] + A[1] + ... + A[P − 1]) − (A[P] + A[P + 1] + ... + A[N − 1])|

In other words, it is the absolute difference between the sum of the first part and the sum of the second part.

For example, consider array A such that:

A[0] = 3
 A[1] = 1
 A[2] = 2
 A[3] = 4
 A[4] = 3

We can split this tape in four places:

> - P = 1, difference = |3 − 10| = 7   
> 
> - P = 2, difference = |4 − 9| = 5    
> 
> - P = 3, difference = |6 − 7| = 1    
> 
> - P = 4, difference = |10 − 3| = 7    

Write a function:

> def solution(A)

that, given a non-empty array A of N integers, returns the minimal difference that can be achieved.

For example, given:

A[0] = 3
 A[1] = 1
 A[2] = 2
 A[3] = 4
 A[4] = 3

the function should return 1, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [2..100,000];
> - each element of array A is an integer within the range [−1,000..1,000].

<br/>

### 알고리즘

이거.. for 문 써야하는 거 아닌가?라고 생각하는 순간 시간복잡도가 떠오르긴 했다. 근데 for 안 쓰고 어케하지... 라고 하며 그냥 무작정 코드를 작성했다. min을 0으로 설정했는데 이게 잘못인 것 같다 ㅎ,,

<br/>

### 코드

```python
def solution(A):

    min_ans = 0

    for i in A:
        first = sum(A[:i])
        second = sum(A[i:])
        ans = abs(first - second)
        
        if min_ans == 0 or min_ans > ans:
            min_ans = ans
    return min_ans

```

이렇게 했는데 시간복잡도는 커녕... 계산실패가 나왔다 ^^,,,, 다른 분들 코드 보니까 min을 0으로 하지말고 none으로 줬어야 했다!! 그리고 append해서 min을 쓸 수 있었다 ㅎ..

```python
def solution(A):
    
    diff = []
    for p in range(1, len(A)) :
        tape1 = A[:p]
        tape2 = A[p:len(A)]
        
        sum1 = sum(tape1)
        sum2 = sum(tape2)
        
        diff.append(abs(sum1 - sum2))
        
    diff = min(diff)

    return diff
```

이렇게

하지만 더 효율적인 코드를 발견할 수 있었다.

<br/>

```python
def solution(A):
    # write your code in Python 3.6
    first=A[0]
    second=sum(A[1:])
    res=abs(first-second)
    for i in range(1,len(A)-1):
        first+=A[i]
        second-=A[i]
        res=min(res,abs(first-second))
        
    return res

```

여담으로 none을 안주고 싶다면 이렇게 풀어도 된다! 아니다 다시 보니 좀 다른 것 같기도 하고 ㅎㅎ..

<br/>

### 다른 코드

```python
def solution(A) :
    
    tape1 = 0
    tape2 = sum(A)
    
    diff = []
    
    for p in A :
        tape1 += p
        tape2 -= p
        
        diff.append(abs(tape1 - tape2))
    
    return min(diff[:-1])
```

이건 구상이 되게 신기했다. 초기값을 0으로 준 건 더한게 최종 값이고 A의 합이 초기값인 것은 계속해서 뺀 게 최종값이다. 그래서 이 둘을 빼서 절댓값을 취한 걸로 최솟값을 구하는게 너무 신기하다..! 더하고 뺀다는 걸 생각한다는게 ㅎㅁㅎ,, 이렇게 하면 시간복잡도도 O(N)으로 매우 간단하게 구해진다.

<br/>

```python
def solution(A):
    
    s = sum(A)
    b = 0
    m = float('inf')
    for i in A[:-1]:
        b += i
        S = abs(s-b*2)

        if m > S :
            m = S

    return m
```

갠적으로 가장 마음에 들었던 코드.. 좌변과 우변을 비교해서 규칙을 발견하고 그에 따라 알고리즘을 구현했는데 이런거 정말 좋아하는데 왜 이 생각을 못했지,,? 코드는 총 합에 뺀 숫자까지의 합에다가 2를 곱한 걸 빼면 return값이 나오는 걸 이용했다.

그러니까 P=2라면 |(a+b) - (c+d+e)|  -> |(a+b+c+d+e) - 2(a+b)|

이거 왜인지는 잘 모르겠으나 글씨가 깨지는데 저 무지막지한 빈칸은 절댓값이다,,

<br/>

### 정리

`abs(x)` x의 절댓값이 반환됨

`A.append(obj)` 배열의 가장 마지막에 obj 추가

`float('inf')` 양의 무한대를 표현 (cf. 음의 무한대는 `float('-inf')`

<br/>

### 느낀점

사실 1일 1커밋을 한다고 오늘 문제는 시간에 쫓겨 대충 푼 것 같다. 어차피 지금 푸는 codility 문제들은 시간 제한 없이 나 스스로 파이썬과 알고리즘에 익숙해지기 위해 푸는 것인데,, 뭔가 커밋을 위한 알고리즘이 된 느낌 ^ㅡㅜ.... 오늘 처음으로 문제가 아깝다는 생각을 했다. 진짜 나 규칙 찾는거 엄청 좋아하는데,,, 평소에는 이게.. 어떻게 되는걸까?라고 생각하면서 푼다면 오늘은 아 이거 빨리 풀어야하는데...를 먼저 생각한 것 같다 ㅎㅎ,, 진짜 다음부터는 절대!! 이렇게 푸는 일 없도록....... !!!!!


