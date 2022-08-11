---

layout: post

title: Codility PassingCars

---

## Lesson 5. Prefix Sums - PassingCars

### 문제

A non-empty array A consisting of N integers is given. The consecutive elements of array A represent consecutive cars on a road.

Array A contains only 0s and/or 1s:

> - 0 represents a car traveling east,
> - 1 represents a car traveling west.

The goal is to count passing cars. We say that a pair of cars (P, Q), where 0 ≤ P < Q < N, is passing when P is traveling to the east and Q is traveling to the west.

For example, consider array A such that:

A[0] = 0
 A[1] = 1
 A[2] = 0
 A[3] = 1
 A[4] = 1

We have five pairs of passing cars: (0, 1), (0, 3), (0, 4), (2, 3), (2, 4).

Write a function:

> de solution(A)

that, given a non-empty array A of N integers, returns the number of pairs of passing cars.

The function should return −1 if the number of pairs of passing cars exceeds 1,000,000,000.

For example, given:

A[0] = 0
 A[1] = 1
 A[2] = 0
 A[3] = 1
 A[4] = 1

the function should return 5, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer that can have one of the following values: 0, 1.

<br/>

### 상황 분석

![스크린샷 20220811 오후 9 14 42](https://user-images.githubusercontent.com/72901045/184131174-36b81af4-331a-40f8-a027-5b8db0b9deed.png)

왜 이렇게... 문제 이해하는 게 어려운지 ^^; A[0]과 A[2]은 오른쪽을 바라보고 A[1], A[3], A[4]는 왼쪽을 바라보고 있다. A[0] 입장에서 계속 출발한다면 얘는 1, 3, 4 세 차량과 마주친다(0, 1), (0, 3), (0, 4). A[2]의 입장에서는 3, 4와 마주칠 것이다(2, 3), (2, 4). 따라서 총 3+2=5해서 다섯 차량을 마주치게 되고, return값도 그렇게 제시하면 된다!

<br/>

### 알고리즘

처음에는 0이 나오면 위에 있는 1의 합과 그다음 0이 나오면 이후 1의 개수를 계속 더해 합하는.. 그런 걸 생각했는데 솔직히 이렇게 바로 나오는 건 언제나 효율이 별로다. 머리를 더 굴려야 한다는 말,,^^...

<br/>

### 코드 작성

```python
def solution(A):
    count = 0
    zero_count = 0
    for i in A:
        if i == 0:
            zero_count += 1
        else:
            count += 1

    return zero_count + count

    if count + zero_count > 1000000000:
        return -1
```

0을 만나면 zero_count를 하나씩 늘리고 1이라면 그냥 count를 하나씩 늘려서 둘을 합친다는 것이다. 지금보니 ??? 싶지만 예시코드에서는 나름 잘 작동했다 ㅎ... 근데 예시에서만 작동해 완성도가 10%였다. 매우 처참........

<br/>

```python
def solution(A):
    count = 0
    zero_count = 0
    for i in A:
        if i == 0:
            zero_count += 1
        else:
            count += zero_count
    return count

    if count > 1000000000:
        return -1
```

이번에는 1의 입장에서 생각해봤다. A[1]에서 마주치는 차량은 0 하나이다. A[3]이 마주치는 차량은 0, 2 두 대이다. A[4] 역시 0, 2 두 대를 마주친다. 따라서 1+2+2=5를 반환하면 된다... 를 토대로 코드를 작성해봤다. 이렇게 하니 또 오류가 났다. return이 저기 있는게 문제였다.

<br/>

```python
def solution(A):
    count = 0
    zero_count = 0
    for i in A:
        if i == 0:
            zero_count += 1
        else:
            count += zero_count

    if count > 1000000000:
        count = -1

    return count
```

암튼 다 수정해서 완성 ^ㅡ^!!

<br/>

### 다른 코드

오늘은 다른 코드도 내가 작성한 것과 상당히 비슷해서 생략하도록 하겠다,, 이 경우도 처음이네 하하하 마찬가지로 함수 정리도 생략하겠다,, 딱히 함수를 쓴 게 없다..!!

<br/>

### 느낀점

슬슬(?) 파이썬에 익숙해져 가는 것 같다! 근데 다른 것보다 문제 이해가 가장 어렵다... ㄱ-

![스크린샷 20220811 오후 10 44 56](https://user-images.githubusercontent.com/72901045/184147851-992fa98c-592e-432c-b630-7768df719b07.png)

어쨌거나 성공했을 때는 뿌듯하다 ^ㅡ^~~


