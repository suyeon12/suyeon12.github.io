---

layout: post

title: Baekjoon N과 M(3)

summary: join

tags: DFS

---

[N과 M (3)](https://www.acmicpc.net/problem/15651)

### 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- 1부터 N까지 자연수 중에서 M개를 고른 수열
- 같은 수를 여러 번 골라도 된다.



<br/>

### 코드 구현

```python
def dfs(num):
  if num == m:
    print(*answer)
    return
  for i in range(n):
    answer[num] = k[i]
    dfs(num+1)

n, m = map(int, input().split())
k = [i+1 for i in range(n)]
answer = [0] * m

dfs(0)
```

- `3 2`를 입력했다고 가정
  
  num = 0, i = 0
  
  ans[0] = k[0] = 1 -> `ans = [1, 0]`
  
  `dfs(1)`
  
      num = 1, i = 0
  
      ans[1] = k[0] = 1 -> `ans = [1, 1]`
  
      `dfs(2)`
  
      => print(*answer) = `1 1`
  
      num = 1, i = 1
  
      ans[1] = k[1] = 2 -> `ans = [1, 2]`
  
      `dfs(2)`
  
       => print(*answer) = `1 2`

등등으로 전개된다

이전 [Baekjoon N과 M(1)](https://suyeon12.github.io/2023/01/01/baekjoon-n과-m-1)과의 차이점은 `visited`가 없다는 것!



<br/>

### 다른 코드

```python
n,m= map(int,input().split())
 
s = []
 
def dfs():
    if len(s)==m:
        print(' '.join(map(str,s)))
        return
    
    for i in range(1,n+1):
        s.append(i)
        dfs()
        s.pop()
dfs()
```

[이 블로그](https://velog.io/@hygge/Python-백준-15651-N과-M-3-Backtracking)를 참고해보면 `join`을 쓸 때 시간이 훨씬 단축된다고 한다! 오... 어쨌거니 여기는 `append`와 `pop`을 활용한 경우

<br/>

### 함수 정리

`''.join()` String 사이에 특정 문자열을 삽입하여 나뉘어 있던 문자열을 합쳐주는 함수, List를 특정 구분자를 포함해 문자열로 변환하는 함수 ex. `','.join(a)`이라면 사이사이 쉼표를 넣어서 반환함

<br/>

### 느낀점

중복 순열의 경우 `visited`로 체크하는 게 필요없다 ㅎㅡㅎ
