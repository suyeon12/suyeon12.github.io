---

layout: post

title: Baekjoon DFS와 BFS

tags: DFS BFS

---

[DFS와 BFS](https://www.acmicpc.net/problem/1260)

### 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.

<br/>

### 코드 구현

```python
import sys
from collections import deque

input = sys.stdin.readline

def dfs(v):
    visited[v] = True
    print(v, end=' ')
    for i in graph[v]:
        if not visited[i]:
            dfs(i)

def bfs(v):
    queue = deque([v])
    visited[v] = True
    while queue:
        v = queue.popleft()
        print(v, end=' ')
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True


n, m, v = map(int, input().split())
graph = [[] for i in range(n+1)]
for i in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)
    graph[a].sort()
    graph[b].sort()

#print(graph)
visited = [False] * (n+1)
dfs(v)
print("")
visited = [False] * (n+1)
bfs(v)
```

먼저 코드 정리

```python
for i in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)
    graph[a].sort()
    graph[b].sort()
```

먼저 양방향이기 때문에 a와 b라는 정점 모두에서 연결한다. 이후 문제에서 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하라는 조건이 있기 때문에 `sort`를 추가했다. 이 부분이 이전에 풀었던 [Baekjoon 바이러스](https://suyeon12.github.io/2022/12/29/baekjoon-%EB%B0%94%EC%9D%B4%EB%9F%AC%EC%8A%A4)와 가장 다른 점

- 만약 sort를 수행하지 않았을 경우 예제 입력 2 출력 결과

        [[], [2, 3], [1, 5], [4, 1], [3, 5], [4, 2]]

- graph[a].sort()만

        [[], [2, 3], [5, 1], [1, 4], [5, 3], [2, 4]]

- graph[b].sort()까지

       [[], [2, 3], [1, 5], [1, 4], [3, 5], [2, 4]]

`dfs`와 `bfs`는 [DFS, BFS](https://suyeon12.github.io/2022/12/30/dfs-bfs)를 참고하였다

<br/>

### 다른 코드

```python
from collections import deque

# n=정점개수, m=간선개수, v=탐색시작점
N, M, V = list(map(int, input().split()))

# 인접영행렬
matrix = [[0]*(N+1) for i in range(N+1)]

#방문한곳체크기록할 리스트
visited_dfs = [0]*(N+1)
visited_bfs = [0]*(N+1)

# 입력받는 값에 대해 영형렬에 1삽입(인접리스트생성)
for i in range(M):
  a,b=map(int,input().split())
  matrix[a][b]=matrix[b][a]=1

def dfs(V):
  visited_dfs[V]=1
  print(V,end=' ')
  #재귀
  for i in range(1, N+1):
    if(visited_dfs[i]==0 and matrix[V][i]==1):
      dfs(i)

def bfs(V):
  #방문해야할 곳을 순서대로 넣을 큐
  queue=deque([V])
  visited_bfs[V]=1
  
  #큐안에 데이터없을때까지
  while queue:
    V=queue.popleft()
    print(V, end=' ')
    for i in range(1, N+1):
      if(visited_bfs[i]==0 and matrix[V][i]==1):
        queue.append(i)
        visited_bfs[i]=1

dfs(V)
print()
bfs(V)
```

`visited`를 dfs와 bfs로 나누니까 더 직관적임 그리고 `matrix[a][b]=matrix[b][a]=1`라고 한 거 신기하다. print 찍어보면 `[[0, 0, 0, 0, 0, 0], [0, 0, 1, 1, 0, 0], [0, 1, 0, 0, 0, 1], [0, 1, 0, 0, 1, 0], [0, 0, 0, 1, 0, 1], [0, 0, 1, 0, 1, 0]]` 이런 식으로 나옴

<br/>

### 느낀점

`bfs`는 처음 해보는데 역시??! 어렵다 ㅋㅋㅋ `sort`는 생각 못한 부분인데 배워서 좋았다




