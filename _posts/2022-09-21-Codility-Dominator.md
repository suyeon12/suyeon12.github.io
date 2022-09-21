---

layout: post

title: Codility Dominator

summary: get(), bisect, counter

---

## Lesson 8. Leader - Dominator

### 문제

An array A consisting of N integers is given. The *dominator* of array A is the value that occurs in more than half of the elements of A.

For example, consider array A such that

A[0] = 3 A[1] = 4 A[2] = 3
 A[3] = 2 A[4] = 3 A[5] = -1
 A[6] = 3 A[7] = 3

The dominator of A is 3 because it occurs in 5 out of 8 elements of A (namely in those with indices 0, 2, 4, 6 and 7) and 5 is more than a half of 8.

Write a function

> def solution(A)

that, given an array A consisting of N integers, returns index of any element of array A in which the dominator of A occurs. The function should return −1 if array A does not have a dominator.

For example, given array A such that

A[0] = 3 A[1] = 4 A[2] = 3
 A[3] = 2 A[4] = 3 A[5] = -1
 A[6] = 3 A[7] = 3

the function may return 0, 2, 4, 6 or 7, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - each element of array A is an integer within the range [−2,147,483,648..2,147,483,647].

<br/>

### 코드 작성

```python
def solution(A):
    dic = {}
    half = len(A) / 2

    for i in range(len(A)):
        if A[i] in dic:
            dic[A[i]] += 1
        else:
            dic[A[i]] = 1
        
        if dic[A[i]] > half:
            return i

    return -1
```

무난하군아.. `딕셔너리`를 이용해서 풀었고 만약 딕셔너리에 A[i] 값이 있다면 값을 하나 늘려주고 아니라면 1이라고 추가해준다. 딕셔너리의 A[i] 값이 반보다 크면 i를 return하고 아님 -1을 리턴한다. 시간복잡도는 O(N*log(N)) or O(N)

<br/>

### 다른 코드

```python
def solution(numbers):
    """Solution method implementation."""
    # set the threshold for dominator detection
    threshold = len(numbers) // 2
    # occurrences dictionary
    counts = {}
    # iterate through the list
    for i, number in enumerate(numbers):
        # update occurrences
        counts[number] = counts.get(number, 0) + 1
        # detect the dominator     
        if counts[number] > threshold:
            return i
    # no dominator
    return -1
```

다 비슷한데 get 쓴게 살짝 다르다.

<br/>

```python
def solution(A):
    stack = []
    for a in A:
        stack.append(a)
        while len(stack) > 1 and stack[-1] != stack[-2]:
            stack.pop(),stack.pop()
    if stack:
        candidate, total = stack[0], 0
        for index, a in enumerate(A):
            total += 1 if  a == candidate else 0
            if total > len(A)/2:
                return index
    return -1
```

pop 두 번 있는 거 오타인 줄 알았는데 테스트 해보니 아니네..? ㅋㅋㅋ `if stack:` 이런 식으로 써도 되는구나.. 

<br/>

```python
def solution(A):
    # write your code in Python 3.6
    if not A:
        return -1
    
    n = len(A)
    
    mid = n // 2
    
    a = sorted(A)
    d = None  # 도미네이터의 후보

    if n % 2 == 0:
        if a[mid] == a[mid-1]:
            d = a[mid]
    else:
        d = a[mid]
    
    if not d:
        return -1
    
    from bisect import bisect_left, bisect_right

    l_idx = bisect_left(a, d)
    r_idx = bisect_right(a, d)
    #print(r_idx, l_idx)

    if r_idx-l_idx >= (n//2)+1:
        return A.index(d)
    else:
        return -1
```

여긴 정렬을 사용한 게 독특했는데 dominator가 존재한다면 무조건 중심에 위치하게 될 것이라고 한다. 홀수 혹은 짝수일 때 `bisect`를 사용하였다. 이는 이진 탐색을 쉽게 구현하게끔 해주는 함수이다.

`bisect_left(literable, value)`: 왼쪽 인덱스를 구함

`bisect_right(literable, value)`: 오른쪽 인덱스를 구함

<br/>

```python
import collections
def solution(A):
    if len(A)==0:
        return -1
    # write your code in Python 3.6
    answer=collections.Counter(A).most_common(1)[0]
    if answer[1]>(len(A)//2):
        return A.index(answer[0])
    else:
        return -1
```

`Counter` 클래스는 중복된 데이터가 저장된 배열을 인자로 넘기면 각 원소가 몇 번 나오는지 저장된 객체를 얻는다고 한다. 아니,, 어떻게 이럴 수 있지? ㅋㅋㅋ 또한 `most_common()`은 데이터가 많은 순으로 정렬된 배열을 리턴한다고 한다.. 세상에...... 따라서 `answer=collections.Counter(A).most_common(1)[0]`은 가장 개수가 많은 걸 리턴한 거네,, 근데 여기서 `if answer[1]>(len(A)//2)` 이 부분이 신기하다. 왜 1일까?

<br/>

### 함수 정리

`get(x)` x라는 key에 대응되는 value를 돌려줌

`bisect_left(literable, value)` 왼쪽 인덱스를 구함

`bisect_right(literable, value)` 오른쪽 인덱스를 구함

`Counter`  중복된 데이터가 저장된 배열을 인자로 넘기면 각 원소가 몇 번 나오는지 저장된 객체를 얻음

`Counter.most_common()` 데이터가 많은 순으로 정렬된 배열을 리턴

<br/>

### 느낀점

파이썬을 파이썬답게 쓰고 싶다~
