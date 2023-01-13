---

layout: post

title: Baekjoon N과 M(4)

tags: DFS

---

[N과 M (4)](https://www.acmicpc.net/problem/15652)

### 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.
- 고른 수열은 비내림차순이어야 한다.
  - 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<br/>

### 코드 구현

```python
def dfs(num, next):
    if num == m:
        print(*answer)
        return
    for i in range(next, n):
        answer[num] = k[i]
        #print(next, num, i)
        #print(answer)
        dfs(num+1, i)

n, m = map(int, input().split())
k = [i+1 for i in range(n)]
answer = [0] * m

dfs(0,0)
```

비내림차순(오름차순)의 아이디어는 [Baekjoon N과 M(2)](https://suyeon12.github.io/2023/01/05/baekjoon-n과-m-2), 중복은 [Baekjoon N과 M(3)](https://suyeon12.github.io/2023/01/11/baekjoon-n과-m-3) 이때 푼 방식으로 구현하면 된다

```
0 0 0
[1, 0]
0 1 0
[1, 1]
1 1
0 1 1
[1, 2]
1 2
0 1 2
[1, 3]
1 3
0 0 1
[2, 3]
1 1 1
[2, 2]
2 2
1 1 2
[2, 3]
2 3
0 0 2
[3, 3]
2 1 2
[3, 3]
3 3
```

<br/>

### 다른 코드

```python
def dfs(x): # 앞의 숫자와 비교해야하므로 파라미터로 넘겨줌
    if len(s)==m: # s의 길이가 m과 같으면 답 출력
        print(*s)
        return

    for i in range(x,n+1): # 파라미터로 받은 숫자보다 같거나 커야함
        s.append(i) # 수를 더해주고
        dfs(i) # 더한 수를 파라미터로 가지고 dfs
        s.pop() # 원 상태로 돌리기 위해 pop

n,m = map(int,input().split())
s = [] #답 리스트
dfs(1)
```

`append`와 `pop`~

<br/>

### 느낀점

구글링해서 2페이지까지는 찾아봤는데 나랑 완전히 똑같이 푼 사람은 없네 흠.. . . .




