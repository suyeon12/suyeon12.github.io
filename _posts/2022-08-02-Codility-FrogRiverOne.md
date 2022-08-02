---

layout: post

title: Codility FrogRiverOne

summary: defaultdict(), enumerate(), set()

---



## Lesson 4. Counting Elements - FrogRiverOne

### 문제

A small frog wants to get to the other side of a river. The frog is initially located on one bank of the river (position 0) and wants to get to the opposite bank (position X+1). Leaves fall from a tree onto the surface of the river.

You are given an array A consisting of N integers representing the falling leaves. A[K] represents the position where one leaf falls at time K, measured in seconds.

The goal is to find the earliest time when the frog can jump to the other side of the river. The frog can cross only when leaves appear at every position across the river from 1 to X (that is, we want to find the earliest moment when all the positions from 1 to X are covered by leaves). You may assume that the speed of the current in the river is negligibly small, i.e. the leaves do not change their positions once they fall in the river.

For example, you are given integer X = 5 and array A such that:

A[0] = 1
 A[1] = 3
 A[2] = 1
 A[3] = 4
 A[4] = 2
 A[5] = 3
 A[6] = 5
 A[7] = 4

In second 6, a leaf falls into position 5. This is the earliest time when leaves appear in every position across the river.

Write a function:

> def solution(X, A)

that, given a non-empty array A consisting of N integers and integer X, returns the earliest time when the frog can jump to the other side of the river.

If the frog is never able to jump to the other side of the river, the function should return −1.

For example, given X = 5 and array A such that:

A[0] = 1
 A[1] = 3
 A[2] = 1
 A[3] = 4
 A[4] = 2
 A[5] = 3
 A[6] = 5
 A[7] = 4

the function should return 6, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N and X are integers within the range [1..100,000];
> - each element of array A is an integer within the range [1..X].

<br/>

### 상황 분석

문제를 이해하기까지 시간이 조금 걸렸다. 요약하자면 먼저 배열 A가 주어진다. 이는 값을 갖고 있는데 X로 주어진 값까지(X가 5라면 1,2,3,4,5) 모두 다 나온 최초의 인덱스를 반환하면 된다. 괜히 개구리가 건너편을 건너고 어쩌고해서 좀 헷갈렸다..

<br/>

### 코드 작성

```python
def solution(X, A):
    ans = []

    for i in range(1,X+1):
        ans.append(i)
    
    for i in range(len(A)):
        if A[i] in ans:
            ans.remove(A[i])
            if len(ans) == 0:
                return i
    
    return -1
```

물론.. 시간복잡도가 n^2 라서 비효율적인 코드이긴한데 나름대로 배운 게 있어서 나름..! 만족스럽다. 물론 63%의 완성도를 가진 코드이긴 하지만... ^ㅡㅜ 수정 하기전에는 0%(예시 코드에서도 런타임 에러가 났었다...), 36%(UnboundLocalError: local variable~ )였으니 뭐.. 허허허 조금 더 노력해서 100%까지 채워보기로 >.<

일단 맨 처음에 A[i] == ans[i] 이런 식으로 비교했었는데 아예 내가 생각해보지 않았다는 가정하에 다시 풀어보니까

<img width="626" alt="스크린샷 2022-08-02 오후 9 19 47" src="https://user-images.githubusercontent.com/72901045/182373079-2b2f0aa5-a6ce-43d9-adfc-b33e656f5934.png">

에러.. 날만 하더라고 ^^ㅋㅋㅋ 그래서 리스트 값 비교를 검색해서 찾아보던 중 in을 발견할 수 있었다. 예전에도 리스트 범위 벗어나서 틀린게 있었는데 이런 식으로 접근해봐도 되지 않을까...

두 번째는 원래 return i가 아니라 answer = i 한 후 return i를 했었는데 이러니 위에 answer 추가해줘야하고 어차피 대입하는거.. 그냥 i로 하자고 한 후에는 일단 Correctness test는 다 만족했다. 이제 할 건 Performance test인데 ^^... 다시 봐도 정말 극악의 성능이다 ㅎ.. 일단 다른 분들 코드를 보고 어떻게 하셨는지 한 번 보기로~

<br/>

### 다른 코드

```python
import collections

def solution(X, A):
    # write your code in Python 3.6
    count = collections.defaultdict(int)

    for i, n in enumerate(A):
        count[n] = i
        if len(count) == X:
            return i

    return -1
```

`defaultdict()` 는 collections에 있는 것으로 딕셔너리를 만드는 dict클래스의 서브클래스이다.

`enumerate()` 는 처음 봤는데 파이썬답게 인덱스와 원소에 동시에 접근하며 루프를 돌릴 수 있게 해준다고 한다. 인자로 넘어온 목록을 기준으로 인덱스와 원소를 차례대로 접근하게 해주는 반복자 객체를 반환해주는 함수인 것이다.  [다음 블로그](https://www.daleseo.com/python-enumerate/) 에서 자세한 내용을 확인할 수 있다. 내가 위에 쓴 in을 쓰면 시간 복잡도가 O(n)이 되는데 for 안에 O(n)을 쓰면 O(n^2)이 되어 실패 확률이 높아진다(바로 나^ㅡㅜ.) 근데 count되는 딕셔너리의 키라면 O(1)의 해쉬맵핑으로 접근할 수 있어 O(n)의 시간복잡도를 유지할 수 있다고 한다. - [참고 자료](https://davi06000.tistory.com/52) 

딕셔너리에 관한 이해가 조금 더 필요할 것 같다!

<br/>

```python
def solution(X, A):
    check = [0] * X
    check_sum = 0
    for i in range(len(A)):
        if check[A[i]-1] == 0:
            check[A[i]-1] = 1
            check_sum += 1
            if check_sum == X:
                return i
    return -1

```

여기서는 check_sum을 이용하여 A in B 검색을 피했다.

<br/>

```python
def solution(X, A):
    # write your code in Python 3.6
    leaf_left = set()
    for i in range(len(A)):
        leaf_left.add(A[i])
        if len(leaf_left) == X:
            return i
    return -1
```

이 코드 되게 직관적이고 좋다. 중복을 허용하지 않고 순서가 없는 집합이 여기 조건과 잘 맞아 떨어진 것 같다!!

<br/>

### 함수 정리

`range(len(A))`

`defaultdict()`  collections에 있는 것으로 딕셔너리를 만드는 dict클래스의 서브클래스

`enumerate()` 인자로 넘어온 목록을 기준으로 인덱스와 원소를 차례대로 접근하게 해주는 반복자 객체를 반환해주는 함수

`set()` 집합 자료형을 만드는 함수

<br/>

### 느낀점

시간 제약 없이 여유롭게 하니 좋구나,, 그리고 저번에 써 둔 array 포스트가 확실히 도움이 된 것 같다! 그때 함수 하나하나 쓸 때는 힘들었지만 ㅎ.. 오늘은 처음 보는 함수를 많이 알아서 여기에 대해 정리해봐야겠다


