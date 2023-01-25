---

layout: post

title: Baekjoon N과 M(8)

tags: DFS

---

[N과 M (8)](https://www.acmicpc.net/problem/15657)

### 문제

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

- N개의 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.
- 고른 수열은 비내림차순이어야 한다.
  - 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

<br/>

### 코드 구현

```python
import sys
input = sys.stdin.readline

def dfs(num, next):
    if num == m:
        print(*answer)
        return
    for i in range(next,n):
        answer[num] = k[i]
        dfs(num+1, i)

n, m = map(int, input().split())
k = sorted(list(map(int, input().split())))

answer = [0] * m

dfs(0,0)
```

많이 푸니까 왜 뻔하다고 하는지 알겠다 ㅋㅋㅠ 이 문제는 [Baekjoon N과 M(5)](https://suyeon12.github.io/2023/01/18/baekjoon-n과-m-5)에서 사용자 임의의 수열을, [Baekjoon N과 M(4)](https://suyeon12.github.io/2023/01/13/baekjoon-n과-m-4)에서 중복이 가능한 비내림차순 수열의 아이디어를 그대로 이용하면 된다

<br/>

### 다른 코드

```python
n, m = map(int, input().split())
nums = sorted(list(map(int, input().split())))
temp = []

def dfs(start):
    if len(temp) == m:
        print(*temp)
        return
    for i in range(start, n):
            temp.append(nums[i])
            dfs(i)
            temp.pop()

dfs(0)
```

`append`와 `pop` 활용

<br/>

### 느낀점

이제 `dfs`의 구조는 익숙해진 것 같다 ㅋㅋㅋ
