---

layout: post

title: Codility CountFactors

summary: range(), math.sqrt()

---

## Lesson 10. Prime and composite numbers - CountFactors

### 문제

A positive integer D is a *factor* of a positive integer N if there exists an integer M such that N = D * M.

For example, 6 is a factor of 24, because M = 4 satisfies the above condition (24 = 6 * 4).

Write a function:

> def solution(N)

that, given a positive integer N, returns the number of its factors.

For example, given N = 24, the function should return 8, because 24 has 8 factors, namely 1, 2, 3, 4, 6, 8, 12, 24. There are no other factors of 24.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..2,147,483,647].

<br/>

### 상황 분석

약수의 개수를 구하면 되는 귀여운 문제라고 생각했는데.. 큼큼;

<br/>

### 코드 작성

```python
def solution(N):
    count = 0
    for i in range(1,N+1):
        if N % i == 0:
            count += 1
    return count
```

5분 컷이군... 하고 결과를 봤는데 57% 성공률이였다;;; 엥? O(N)인데... 문제가 되나?? 하며 봤는데 뭐.... 성능 테스트에서 0점이라 ㅋㅋ ^ㅡㅜ...

<br/>

```python
import math

def solution(N):
    count = 0
    for i in range(1,int(math.sqrt(N))+1):
        if N % i == 0:
            if i ** 2 == N:
                count += 1
            else:
                count += 2
    return count
```

이건 좀 수학적 지식이 필요하다. 예를 들어 6의 약수는 1, 2, 3, 6으로 앞 두 개만 구한다면 뒤는 짝을 이룬다. 그래서 나도 반을 나눠볼까... 했는데 이럼 절대 안 맞음 ㅋㅋㅋ 예를 들어 10의 약수 개수랑 5의 약수 개수*2는 같을 수가 없지.. 슬쩍 고민하다,,, 그냥 구글링했다 ㅋ... 루트라는 신선한 발상이 있어서 이거에서 힌트 얻고 코드를 작성해봄

N이 100이라고 가정하자. 그럼 100의 루트인 10이되고 `range(1, 11)`의 for문이 돌아간다. `N % i == 0`에서 만족하는 i는 1, 2, 4, 5, 10이다. 여기서 1, 2, 4, 5는 짝을 이루고 있으니까 2개씩 더해주고 10은 하나니까 1만 더해주면 된다. 최종 시간복잡도는 `O(sqrt(N))`

<br/>

### 다른 코드

```python
def solution(N):
    result = 0
    for i in range(1,N+1):
        if i*i > N:
            break
        if i*i == N: 
            result += 1
            break
        if N % i == 0: 
            result += 2
    return result
```

이번 문제는 코드가 다 비슷한 것 같다 이건 보기 직관적이라 좋네

<br/>

### 함수 정리

`range(n, m)` n부터 m까지인데 m은 포함되지 않음

`math.sqrt(n)` n의 제곱근을 반환함

<br/>

### 느낀점

사실 시간 복잡도 줄이려고 생각할 때 약수의 개수 구하기..... 소인수분해를 해서 ,,, ..... ^^~ 암튼 처음엔 그렇게 생각했다고~


