---

layout: post

title: Codility MaxSliceSum

---

## Lesson 9. Maximum slice problem - MaxSliceSum

### 문제

A non-empty array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P ≤ Q < N, is called a *slice* of array A. The *sum* of a slice (P, Q) is the total of A[P] + A[P+1] + ... + A[Q].

Write a function:

> def solution(A)

that, given an array A consisting of N integers, returns the maximum sum of any slice of A.

For example, given array A such that:

A[0] = 3 A[1] = 2 A[2] = -6
A[3] = 4 A[4] = 0

the function should return 5 because:

> - (3, 4) is a slice of A that has sum 4,
> - (2, 2) is a slice of A that has sum −6,
> - (0, 1) is a slice of A that has sum 5,
> - no other slice of A has sum greater than (0, 1).

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..1,000,000];
> - each element of array A is an integer within the range [−1,000,000..1,000,000];
> - the result will be an integer within the range [−2,147,483,648..2,147,483,647].

<br/>

### 알고리즘

부분합 중 가장 큰 값을 return하면 된다

<br/>

### 코드 작성

```python
def solution(A):
    sum = 0
    maxsum = A[0]
    for i in A:
        sum = max(sum + i, i)
        maxsum = max(maxsum, sum)
        # print(maxsum)
    return maxsum
```

이것저것 해보다가(사실 지난번에 내가 푼 [문제](https://suyeon12.github.io/2022/10/03/codility-maxprofit) 참고함 ㅋㅋㅋ) 어 혹시..? 하고 했는데 되더라. 사실 이런것보다 보는 순간 바로 이거다! 하고 풀어야 하는데 ^ㅡ^.. 시간복잡도는 O(N)

<br/>

### 다른 코드

```python
def solution(A):
    partSum = 0
    maxSum = -1000000
    for n in A:
        maxSum = max(maxSum, partSum+n)
        partSum = max(0, partSum+n)
    return maxSum
```

합이 음수가 아닐 때까지 계속 더하면서 최댓값을 구하고 합이 음수라면 이전의 합을 포함하지 않고 다음 수부터 합을 구한다는게 직관적이네

<br/>

### 느낀점

오랜만에 [링크](https://app.codility.com/demo/results/trainingSB6PTH-SSR/) 올려봅니다 푸하하... 찾아보니 이와 관련된 알고리즘이 `Kadane’s Algorithm` 이던데 다음에 공부해봐야겠다!


