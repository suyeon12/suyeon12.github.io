---

layout: post

title: Baekjoon N과 M(2)

summary: print(*arr), ''.join(), map(f, iterable)

tags: DFS

---

[N과 M (2)](https://www.acmicpc.net/problem/15650)

### 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
- 고른 수열은 오름차순이어야 한다.

<br/>

### 코드 구현

```python
def dfs(num, next):
  if num == m: # num과 m이 같다면 출력
    print(*answer)
    return
  for i in range(next, n):
    if visited[i] == True:
      continue 

    answer[num] = k[i]
    visited[i] = True # 체크
    #print(num, i, next)
    #print(answer)
    #print(visited)
    dfs(num+1, i+1) 
    visited[i] = False # 체크를 풀어줌

n, m = map(int, input().split())
k = [i+1 for i in range(n)]
answer = [0] * m
visited = [False] * n

#print(k)
dfs(0,0)
```

이전 [Baekjoon N과 M(1)](https://suyeon12.github.io/2023/01/01/baekjoon-n과-m-1)에서 오름차순 정렬 부분이 추가되었다. `print` 부분을 살펴보면 어떻게 구현했는지 더 상세히 알 수 있다

```
0 0 0
[1, 0]
[True, False, False, False]
1 1 1
[1, 2]
[True, True, False, False]
1 2
1 2 1
[1, 3]
[True, False, True, False]
1 3
1 3 1
[1, 4]
[True, False, False, True]
1 4
0 1 0
[2, 4]
[False, True, False, False]
1 2 2
[2, 3]
[False, True, True, False]
2 3
1 3 2
[2, 4]
[False, True, False, True]
2 4
0 2 0
[3, 4]
[False, False, True, False]
1 3 3
[3, 4]
[False, False, True, True]
3 4
0 3 0
[4, 4]
[False, False, False, True]
```

<br/>

### 다른 코드

```python
'''
1부터 n까지 자연수 중에 중복없이 m개
'''


def DFS(start):
    if len(s) == m:  # 해당 배열의 길이가 m과 동일해지면 프린트 후 리턴
        print(' '.join(map(str, s)))
        return

    # 반복문 반복시
    for i in range(start, n + 1):
        if i not in s:
            s.append(i)
            DFS(i+1)
            s.pop()


n, m = map(int, input().split())
s = []
DFS(1)
```

dfs의 매개변수가 하나다!

<br/>

```python
# 입력받기
import sys

input = lambda: sys.stdin.readline().rstrip()

n, m = map(int, input().split())
# 방문처리 리스트 초기화
visited = [False for _ in range(n)]
# 길이만큼 저장할 임시 리스트 생성
arr = []

def dfs(start, depth):
    if depth == m:
        print(' '.join(map(str, arr)))
    for i in range(start, n):
        if not visited[i]:
            visited[i] = True
            arr.append(i+1)
            # N과M(1)과 다른 부분은 이 부분입니다.
            # 이전에 저장된 수보다 큰 수를 저장하기 때문에 직전의 i+1을 해줍니다.
            # 그 결과로, depth가 m이 아닐 경우, arr에 저장된 수보다 큰 수만 저장됩니다.
            dfs(i+1, depth+1)
            visited[i] = False
            arr.pop()
dfs(0,0)
```

`visitied`로 점검하듯이 푸는 방식! append, pop을 쓴 것만 다르고 비슷해보인다

<br/>

### 함수 정리

`print(*arr)` 대괄호없이 한 번에 출력

`''.join()` String 사이에 특정 문자열을 삽입하여 나뉘어 있던 문자열을 합쳐주는 함수, List를 특정 구분자를 포함해 문자열로 변환하는 함수 ex. `','.join(a)`이라면 사이사이 쉼표를 넣어서 반환함

`map(f, iterable)` 함수(f)와 반복가능한(iterable) 자료형을 입력받고, 입력받은 자료형의 각 요소를 함수(f)가 수행한 결과를 묶어서 반환하는 함수

<br/>

### 느낀점

백트래킹 어렵다 ㅋㅋㅋㅋ 근데 이제 뭐가 뭔지 살짝 알 것 같기도 해..


