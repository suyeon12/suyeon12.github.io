---

layout: post

title: Codility MaxCounters

summary: enumerate(), max(), clear()

---

## Lesson 4. Counting Elements - MaxCounters

### 문제

You are given N counters, initially set to 0, and you have two possible operations on them:

> - *increase(X)* − counter X is increased by 1,
> - *max counter* − all counters are set to the maximum value of any counter.

A non-empty array A of M integers is given. This array represents consecutive operations:

> - if A[K] = X, such that 1 ≤ X ≤ N, then operation K is increase(X),
> - if A[K] = N + 1 then operation K is max counter.

For example, given integer N = 5 and array A such that:

A[0] = 3
 A[1] = 4
 A[2] = 4
 A[3] = 6
 A[4] = 1
 A[5] = 4
 A[6] = 4

the values of the counters after each consecutive operation will be:

(0, 0, 1, 0, 0)
 (0, 0, 1, 1, 0)
 (0, 0, 1, 2, 0)
 (2, 2, 2, 2, 2)
 (3, 2, 2, 2, 2)
 (3, 2, 2, 3, 2)
 (3, 2, 2, 4, 2)

The goal is to calculate the value of every counter after all operations.

Write a function:

> def solution(N, A)

that, given an integer N and a non-empty array A consisting of M integers, returns a sequence of integers representing the values of the counters.

Result array should be returned as an array of integers.

For example, given:

A[0] = 3
 A[1] = 4
 A[2] = 4
 A[3] = 6
 A[4] = 1
 A[5] = 4
 A[6] = 4

the function should return [3, 2, 2, 4, 2], as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N and M are integers within the range [1..100,000];
> - each element of array A is an integer within the range [1..N + 1].

<br/>

### 상황 분석

<img width="518" alt="스크린샷 2022-08-09 오전 12 41 16" src="https://user-images.githubusercontent.com/72901045/183457900-1ea8ff9d-0731-490b-8b4a-9bc15bd9cb05.png">

문제 이해만 해도 15분 정도 걸린듯^ㅡ^..

1과 N 사이에 있으면서 A[k]가 N의 숫자들과 같다면 카운터에 1을 추가한다. 만약 A[K]가 N+1이라면 그 전의 카운터 값 중 가장 큰 값으로 전체가 대체된다.

<br/>

### 코드 작성

```python
def solution(N, A):
    count = [0] * N
    for k in A:
        if A[k] == N:
            count[k] += 1
            if A[k] == N+1:
                count = max(count) * N
    
    return count
```

이렇게 썼다가 계속 [0,0,0,0,0] 뜨죠 ㄱ-

<br/>

```python
def solution(N, A):
    count = [0] * N
    for k in A:
        if 1 <= k <= N:
            count[k-1] += 1
        else:
            count = [max(count)] * N
    
    return count
```

이리저리 만지다가 나온 코드인데 솔직히.. 내가 썼지만(?) 이해를 못 하겠다. 사실 내가 백퍼 썼다고 하기도 그런게 내가 쓴 게 아니라 다른 사람이 쓴 코드에 영향 받은게 확실하니까,, ㅜ 어쨌거나 완성도는 66퍼로 시간복잡도가 O(N*M)이라 일어난 일인데,, 다른 분들이 쓴 코드를 보자~ ㅜㅜㅜ...

<br/>

### 다른 코드

