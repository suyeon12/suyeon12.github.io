---

layout: post

title: Codility NailingPlanks

summary: zip()

---

## Lesson 14. Binary search algorithm -NailingPlanks

### 문제

You are given two non-empty arrays A and B consisting of N integers. These arrays represent N planks. More precisely, A[K] is the start and B[K] the end of the K−th plank.

Next, you are given a non-empty array C consisting of M integers. This array represents M nails. More precisely, C[I] is the position where you can hammer in the I−th nail.

We say that a plank (A[K], B[K]) is nailed if there exists a nail C[I] such that A[K] ≤ C[I] ≤ B[K].

The goal is to find the minimum number of nails that must be used until all the planks are nailed. In other words, you should find a value J such that all planks will be nailed after using only the first J nails. More precisely, for every plank (A[K], B[K]) such that 0 ≤ K < N, there should exist a nail C[I] such that I < J and A[K] ≤ C[I] ≤ B[K].

For example, given arrays A, B such that:

A[0] = 1 B[0] = 4
 A[1] = 4 B[1] = 5
 A[2] = 5 B[2] = 9
 A[3] = 8 B[3] = 10

four planks are represented: [1, 4], [4, 5], [5, 9] and [8, 10].

Given array C such that:

C[0] = 4
 C[1] = 6
 C[2] = 7
 C[3] = 10
 C[4] = 2

if we use the following nails:

> - 0, then planks [1, 4] and [4, 5] will both be nailed.
> - 0, 1, then planks [1, 4], [4, 5] and [5, 9] will be nailed.
> - 0, 1, 2, then planks [1, 4], [4, 5] and [5, 9] will be nailed.
> - 0, 1, 2, 3, then all the planks will be nailed.

Thus, four is the minimum number of nails that, used sequentially, allow all the planks to be nailed.

Write a function:

> def solution(A, B, C)

that, given two non-empty arrays A and B consisting of N integers and a non-empty array C consisting of M integers, returns the minimum number of nails that, used sequentially, allow all the planks to be nailed.

If it is not possible to nail all the planks, the function should return −1.

For example, given arrays A, B, C such that:

A[0] = 1 B[0] = 4
 A[1] = 4 B[1] = 5
 A[2] = 5 B[2] = 9
 A[3] = 8 B[3] = 10
 C[0] = 4
 C[1] = 6
 C[2] = 7
 C[3] = 10
 C[4] = 2

the function should return 4, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N and M are integers within the range [1..30,000];
> - each element of arrays A, B and C is an integer within the range [1..2*M];
> - A[K] ≤ B[K].



<br/>

### 코드 작성

```python
def check(A, B, C, k):
    nails = [0] * (2 * len(C) + 1) 

    for i in range(k):
        nails[C[i]] = 1
    #print(nails)

    for i in range(1, len(nails)):
        nails[i] += nails[i-1]
    #print(nails)

    for i in range(len(A)):
        if nails[B[i]] - nails[A[i]-1] == 0:
            return False
    return True


def solution(A, B, C):
    answer = -1
    min_nails = 1
    max_nails = len(C)

    while (min_nails <= max_nails):
        mid = (min_nails + max_nails) // 2
        if check(A,B,C,mid):
            answer = mid
            max_nails -= 1
            #print("max", max_nails)
        else:
            min_nails += 1
            #print("min", min_nails) 
    return answer
```

어제 풀었던 문제를 참고해서 `solution` 부분은 할 만했다. 먼저 못으로 고정할 수 없는 부분은 `-1`을 반환하므로 못으로 고정할 수 있는 최솟값은 1이다. 또한 최댓값은 모든 못을 다 사용하는 경우로 `len(C)`이다. 값은 이 사이에서 이진탐색을 해서 찾으면 되는데 언제나... check 부분이 어렵다 ^^,,

[algorithms-playground/NailingPlanks.py at master · mbobesic/algorithms-playground · GitHub](https://github.com/mbobesic/algorithms-playground/blob/master/codility/NailingPlanks.py)

[Codility/NailingPlanks.java at master · ZRonchy/Codility · GitHub](https://github.com/ZRonchy/Codility/blob/master/Lesson12/NailingPlanks.java)

다음 코드를 참고하여 `check`부분을 작성하였고 print 주석 처리를 해제하면 다음과 같다.

```
[0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0]
[0, 0, 0, 0, 1, 1, 2, 3, 3, 3, 3]
min 2
[0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0]
[0, 0, 0, 0, 1, 1, 2, 3, 3, 3, 3]
min 3
[0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1]
[0, 0, 0, 0, 1, 1, 2, 3, 3, 3, 4]
max 4
[0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0]
[0, 0, 0, 0, 1, 1, 2, 3, 3, 3, 3]
min 4
[0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1]
[0, 0, 0, 0, 1, 1, 2, 3, 3, 3, 4]
max 3
```

이렇게 하고 맞을 줄 알았는데... O((N + M) * N)으로 타임아웃 에러가 떠 50% 성공률이 나왔다.... 아니 왜??!

<br/>

### 다른 코드

```python
def solution(A, B, C):
    # write your code in Python 3.6
    
    N=len(A)
    M=len(C)
    largestPos=max(A+B+C)
    
    low=1
    high=N
    
    lastValid=-1
    while(low<=high):
        mid=(low+high)//2
        
        nailedPositions=[0]*(largestPos+1)
        for i in range(mid):
            nailedPositions[C[i]]=1
        
        for i in range(1,largestPos+1):
            nailedPositions[i]+=nailedPositions[i-1]
        
        possible=True
        for a,b in zip(A,B):
        	# no nail has been nailed in interval [a,b]. 
            if nailedPositions[b]-nailedPositions[a-1]==0:
                possible=False
                break
        
        
        if possible:
            lastValid=mid
            high=mid-1
        else:
            low=mid+1
    
    return lastValid
```

이건 O((N + M) * log(M)) 이더라... 아니 진짜 내가 쓴 거랑 비슷한 것 같은데 대체 뭐가 문제냐 `largestPos=max(A+B+C)` 이 부분... 때문일까? 하아,, zip() 부분은 한 번 해봤는데 비슷하더라구

<br/>

### 함수 정리

`zip()` 길이가 같은 리스트 등의 요소를 묶어주는 함수



<br/>

### 느낀점

시간 복잡도.... 너 뭐야?? ㅜ
