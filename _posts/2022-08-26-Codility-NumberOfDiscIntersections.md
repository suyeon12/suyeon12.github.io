---

layout: post

title: Codility NumberOfDiscIntersections

summary: A.append((x,y)), bisect.bisect()

tag: sort

---

## Lesson 6. Sorting - NumberOfDiscIntersections

### 문제

We draw N discs on a plane. The discs are numbered from 0 to N − 1. An array A of N non-negative integers, specifying the radiuses of the discs, is given. The J-th disc is drawn with its center at (J, 0) and radius A[J].

We say that the J-th disc and K-th disc intersect if J ≠ K and the J-th and K-th discs have at least one common point (assuming that the discs contain their borders).

The figure below shows discs drawn for N = 6 and A as follows:

A[0] = 1
 A[1] = 5
 A[2] = 2
 A[3] = 1
 A[4] = 4
 A[5] = 0

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/number_of_disc_intersections/static/images/auto/0eed8918b13a735f4e396c9a87182a38.png)

There are eleven (unordered) pairs of discs that intersect, namely:

> - discs 1 and 4 intersect, and both intersect with all the other discs;
> - disc 2 also intersects with discs 0 and 3.

Write a function:

> def solution(A)

that, given an array A describing N discs as explained above, returns the number of (unordered) pairs of intersecting discs. The function should return −1 if the number of intersecting pairs exceeds 10,000,000.

Given array A shown above, the function should return 11, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - each element of array A is an integer within the range [0..2,147,483,647].

<br/>

### 상황 분석

<img width="319" alt="스크린샷 2022-08-26 오후 9 04 38" src="https://user-images.githubusercontent.com/72901045/186899758-d888c7cb-cc55-4b47-a336-21d550eeebda.png">

수직선 상으로 표현하면 조금 더 이해하기 쉽다. A[0] = 1이므로 0에서 1을 빼고 더한 만큼의 범위를 갖는다. 마찬가지로 A[1] = 5는 1에서 5을 빼고 더한만큼의 범위를 갖는다. 근데 나는 여기서 교차한다는 걸 어떻게 구현해야 할 지 조금 헷갈렸다 ^ㅡㅜ..

<br/>

### 알고리즘

<img width="557" alt="스크린샷 2022-08-26 오후 9 09 58" src="https://user-images.githubusercontent.com/72901045/186900492-1323049f-7fe5-4daf-85a9-07f59cb12f12.png">

