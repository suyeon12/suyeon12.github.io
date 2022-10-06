---

layout: post

title: Codility MaxDoubleSliceSum

summary: range()

---

## Lesson 9. Maximum slice problem - MaxDoubleSliceSum

### 문제

A non-empty array A consisting of N integers is given.

A triplet (X, Y, Z), such that 0 ≤ X < Y < Z < N, is called a *double slice*.

The *sum* of double slice (X, Y, Z) is the total of A[X + 1] + A[X + 2] + ... + A[Y − 1] + A[Y + 1] + A[Y + 2] + ... + A[Z − 1].

For example, array A such that:

A[0] = 3
 A[1] = 2
 A[2] = 6
 A[3] = -1
 A[4] = 4
 A[5] = 5
 A[6] = -1
 A[7] = 2

contains the following example double slices:

> - double slice (0, 3, 6), sum is 2 + 6 + 4 + 5 = 17,
> - double slice (0, 3, 7), sum is 2 + 6 + 4 + 5 − 1 = 16,
> - double slice (3, 4, 5), sum is 0.

The goal is to find the maximal sum of any double slice.

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N integers, returns the maximal sum of any double slice.

For example, given:

A[0] = 3
 A[1] = 2
 A[2] = 6
 A[3] = -1
 A[4] = 4
 A[5] = 5
 A[6] = -1
 A[7] = 2

the function should return 17, because no double slice of array A has a sum of greater than 17.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [3..100,000];
> - each element of array A is an integer within the range [−10,000..10,000].

<br/>

### 상황 분석

y를 기준으로 왼쪽과 오른쪽의 최댓값을 구한 후 더한다. 

<br/>

### 코드 작성

```python
def solution(A):
    left_sum = [0] * len(A)
    right_sum = [0] * len(A)
    max_sum = 0

    for i in range(1, len(A)-1):
        left_sum[i] = max(left_sum[i-1] + A[i], 0)
        # print(left_sum)


    for i in range(len(A)-2, 0, -1):
        right_sum[i] = max(right_sum[i+1] + A[i], 0)
        # print(right_sum)

    for i in range(1, len(A)-1):
        max_sum = max(max_sum, left_sum[i-1] + right_sum[i+1])
    
    return max_sum
```

잘 모르겠어서 구글링했다 ㅋㅎ.. 가장 참고한 기사는 [여기](https://medium.com/@deck451/codility-algorithm-practice-lesson-9-maximum-slice-problem-task-3-maxdoubleslicesum-a-python-662b74bf4b3e)! `left_sum = [0, 2, 8, 7, 11, 16, 15, 0]`이고 `right_sum = [0, 16, 14, 8, 9, 5, 0, 0]` 인데 항상 내가 그렇듯... 디버그하듯이 점검했는데 이건 그것보다 다르게 이해하는게 좀 더 편했다

마지막 `max_sum`은 max 부분을 빼고 보면 `left_sum[i-1] + right_sum[i+1]` 이다. 여기서 `left_sum[i-1]` 을 위 식과 비슷하게 풀어써보면 `left_sum[i-2] + A[i-1]`이다. `right_sum[i+1]`은 `right_sum[i+2] + A[i+1]`이다. 좀 깔끔하게 정리하면 i를 기준으로 동일하게 떨어져 있는 배열의 합이랑 같은 것이다. 여기에 `max`를 하니 가장 큰 값이고! 위에 봤던 y가 여기서는 i라고 생각하니 이해가 편했다. 시간복잡도는 O(N)

<br/>

### 함수 정리

`range(start, stop, step)` step 부분에 음수를 넣으면 숫자가 감소함

<br/>

### 느낀점

다들 어렵다고 하는구만... 그래 어렵다니까..! ㅜㅡㅜ,,,