```python
def solution(N, A):
    counters = [0 for _ in range(N+1)]
    # 임시 최대값
    temp_mx = 0
    # 저장될 최대값
    mx = 0
    
    for x in A:
        # N+1 일 땐 저장될 최대값을 갱신해준다.
        if x == N+1 :
            mx = temp_mx
        # N+1보다 작을 경우 
        else :
            # x 이전의 경우에 N+1이 나왓다면 mx가 갱신되었을 것이고 아니면 0이므로 1만 증가시켜준다.
            if counters[x] < mx :
                counters[x] = mx + 1
            # counters[x]가 mx보다 크다면 1만큼만 올려야한다. 이미 최대값인데 mx+1 만큼 증가될 수 있다. 
            else:
                counters[x] += 1
            # counters[x] 가 부분 최대값보다 크다면 갱신해준다.
            if counters[x] > temp_mx :
                temp_mx = counters[x]
            
    # 만약 N+1 이 나온 이후에 증가된 부분이 있다면 위에 if문에서 이미 mx + 1 로 처리가 되었다. 
    # 즉 mx보다 작은 모든 값은 N+1이 나온 이후에 갱신된 적이 없다는 뜻이므로 전체를 += mx가 아니라 = mx로 갱신하면 된다.
    for i in range(1, len(counters)):
        if counters[i] < mx :
            counters[i] = mx
    return counters[1:]

```

주석이랑 같이 보니까 알듯말듯하다. temp_mx는.. 어떻게 쓰는거지? 이거 실제로 작동되는 과정을 디버그처럼 알려주면 좋겠다.....

<br/>

```python
def fast_solution(N, A):
    counters = [0] * N
    max_counter = 0
    last_update = 0

    for K,X in enumerate(A): # O(M)
        if 1 <= X <= N:
            counters[X-1] = max(counters[X-1], last_update)
            counters[X-1] += 1
            max_counter = max(counters[X-1], max_counter)
        elif A[K] == (N + 1):
            last_update = max_counter

    for i in xrange(N): # O(N)
        counters[i] = max(counters[i], last_update)

    return counters
```

풀었던 것에서 시간복잡도를 개선한 코드를 발견하였다. 

The secret is **not to update all the counters every time** you get the instruction to bump them all up to a new minimum value. This incurs an operation involving every counter on every occasion, and is the difference between a ~60% score and a 100% score. Instead, **avoid this hit by keeping track of the current minimum and maximum values**; using and updating them for each counter you visit. Then, after all the instructions are processed, because there may be counters which haven't been touched with their own personal update since **the last update-all instruction, pass over the counters themselves and ensure they are at the minimum value.** ([python - What&#39;s wrong with this solution for Max Counters codility challenge - Stack Overflow](https://stackoverflow.com/questions/20506849/whats-wrong-with-this-solution-for-max-counters-codility-challenge)[python - What&#39;s wrong with this solution for Max Counters codility challenge - Stack Overflow](https://stackoverflow.com/questions/20506849/whats-wrong-with-this-solution-for-max-counters-codility-challenge))

아 어렵다.. 코드만 보면 얼핏 알 것 같기도한데 예시로 코드 따라 디버그해보라고 하면 잘 모르겠다. 일단 변수 두 개를 써서 해야하는 건 분명해보인다,,

<br/>

```python
def solution(N, A):
    
    counters = [0] * N
    max_numbers = set()
    
    for i in range(0, len(A)):

        if 1 <= A[i] <= N:

            index = A[i]-1
            counters[index] += 1
            max_numbers.add(counters[A[i]-1])

        elif A[i] == N + 1 and len(max_numbers) > 0:

            counters = [max(max_numbers)] * N
            max_numbers.clear()

    return counters
```

여기서도 집합 개념을 쓸 수 있는게 매우 신기하다. A[i]를 사이에 넣어도 코드 작동이 되나?

<br/>

### 함수 정리

`enumerate()` 리스트 등의 요소를 인덱스(카운터)와 함께 얻을 때 사용

`max()` 매개변수로 들어온 반복이 가능한 인자들 중 가장 큰 데이터 반환

`clear()` 리스트의 모든 요소 제거

<br/>

### 느낀점

처음으로 medium 문제 풀어본건데 정말 매우 어렵다..! ^ㅡ^ 다른 분들이 쓰신 코드 읽음 거의 이해 가능했었는데 오늘은.. 좀 어렵네,,


