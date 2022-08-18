---
layout: post

title: Codility MinAvgTwoSlice


---

## Lesson 5. Prefix Sums - MinAvgTwoSlice

### 문제

A non-empty array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P < Q < N, is called a *slice* of array A (notice that the slice contains at least two elements). The *average* of a slice (P, Q) is the sum of A[P] + A[P + 1] + ... + A[Q] divided by the length of the slice. To be precise, the average equals (A[P] + A[P + 1] + ... + A[Q]) / (Q − P + 1).

For example, array A such that:

A[0] = 4
 A[1] = 2
 A[2] = 2
 A[3] = 5
 A[4] = 1
 A[5] = 5
 A[6] = 8

contains the following example slices:

> - slice (1, 2), whose average is (2 + 2) / 2 = 2;
> - slice (3, 4), whose average is (5 + 1) / 2 = 3;
> - slice (1, 4), whose average is (2 + 2 + 5 + 1) / 4 = 2.5.

The goal is to find the starting position of a slice whose average is minimal.

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N integers, returns the starting position of the slice with the minimal average. If there is more than one slice with a minimal average, you should return the smallest starting position of such a slice.

For example, given array A such that:

A[0] = 4
 A[1] = 2
 A[2] = 2
 A[3] = 5
 A[4] = 1
 A[5] = 5
 A[6] = 8

the function should return 1, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [2..100,000];
> - each element of array A is an integer within the range [−10,000..10,000].

<br/>

### 코드 작성

```python
def solution(A):
    init = sum(A[0:2])/2

    for p in range(len(A)):
        for q in range(p+1,len(A)-1):
            temp = sum(A[p:q+1])/len(A[p:q+1])
            if init >= temp:
                init = temp
                ans = p
    return ans
```

for문 안 for문으로 시간복잡도가 최악이지만 ^ㅡ^.. 일단 비효율적으로 써놓고 점차 개선하자!를 토대로 쓴 코드. 그리고 결과는,, 10인가 20인가.. 일단 정확도부터 영 틀렸다

<br/>

```python
def solution(A):
    init = sum(A[0:2])/2
    init_list = [0]
    temp = 0

    for i in A:
        temp += i
        init_list.append(temp)

    for p in range(len(A)-1):
        for q in range(p+1,len(A)):
            p_avg = (init_list[q+1] - init_list[p]) / (q-p+1)
            if init >= p_avg:
                init = p_avg
                ans = p
    return ans
```

그래서 리스트로 해 놓고 prefix sum인만큼 이 개념 이용해서 p_avg 이용해서 해봤는데 50 ㅋㅋ.. 좀 더 해볼까했는데 아 어차피 시간복잡도 때문에 통과 못할 거 이걸 해봤자.....라는 생각이 너~~무 많이 들어서 그냥,, 구글을 참고했다 ㅎ

<br/>

### 다른 코드

```python
def solution(A):
    minAvg = (A[0] + A[1])/2
    minIdx = 0
    for i in range(2,len(A)):
        avg = (A[i-2] + A[i-1] + A[i])/3
        if (minAvg > avg):
            minAvg = avg
            minIdx = i-2

        avg = (A[i-1] + A[i])/2
        if (minAvg > avg):
            minAvg = avg
            minIdx = i-1

    return minIdx
```

다음 블로그들([1](https://cheolhojung.github.io/posts/algorithm/Codility-MinAvgTwoSlice.html), [2](https://davi06000.tistory.com/58), [3](https://nukeguys.tistory.com/175))을 참고해서 이해했다. 이건 수학적 지식이 필요한 문제였다....! 아 진짜 어쩐지 안 풀리더라ㅜ

(a+b) <= (c+d)일 때 (a, b)와 (c, d)의 평균은 (a + b) 이상이 된다. 원소가 4개(a, b, c, d)인 그룹은 (a, b)와 (c, d)를 나누었을 때, 각각의 평균의 작은 값 이상이 된다. 따라서 2개인 그룹이 작은 값이 되어 4개의 그룹은 고려할 필요가 없다.

여기서 가질 수 있는 평균의 최소값은 배열의 가장 작은 값과 같다. **전체 배열의 평균값은 subarray의 평균값 중 작은 값보다 같거나 크다. 가장 작은 단위의 subarray는 문제를 기준으로 원소를 2개** 가진다(p, q). 만약 원소가 3인 그룹을 확인하려면 2와 1로 나눌 수 있는데 최솟값이 2이므로 **최종적으로 2와 3**을 생각하면 된다.

<br/>

### 느낀점

나는 코딩할 때 수학보다 영어가 더 필요하다고 생각하는 사람인데 알고리즘을 풀 때는 수학이 좀 더 중요한 것 같다 ㅎ... 진짜 이런 지식을 문제를 보고 유추해낼 수 있나? 이건 사전지식이 필요한 문제가 아닌가 싶다.
