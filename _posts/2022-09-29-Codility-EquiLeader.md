---

layout: post

title: Codility EquiLeader

summary: counter, enumerate()

---

## Lesson 8. Leader - EquiLeader

### 문제

A non-empty array A consisting of N integers is given.

The *leader* of this array is the value that occurs in more than half of the elements of A.

An *equi leader* is an index S such that 0 ≤ S < N − 1 and two sequences A[0], A[1], ..., A[S] and A[S + 1], A[S + 2], ..., A[N − 1] have leaders of the same value.

For example, given array A such that:

A[0] = 4
 A[1] = 3
 A[2] = 4
 A[3] = 4
 A[4] = 4
 A[5] = 2

we can find two equi leaders:

> - 0, because sequences: (4) and (3, 4, 4, 4, 2) have the same leader, whose value is 4.
> - 2, because sequences: (4, 3, 4) and (4, 4, 2) have the same leader, whose value is 4.

The goal is to count the number of equi leaders.

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N integers, returns the number of equi leaders.

For example, given:

A[0] = 4
 A[1] = 3
 A[2] = 4
 A[3] = 4
 A[4] = 4
 A[5] = 2

the function should return 2, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [−1,000,000,000..1,000,000,000].



<br/>

### 다른 코드

```python
def solution(A):
    # write your code in Python 3.6
    candidate = []
    for i,v in enumerate(A):
        if len(candidate) == 0 or v == candidate[-1]:
            candidate.append(v)
        elif v != candidate[-1]:
            candidate.pop()
    
    try:
        count = A.count(candidate[0])
    except:
        count = 0
    
    if count <= len(A) // 2:
        return 0

    
    p1_len = 0
    p2_len = len(A)
    p1_count = 0
    p2_count = count
    equi = 0
    for i,v in enumerate(A):
        p1_len += 1
        p2_len -= 1
        if v == candidate[0]:
            p1_count += 1
            p2_count -= 1
        if p1_count > p1_len // 2 and p2_count > p2_len // 2:
            equi += 1
    return equi
```

그나마 이해할 수 있었던 코드 ^ㅡ^...

<br/>

```python
from collections import Counter

def solution(A):

    result = 0
    candidate, candidate_count  = Counter(A).most_common()[0]

    left = 0
    for idx, value in enumerate(A):
            
        if value == candidate:
            left += 1
            
        if left > (idx + 1) // 2 and (candidate_count - left) > (len(A) - (idx + 1)) // 2:
            result +=1

    return result

```

저번에 배웠던 `collections`를 이용했는데 문제는... `left > (idx + 1)`이랑 `(len(A) - (idx + 1)`` 이 부분 이해가 안 감 아니 왜요..ㅜㅜ

<br/>

```python
def solution(A):
    n = len(A)
    d = dict()
    lv = lk = 0
    for i in range(n):
        if A[i] not in d:
            d[A[i]] = 1
        else:
            d[A[i]] += 1
        if lv < d[A[i]]:
            lv = d[A[i]]
            lk = A[i]
    if lv <= n / 2:
        return 0
    left = 0
    right = lv
    count = 0
    for i in range(n):
        if A[i] == lk:
            left += 1
            right -= 1
        if left > (i + 1) / 2 and right > (n - i - 1) / 2:
            count += 1
    return count
```

`left > (i + 1) / 2 and right > (n - i - 1) / 2` 마찬가지로 왜? ㅜㅜ..

<br/>

```python
def solution(A):
    left, right = {}, {}
    left_length, right_length = 0, len(A)
    left_leader = 0
    cnt, left_cnt = 0, 0

    for num in A:
        try:
            right[num] += 1
        except KeyError:
            right[num] = 1

    for i in range(len(A)):
        right[A[i]] -= 1
        right_length -= 1

        try:
            left[A[i]] += 1
        except KeyError:
            left[A[i]] = 1

        left_length += 1

        if left[A[i]] > left_cnt:
            left_leader = A[i]
            left_cnt = left[A[i]]

        if right[left_leader] > right_length // 2 and left_cnt > left_length // 2:
            cnt += 1

    return cnt
```

`right[left_leader] > right_length // 2` 이건 알겠는데 `left_cnt > left_length // 2` 여긴 모르겠다..

<br/>

```python
def solution(A):
    # find leader of A
    st = []
    for a in A:
        if len(st) == 0 or st[-1] == a:
            st.append(a)
        else:
            st.pop()
    
    # there is no leader
    if len(st) == 0:
        return 0
    
    candidate = st[-1]
    leader_count = 0
    for a in A:
        if candidate == a:
            leader_count += 1
    
    # there is no leader
    if leader_count <= len(A) // 2:
        return 0

    leader = candidate
    
    # dp[x] : the number of leaders in A[0...x]
    dp = []
    cur_count = 0
    for a in A:
        if a == leader:
            cur_count += 1
        dp.append(cur_count)
    #print(dp)

    # N = len(A) - 1
    # head = [0...x], tail = [x+1...N]
    # the number of leaders in head : dp[x]
    # the number of leaders in tail : leader_count - dp[x]
    ans = 0
    for i in range(len(A) - 1):
        if dp[i] > (i + 1) // 2 and (leader_count - dp[i]) > (len(A) - 1 - i) // 2:
            ans += 1

    return ans
```

깔끔한 주석과 안 돌아가는 머리.. 하 자괴감 들어

<br/>

### 함수 정리

`Counter` 중복된 데이터가 저장된 배열을 인자로 넘기면 각 원소가 몇 번 나오는지 저장된 객체를 얻음

`Counter.most_common()` 데이터가 많은 순으로 정렬된 배열을 리턴

`enumerate()` 인자로 넘어온 목록을 기준으로 인덱스와 원소를 차례대로 접근하게 해주는 반복자 객체를 반환해주는 함수. [다음 블로그](https://www.daleseo.com/python-enumerate/)에서 자세한 내용을 확인할 수 있다

<br/>

### 느낀점

하나도 못 푼 문제는 처음인듯 근데 너~~무 아니... 진짜 이해를 못하겠다구ㅠ 좀 이해한 다음 쓰려고 했는데 코드 봄 -> 모르겠음 -> 며칠 봄 -> 포기 -> 다시 봄 -> 모르겠음... 의 반복이라 일단 이 문제는 다음으로 미루려고ㅜ 흑흑....~