솔직히 잘 모르겠어서 조금 검색해서 힌트 얻고 다시 고민했다는 걸 부정하진 못할 것 같다.. 일단 저 수직선 산은 시작 값 순서대로 표현한 부분이다. A1 입장에서 만나는 것은 A0, A2, A4, A3, A5 5개이다. A0에서 만나는 것은 A2, A4이다. 이렇게 생각하고 나니 이전에 풀었던 [PassingCars](https://suyeon12.github.io/2022/08/11/codility-passingcars) 문제가 떠올라서 이 느낌으로 인덱스를 주어 풀어봤는데 리스트는 그게 안 되더라고? 컴파일 에러가 떴다 ㅎ...

<br/>

### 코드 작성

```python
def solution(A):
    first = []
    last = []

    for i in range(len(A)):
        first.append(i - A[i])
        last.append(i+A[i])

    first.sort()
    last.sort()

    open_discs = 0
    intersections = 0
    k = 0

    for i in range(len(A)):
        for j in range(k, len(A)):
            if first[i] <= last[j]:
                intersections += open_discs
                if intersections > 10000000:
                    return -1
                open_discs += 1
                break
            open_discs -= min(1, open_discs)
            k += 1
    return intersections
```

도저히 모르겠어서 [다음 기사](https://medium.com/@deck451/codility-algorithm-practice-lesson-6-sorting-task-4-number-of-disc-intersections-a-python-378a296e760d)를 참고했다.

The approach I decided to use is detailed below:

- we calculate the start points and the end points for all discs. If **A[i]** is the radius and **i** is the disc center, then the starting point would be **i-A[i]**, while the ending point would be **i+A[i]**. These will go into two separate lists, which we will sort in ascending order. Note that we don’t really care about keeping any correspondence between the starting and ending point of a given disc;

-> 시작점과 끝점을 계산하고 오름차순으로 정렬된다는 것은 이해했다.

- we then iterate through the starting points list (**i**). For each starting point, we iterate through the ending points list (**j**), starting from a **k** index, which is initially set to 0;

-> 시작점 목록을 통해 반복(o). 0으로 시작된 k 인덱스에서 시작하여 끝점 목록(j)을 반복(??) k 인덱스가 왜 나오는 건지..

- if the **i-th** starting point is less than or equal to the **j-th** ending point, we call that **starting a disc**. We’ll then increase the number of intersections by the number of open discs. Should the number of intersections exceed 10,000,000, we’re going to have to return -1, as requested by the task. If not, we increment the number of open discs and we then immediately break off the second **for** loop, to save time;

-> i번째 시작점이 <= j번째 끝점, 디스크 시작. (근데 시작점은 거의 끝점보다 작거나 같지 않나요?? 시작점: -4, -1, 0, 0, 2, 5 끝점: 1, 4, 4, 5, 6, 8이니 -4는 모든 끝점보다 작거나 같고 -1도 그런 것 같은데 -4는 그렇다 치더라도 -1은 이렇게 나오면 안 되는 거 아닌가 ^ㅡㅜ..... )

-> first[0] < last[1...5] (6), first[1] < last[1..5] (6),  firt[2] < last[1..5] (6) 이까지만 해도 벌써 18개인데 아 이거... 맞는 건가? 정말로???

-> 교차로 수가 1,000,000을 초과하면 작업에서 요청한 대로 -1을 반환(O)

-> 그렇지 않은 경우 열려 있는 디스크의 수를 늘린 다음 시간을 절약하기 위해 즉시 두 번째 디스크를 루프용으로 분리(???)

- but if the **i-th** starting point is greater than the **j-th** ending point, we consider that we can’t close the **i-th** disk, but that we **close a disc**. As such, we decrement the number of open discs and then increment **k**. When we’ll have to inevitably increase **i**, the inner loop will not start from **k=0**, but from **k=1**, obviously, because a circle has just been closed. We are not done yet, because we still need to open the **i-th** circle, so we’ll just let the inner loop run its course to the next **j**;

-> 이 부분은 다 이해가 X...

O(N*log(N)) or O(N) 결과가 나왔기는 한데 솔직히 이거는 코드를 그대로 친 것과 다름이 없다. 이해를 못하겠음........ ㅜ

<br/>

### 다른 코드

```python
def solution(A):
    lis = []
    
    for i, a in enumerate(A):
        # -1은 왼쪽 원의 끝, +1은 오른쪽 원의 끝
        lis.append((i-a, -1))
        lis.append((i+a, 1))
    # 가장 왼쪽 기준(음수)에서 시작    
    lis.sort()
    # 교집합 수
    intersect = 0
    # 아직 열려있는 원(카운트 중인)의 수
    opened = 0
    
    for i in range(len(lis)):
        # 원의 오른쪽 끝 좌표만나면 열린 원의 개수 차감
        if lis[i][1] == 1:
            opened -= 1
        # 열린 원의 수만 큼 중첩해서 교집합 수 카운트
        # 원의 왼쪽 끝 좌표만나면 열린 원의 개수 증가
        if lis[i][1] == -1:
            intersect += opened
            opened += 1
    # 천만개 초과시에 -1 반환처리
    if intersect > 10000000:
        return -1
    
    return intersect
```

보다보니 느끼는건데 원이 열려있다는 개념을 잘 이해하지 못한 것 같다

<br/>

```python
import bisect

def solution(A):
    start = []
    end = []
    
    for i in range(len(A)):
        start.append(i-A[i])
        end.append(i+A[i])
        
    start.sort()    # 검색을 위해
    end.sort()      # 끝점 기준 정렬
    intersect = 0
    for i in range(len(A)):
        num = bisect.bisect(start, end[i])  #작은 끝점보다 왼쪽에 있는 시작점은 겹침(끝점은 이미 큰 상태)
        intersect += num - 1 - i # 자신 제외, 이전에 포함된 대상들 제외(이전에 겹쳤어도 -, 안겹쳤어도 - 필요)
        if intersect > 10000000:
            return -1
    return intersect
```

[다음 블로그](https://nukeguys.tistory.com/179)에서 가져왔는데 여기서 binary search요..? ㅋㅋ

<br/>

### 함수 정리

`A.append((x,y))` 2차원 배열 요소 삽입

``bisect.bisect()``  정렬된 리스트에 값을 삽입할 때 정렬을 유지할 수 있는 인덱스를 반환


<br/>

### 느낀점

오늘 포스팅에서 제일 많이 한 말: 이해가 안간다.. ㅋㅋㅋㅋㅜ 오랜만에 상황분석과 알고리즘 둘 다 적은 문제인데 너~~무 어렵다~!!! 
