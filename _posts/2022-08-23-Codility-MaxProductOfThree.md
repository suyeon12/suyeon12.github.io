---
layout: post

title: Codility MaxProductOfThree

tags: sort
---

## Lesson 6. Sorting - MaxProductOfThree

### 문제

A non-empty array A consisting of N integers is given. The *product* of triplet (P, Q, R) equates to A[P] * A[Q] * A[R] (0 ≤ P < Q < R < N).

For example, array A such that:

A[0] = -3
 A[1] = 1
 A[2] = 2
 A[3] = -2
 A[4] = 5
 A[5] = 6

contains the following example triplets:

> - (0, 1, 2), product is −3 * 1 * 2 = −6
> - (1, 2, 4), product is 1 * 2 * 5 = 10
> - (2, 4, 5), product is 2 * 5 * 6 = 60

Your goal is to find the maximal product of any triplet.

Write a function:

> def solution(A)

that, given a non-empty array A, returns the value of the maximal product of any triplet.

For example, given array A such that:

A[0] = -3
 A[1] = 1
 A[2] = 2
 A[3] = -2
 A[4] = 5
 A[5] = 6

the function should return 60, as the product of triplet (2, 4, 5) is maximal.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [3..100,000];
> - each element of array A is an integer within the range [−1,000..1,000].

<br/>

### 상황 분석

뒤에 코드 작성하면서도 나오겠지만 3개가 모두 양수일 때와 음수나 0이 섞여있을 때를 구분해야 한다.

<br/>

### 코드 작성

```python
def solution(A):
    A.sort()
    return A[-1] * A[-2] * A[-3]
```

뭐.. 처음 드는 생각대로 써 봤다. 순서대로 정렬한 후 가장 마지막 값을 곱한 결과를 내면 되지 않나? 했는데 정확도가 떨어졌다. 왜 이러나 싶었는데 [-5, -5, -4, 3] 케이스에서는 원하는 결과가 안 나온다는 거다. 흠 그렇지.. 하고 다시 생각해봤다

<br/>

```python
def solution(A):
    temp = sorted(A)
    for i in range(len(temp)-2):
        ans = 0
        if temp[i] > 0 and temp[i+1] > 0 and temp[i+2] > 0:
            ans = temp[-1] * temp[-2] * temp[-3]
        elif  temp[i] <= 0 and temp[i+1] <= 0 and temp[i+2] >= 0:
            ans = temp[0] * temp[1] * temp[-1]
    return ans
```

모두 양수일 때는 아까처럼하고 음수가 2개 섞여있고 하나는 0보다 클 경우(위 테스트케이스)에서는 첫번째, 두번째 값과 마지막 값을 곱해주면 제일 크더라. 이것저것 실험해보니 나름대로 일반화할만해서 이렇게 적었다. 좀 코드는 더럽지만... 맞겠지? 했는데 비슷하거나 더 낮은 정확도가 나왔다(....) [-10, -5, -4] 케이스에서는 0을 반환한다는 거다. 뭐 모두 양수이거나 2개 음수, 하나 양수인 경우에 안 들어가서 그렇긴 한데........! .......

<br/>

```python
def solution(A):
    A.sort()
    return max(A[-1] * A[-2] * A[-3], A[0] * A[1] * A[-1])
```

결론은 아 이거 하나하나 if 돌리면 또 다른게 튀어나올지 몰라 ^^... 하면서 큰 값을 return 하기로 했다. 아까 말했듯 이것저것 해본 결과 뒤에서 세 개 혹은 앞 두개와 제일 끝 하나 둘 중 하나에 답이 있었으니까,,, 어쨌든 세 번의 시도 끝에 O(N * log(N))으로 통과했다.

<br/>

### 느낀점

<img width="424" alt="스크린샷 2022-08-23 오후 6 07 42" src="https://user-images.githubusercontent.com/72901045/186119386-a7c1c215-20dd-40ca-ae36-c42305ff8719.png">

노트에 이것저것 쓰면서 하고 있는데 종이랑 펜 지참 안되는 코테에서는 어떡하지,,? ㅋㅋㅜ 언젠가 손으로 쓰지 않아도 생각만으로 이해할 수 있을까...
