---

layout: post

title: Baekjoon 2xn 타일링

tags: DP

---

[2×n 타일링](https://www.acmicpc.net/problem/11726)

### 문제

2×n 크기의 직사각형을 1×2, 2×1 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오.

아래 그림은 2×5 크기의 직사각형을 채운 한 가지 방법의 예이다.

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/11726/1.png)

<br/>

### 문제

```python
n = int(input())

dp = [1, 2]

for i in range(n):
  if n >= len(dp):
    for i in range(len(dp), i+1):
      dp.append(dp[i-1] + dp[i-2])
print(dp[-1]%10007)
```

전에 풀었던 [피보나치 함수](https://suyeon12.github.io/2022/12/28/baekjoon-피보나치-함수)를 참고해서 풀었는데 틀렸대.. 아니 왜? ㅋㅋㅋ 사실 식은 정말 빨리 구했는데 아 여기서 틀려서 ㅋㅋㅋ 테스트 케이스는 괜찮았어.... ㅜㅜ

<br/>

```python
n = int(input())

dp = [1, 2]

for i in range(2,n+1):
  dp.append(dp[i-1] + dp[i-2])

#print(dp)
print(dp[-2]%10007)
```

어쨌거나 최종본...!

<br/>

### 다른 코드

```python
n = int(input())
# memoization을 위함
cache = [0]*1001
# n이 1,2인 경우는 명확하니까 미리 선언해둔다.
cache[1]=1
cache[2]=2
# dynamic programming
for i in range(3,1001):
  cache[i] = (cache[i-1]+cache[i-2])%10007

print(cache[n])
```

여기서는 1001까지 아예 목록을 다 만들어놓음

<br/>

```python
n = int(input())

d = [0]*(n+2)
d[1] = 1
d[2] = 1

for i in range(3,n+2) :
    d[i] = d[i-1] + d[i-2]

print(d[n+1])
```

d[n+1]구조로!

<br/>

```python
n = int(input())
arr = [1] * n

if n >= 2:
    arr[1] = 2    
    for i in range(2, n):
        arr[i] = arr[i-1] + arr[i-2]

print(arr[-1]%10007)
```

내가 쓴 거랑 비슷함..

<br/>

### 느낀점

<img width="514" alt="스크린샷 2022-12-30 오후 6 04 47" src="https://user-images.githubusercontent.com/72901045/210053216-18a02688-01b5-49d4-baae-6bf4fc490c1c.png">

백준에서 많이 푼 순서대로 풀고 있는데 백준은 정말.. DP를 좋아해! ㅋㅋㅋㅋㅋㅋ 규칙 발견만 하면 되는거라 어렵지는 않다 오히려 재밌긴해...


