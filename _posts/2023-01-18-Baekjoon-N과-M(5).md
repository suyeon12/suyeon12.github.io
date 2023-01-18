---

layout: post

title: Baekjoon N과 M(5)

summary: list(map(int, input().split())), join

tags: DFS

---

[N과 M (5)](https://www.acmicpc.net/problem/15654)

### 문제

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열

<br/>

### 코드 구현

```python
def dfs(num):
    if num == m:
        print(*answer)
        return
    for i in range(n):
        if visited[i] == True:
            continue
        #print(num, i, answer)
        answer[num] = k[i]
        visited[i] = True
        dfs(num+1)
        visited[i] = False

n, m = map(int, input().split())
k = sorted(list(map(int, input().split())))
#print(k)
visited = [False] * n
answer = [0] * m

dfs(0)
```

수열이 주어졌다는 점을 빼면 [Baekjoon N과 M(1)](https://suyeon12.github.io/2023/01/01/baekjoon-n과-m-1)과 비슷한 코드다 요즘 중복수열로 `visited`를 안 썼는데 오랜만에 쓰니 신선했음!

<br/>

### 다른 코드

```python
N, M = map(int, input().split())
L = list(map(int, input().split()))

L.sort()
visited = [False] * N
out = []

def solve(depth, N, M):
    if depth == M:
        print(' '.join(map(str, out)))
        return
    for i in range(N):
        if not visited[i]:
            visited[i] = True
            out.append(L[i])
            solve(depth+1, N, M)
            out.pop()
            visited[i] = False

solve(0, N, M)
```

`append`와 `pop`을 활용한 예시

<br/>

### 함수 정리

`list(map(int, input().split()))` 한 줄로 입력된 숫자들을 리스트로

`''.join()` String 사이에 특정 문자열을 삽입하여 나뉘어 있던 문자열을 합쳐주는 함수, List를 특정 구분자를 포함해 문자열로 변환하는 함수 ex. `','.join(a)`이라면 사이사이 쉼표를 넣어서 반환함

<br/>

### 느낀점

n과 m은 어디까지 있을까? 반복해서 쓰니까 확실히 연습은 된다 ㅋㅋ   
