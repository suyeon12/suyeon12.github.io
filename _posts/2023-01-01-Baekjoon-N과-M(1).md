---

layout: post

title: Baekjoon N과 M(1)

summary: print(*arr), ''.join(), map(f, iterable)

tags: DFS

---

[N과 M (1)](https://www.acmicpc.net/problem/15649)

### 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

<br/>

### 알고리즘

- 백트래킹
  
  DFS를 기반으로 불필요한 경우를 배제하며 원하는 해답에 도달할 때까지 탐색하는 전략
  
  스택을 사용
  
  비효율적인 경로를 차단하고 목표지점에 갈 수 있는 가능성이 있는 루트를 검사하는 방법
  
  가지치기

<br/>

### 코드 구현

```python
def dfs(num):
  if num == m: # num과 m이 같다면 출력
    print(*answer)
    return
  for i in range(len(k)):
    if visited[i] == True:
      continue 
    # 만약 방문하지 않았다면
    answer[num] = k[i]
    visited[i] = True # 체크
    dfs(num+1) # dfs에 numdmf +1 하고 호출
    visited[i] = False # 체크를 풀어줌

n, m = map(int, input().split())
k = [i+1 for i in range(n)]
answer = [0] * m
visited = [False] * len(k)

#print(k)
dfs(0)
```

- `3 2`를 입력했다고 가정
  
  num = 0, i = 0
  
  ans[0] = k[0] = 1 -> `ans = [1, 0]`
  
  v[0] = True -> `v = [T, F, F]`
  
  `dfs(1)`
  
      num = 1, i = 1(i = 0일때는 v[0] = T라서 넘어감)
  
      ans[1] = k[1] = 2 -> `ans = [1, 2]`
  
      v[1] = True -> `v = [T, T, F]`
  
      `dfs(2)`
  
      => print(*answer) = `1 2`
  
      num = 1, i = 2
  
      ans[1] = k[2] = 3 -> `ans = [1, 3]`
  
      v[2] = True -> `v = [T, F, T]`
  
      `dfs(2)`
  
      => print(*answer) = `1 3`
  
  등등으로 전개된다
  
  ```python
    for i in range(len(k)):
      if visited[i] == True:
        continue
      answer[num] = k[i]
      visited[i] = True
      print(num, i)
      print(answer)
      print(visited)
  ```
  
  이랬을 때 결과는 다음과 같다. 보면 알겠지만 num이 1일때 출력된다.
  
  ```
  0 0
  [1, 0]
  [True, False, False]
  1 1
  **[1, 2]**
  [True, True, False]
  1 2
  **[1, 3]**
  [True, False, True]
  0 1
  [2, 3]
  [False, True, False]
  1 0
  **[2, 1]**
  [True, True, False]
  1 2
  **[2, 3]**
  [False, True, True]
  0 2
  [3, 3]
  [False, False, True]
  1 0
  **[3, 1]**
  [True, False, True]
  1 1
  **[3, 2]**
  [False, True, True]
  ```

<br/>

### 다른 코드

```python
import sys
input = sys.stdin.readline
 
N,M = map(int, input().split())
result = []
chk = [False] * (N+1) # 인덱스 값이 0이기 때문에 N보다 1크게 설정해주기(범위가 0부터 N-1이기 때문에 1~N까지 사용할 수 있도록 N+1만큼 곱해줌)
 
def recur(num):
    if num == M:
        print(' '.join(map(str, result)))   # result를 string으로 바꾼 후, 각각 배열에 있는 값을 한 칸 공백을 두고 join 해서 출력
        return
    for i in range(1, N+1):  # 위에 [False]를 N+1만큼 곱해준 것과 같은 원리
        if chk[i] == False:
            chk[i] = True
            result.append(i)
            recur(num+1)
            chk[i] = False
            result.pop()    # 마지막 값을 제거해줌
            
recur(0)
```

대부분 `append()`와 `pop()`을 쓰는 것 같다

<br/>

### 함수 정리

`print(*arr)` 대괄호없이 한 번에 출력

`''.join()` String 사이에 특정 문자열을 삽입하여 나뉘어 있던 문자열을 합쳐주는 함수, List를 특정 구분자를 포함해 문자열로 변환하는 함수 ex. `','.join(a)`이라면 사이사이 쉼표를 넣어서 반환함

`map(f, iterable)` 함수(f)와 반복가능한(iterable) 자료형을 입력받고, 입력받은 자료형의 각 요소를 함수(f)가 수행한 결과를 묶어서 반환하는 함수

<br/>

### 느낀점

저번에 [DFS, BFS](https://suyeon12.github.io/2022/12/30/dfs-bfs) 글을 올려서 순열문제를 `DFS`로 푼다는 걸 알았다 아니면 시작도 못했을듯 ㅜ.ㅜ 너무 어려워....






