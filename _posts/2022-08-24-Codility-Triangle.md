---

layout: post

title: Codility Triangle

summary: while, for, break, continue

tag: sort

---

## Lesson 6. Sorting - Triangle

### 문제

An array A consisting of N integers is given. A triplet (P, Q, R) is *triangular* if 0 ≤ P < Q < R < N and:

> - A[P] + A[Q] > A[R],
> - A[Q] + A[R] > A[P],
> - A[R] + A[P] > A[Q].

For example, consider array A such that:

A[0] = 10 A[1] = 2 A[2] = 5
 A[3] = 1 A[4] = 8 A[5] = 20

Triplet (0, 2, 4) is triangular.

Write a function:

> def solution(A)

that, given an array A consisting of N integers, returns 1 if there exists a triangular triplet for this array and returns 0 otherwise.

For example, given array A such that:

A[0] = 10 A[1] = 2 A[2] = 5
 A[3] = 1 A[4] = 8 A[5] = 20

the function should return 1, as explained above. Given array A such that:

A[0] = 10 A[1] = 50 A[2] = 5
 A[3] = 1

the function should return 0.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - each element of array A is an integer within the range [−2,147,483,648..2,147,483,647].

<br/>

### 코드 작성

```python
def solution(A):
    A.sort()
    ans = []
    for p in range(len(A)-2):
        if A[p] + A[p+1] > A[p+2] and A[p+1] + A[p+2] > A[p] and A[p+2] + A[p] > A[p+1]:
            ans.append(p)
        else:
            pass
    if len(ans) > 0:
        return 1
    else:
        return 0
```

O(N*log(N))의 시간복잡도가 나왔다. 전체 다 100%로 통과하긴 했는데 if 구문도 너무 길고 `else: pass` 이 부분도 그냥 생략할 수 있지 않을까? 방금 찾아봤는데 가능하군.. 어쩐지 좀 지저분하더라

<br/>

```python
def solution(A):
    A.sort()

    for p in range(len(A)-2):
        # print(A[p], A[p+1], A[p+2])
        if A[p] + A[p+1] > A[p+2] and A[p+1] + A[p+2] > A[p] and A[p+2] + A[p] > A[p+1]:
            ans = 1
            pass    # break
        else:
            ans = 0
    return ans
```

이전에는 이렇게 풀었는데 계속 0으로 빠졌다. print로 보니까 5,8,10 멀쩡하게 잘 구해놓고 계속 반복하다보니 마지막에 0으로 빠진 거였다... 그래서 pass 써줬는데도 왜 이러지??싶었는데 이거 옮겨 쓰면서 기억남. pass가 아니라 break를 써야 한다는 사실...~~ ㅋㅋ 아니 나는 다 구했으면 지나가라,,라는 의미로 쓴 거 였는데 그게 아니라 멈추라는 break를 써야 했구나,, 잘 안되서 급 리스트 추가했는데!

<br/>

### 다른 코드

```python
def solution(numbers):
    """Solution method implementation."""
    # sort the list
    sorted_list = sorted(numbers)
    # get the length of the list
    list_len = len(sorted_list)
    # iterate through the list and perform the check
    for i in range(list_len - 2):
        if sorted_list[i] > sorted_list[i + 2] - sorted_list[i + 1]:
            return 1
    
    return 0
```

- in a sorted array, we can be sure that arr[i+1] + arr[i+2] > arr[i], so one of the three criteria is already satisfied;
- we can also be sure that arr[i] + arr[i + 2] > arr[i+1], so that makes two criteria already checked;
- we only need to check triplets formed by arr[i], arr[i+1] and arr[i+2]. The condition we need to check is the remaining criterion, which is whether arr[i] + arr[i+1] > arr[i+2].

내가 저 위에 and로 조건 여러 개 달았던게 정렬을 하면 하나만 생각해도 괜찮았다! 그냥 레슨이 정렬이니까 정렬 쓰고 조건 별 생각없이 다 썼는데^^.... 식 쓸 때는 한 번 더 생각하고 쓰자~

<br/>

### 함수 정리

`while`  해당 조건이 거짓이 될 때까지 반복해서 실행하는 제어문

`for`  python의 경우 초기식, 조건식, 증감식의 반복이 아닌 리스트나 튜플, 딕셔너리 안의 원자값을 시퀀스대로 가져오는 반복문

`break`  조건문에 의해 루프를 탈출하는 것이 아닌 강제적으로 탈출

`continue`  반복문의 뒤 내용을 무시하고 반복문의 다음 스텝으로 넘어감(반복문을 건너뛸 수 있는 것처럼 보임)

<br/>

### 느낀점

이전 [MissingInteger](https://suyeon12.github.io/2022/08/10/codility-missinginteger) 포스팅에서 for 구문을 잘못 생각한 것 같다고 쓴 적이 있엇는데 이게 파이썬에서는 원자값을 시퀀스대로 가져와서 그런거구나 ㅋㅋㅋ 이참에 이런저런 구문을 확실히 이해할 수 있어서 좋았다! 오늘은 다 풀기까지 38분이 걸렸는데 뭔가 이상적으로(?) 푼 것 같다. 잘 통과되기도 했고 정리도 오랜만에 하고.. ㅋㅋㅋ 난이도 Easy인데 이렇게 만족하면 안될 것 같긴 하지만(...)


