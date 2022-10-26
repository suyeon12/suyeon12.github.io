---
layout: post

title: Codility Flags

summary: range(), try except

---

## Lesson 10. Prime and composite numbers - Flags

### 문제

A non-empty array A consisting of N integers is given.

A *peak* is an array element which is larger than its neighbours. More precisely, it is an index P such that 0 < P < N − 1 and A[P − 1] < A[P] > A[P + 1].

For example, the following array A:

A[0] = 1
 A[1] = 5
 A[2] = 3
 A[3] = 4
 A[4] = 3
 A[5] = 4
 A[6] = 1
 A[7] = 2
 A[8] = 3
 A[9] = 4
 A[10] = 6
 A[11] = 2

has exactly four peaks: elements 1, 3, 5 and 10.

You are going on a trip to a range of mountains whose relative heights are represented by array A, as shown in a figure below. You have to choose how many flags you should take with you. The goal is to set the maximum number of flags on the peaks, according to certain rules.

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/flags/static/images/auto/6f5e8faa3000c0a74157e6e0bc759b8a.png)

Flags can only be set on peaks. What's more, if you take K flags, then the distance between any two flags should be greater than or equal to K. The distance between indices P and Q is the absolute value |P − Q|.

For example, given the mountain range represented by array A, above, with N = 12, if you take:

> - two flags, you can set them on peaks 1 and 5;
> - three flags, you can set them on peaks 1, 5 and 10;
> - four flags, you can set only three flags, on peaks 1, 5 and 10.

You can therefore set a maximum of three flags in this case.

Write a function:

> def solution(A)

that, given a non-empty array A of N integers, returns the maximum number of flags that can be set on the peaks of the array.

For example, the following array A:

A[0] = 1
 A[1] = 5
 A[2] = 3
 A[3] = 4
 A[4] = 3
 A[5] = 4
 A[6] = 1
 A[7] = 2
 A[8] = 3
 A[9] = 4
 A[10] = 6
 A[11] = 2

the function should return 3, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..400,000];
> - each element of array A is an integer within the range [0..1,000,000,000].

<br/>

### 코드 작성

```python
def solution(A):
    peak = []
    for p in range(1, len(A)-1):
        if A[p-1] < A[p] and A[p] > A[p+1]:
            peak.append(p)

    print(len(peak))

    for k in range(len(peak)):
```

이까지 쓰고 도저히 생각이 안나서 구글링했다 ^^ ㅋㅋㅋ.... ㅜㅜㅜㅜㅜㅜ

<br/>

### 다른 코드

```python
def solution(A):
    # write your code in Python 3.6

    # peak가 있는 인덱스만 모아놓은 리스트 초기화
    peak_arr_list = []


    # peak가 있는 인덱스만 모아놓은 리스트 값 채워넣기
    for i in range(1,len(A)-1):
        if A[i] > A[i-1] and A[i] > A[i+1]:
            peak_arr_list.append(i)

    # 만약 peak가 0개라면 return 0
    if len(peak_arr_list) == 0:
        return 0

    # 만약 peak가 1개라면 return 1
    if len(peak_arr_list) == 1:
        return 1       

    # peak_arr_list 출력 테스트
    #print(peak_arr_list)

    # peak_arr 라는 리스트 초기화
    peak_arr = [0]*len(A)
    peak_arr[0] = -1
    peak_arr[-1] = -1
    peak_idx = -1

    # peak_arr 값 채워넣기
    for i in range(len(A)-2,0,-1):
        if A[i] > A[i-1] and A[i] > A[i+1]:
            peak_idx = i

        peak_arr[i] = peak_idx

    # peak_arr 출력 테스트
    #print(peak_arr)

    max_num_flag = -1

    # 1에서 len(A)까지 반복 -> 최대 peak 개수는 len(A)가 될 수 있음
    for i in range(1,len(peak_arr_list)+1):

        # index는 매 loop마다 1값으로 초기화
        index = 1

        # num_flag는 매 loop마다 peak 조건을 만족하면 +1씩 해줌
        num_flag = 0

        # num_flag가 loop보다 작으면 반복 -> 같아지면 해당 num_flag는 가능한 개수이다.
        while num_flag < i:
            try:
                if peak_arr[index] != -1:
                    num_flag += 1
                    index = peak_arr[index] + i
                else:
                    return max_num_flag
            except:
                return max_num_flag

        # 만약 이전 loop의 peak 개수보다 크면 max_num_flag 초기화
        max_num_flag = max(num_flag,max_num_flag)

    # for 끝나면 가장 최대 peak 개수를 의미하는 max_num_flag 반환
    return max_num_flag

    pass
```

[다음 블로그](https://datacodingschool.tistory.com/71?category=391801)의 강의를 보고 이해했다. 정말 최고의 강의... 이렇게 생각할 수도 있구나!와 이해할 수 있어서 정말 좋았어 ㅋㅋ 시간 복잡도는 O(N)

<br/>

```python
import math

def solution(A):
    peak = []

    for p in range(1, len(A)-1):
        if A[p-1] < A[p] and A[p] > A[p+1]:
            peak.append(p)

    if len(peak) == 0:
        return 0

    if len(peak) == 1:
        return 1

    # print(peak)

 # 제일 처음 peak와 끝 peak 사이에 최대한 깃발을 꽂을 수 있는 갯수
    maxflag = int(math.sqrt(peak[-1]-peak[0]))+1

    # print(maxflag)

    # 제일처음 peak에서부터 조건을 비교하면서, count로 체크한 후 flag 수(f)를 return
    for f in range(maxflag, 2, -1):
        # print(f)
        count = 1
        p = peak[0]
        for i, pe in enumerate(peak):
            if f <= pe-p:
                count += 1
                p = pe
            # print("p:", p)
            # print("count:" ,count)

        if count >= f:
            break

    return f
```

기존 내가 쓴 코드에 [또 다른 블로그](https://smecsm.tistory.com/232?category=922260)를 참고해서 적어봤는데 93%의 성공률과 `The following issues have been detected: runtime errors. For example, for the input [0, 0, 0, 0, 0, 1, 0, 1, 0, 1] the solution terminated unexpectedly.` 오류가 발생하였다. 왤까?

그리고 `range(maxflag, 2, -1)`를 하면 문제와 같은 조건에서 4와 2가 나와야 하는 거 아냐? 근데 왜 print(f)를 하면 4와 3이 나오는 걸까........ 아 지금 알았다 range 함수 착각함 이거 2칸씩 띄우는게 아니라 처음과 끝이네,, 이 부분 다시 새기고 넘어가야지

<br/>

### 함수 정리

`range(start, stop, step)`

`try: ... except [발생 오류]: ...`



<br/>

### 느낀점

문제를 잘 풀고싶다... 조금 더 많이 오래 고민하자!
