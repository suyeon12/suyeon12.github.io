---

layout: post

title: Codility FrogJmp

summary: divmod()

tags: Time-Complexity

---



## Lesson 3. Time Complexity - FrogJmp

### 문제

A small frog wants to get to the other side of the road. The frog is currently located at position X and wants to get to a position greater than or equal to Y. The small frog always jumps a fixed distance, D.

Count the minimal number of jumps that the small frog must perform to reach its target.

Write a function:

> def solution(X, Y, D)

that, given three integers X, Y and D, returns the minimal number of jumps from position X to a position equal to or greater than Y.

For example, given:

X = 10
 Y = 85
 D = 30

the function should return 3, because the frog will be positioned as follows:

> - after the first jump, at position 10 + 30 = 40
> - after the second jump, at position 10 + 30 + 30 = 70
> - after the third jump, at position 10 + 30 + 30 + 30 = 100

Write an ****efficient**** algorithm for the following assumptions:

> - X, Y and D are integers within the range [1..1,000,000,000];
> - X ≤ Y.

<br/>

### 코드 작성

```python
def solution(X, Y, D):
    i = 0
    while Y > X + (D * i):
        i = i + 1   
    return i
```

사실 딱 처음 봤을 때 이거... while 쓰면 바로 되는거 아닌가? 하고 썼다. 물론 lesson 제목이 시간 복잡도인게 살짝 걸렸지만^ㅡ^... 일단 테스트 돌려봐도 통과하고! 진짜 10분 컷...?하고 제출하면서도 효율적인 알고리즘에서 걸렸는데 역시~ 완성도는 33%가 나왔다 ㅎ... (시간복잡도는 O(Y-X),,)

 <br/>

### 다른 코드

```python
def solution(X, Y, D):
    # write your code in Python 3.6
    between = Y - X
    if not between:
        return 0

    div, mod = divmod(between,D)
    if not mod: # equal to
        return div
    else: # greater than
        return div +1
```

내가 곱하기하고 더해서 크기 비교한 걸 나눠서 생각할 수도 있구나.. 그리고 예외처리를 되게 꼼꼼하게 해줘야 하네. X와 Y가 같을 경우는 0을 반환! `divmod(a,b)` 는 나눈 몫과 나머지를 튜플로 반환하는 파이썬의 내장 함수이다. 만약 나머지가 없다면 깔끔하게 나눠진 경우로 몫만 반환하면 되고 그렇지 않다면 몫에서 1을 더한 결과를 반환한다,

문제의 (10, 85, 30)을 예로 들면 75를 30으로 나눈다. 그럼 몫이 2, 나머지가 15인데 이는 else에 해당하므로 몫에서 1을 더한 3을 반환한다.

<br/>

### 정리

`divmod(a,b)` 매개변수로 받은 a를 b로 나눈 후 몫과 나머지를 튜플 데이터 타입으로 반환

<br/>

### 느낀점

예외처리를 꼼꼼히 하자..! 그리고 된다고 되네,,?하고 넘어가지말고 효율적인 코드인지 한 번 더 생각해보기
