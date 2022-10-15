---

layout: post

title: Codility MinPerimeterRectangle

summary: range(), math.sqrt(), math.ceil()

---

## Lesson 10. Prime and composite numbers - MinPerimeterRectangle

### 문제

An integer N is given, representing the area of some rectangle.

The *area* of a rectangle whose sides are of length A and B is A * B, and the *perimeter* is 2 * (A + B).

The goal is to find the minimal perimeter of any rectangle whose area equals N. The sides of this rectangle should be only integers.

For example, given integer N = 30, rectangles of area 30 are:

> - (1, 30), with a perimeter of 62,
> - (2, 15), with a perimeter of 34,
> - (3, 10), with a perimeter of 26,
> - (5, 6), with a perimeter of 22.

Write a function:

> def solution(N)

that, given an integer N, returns the minimal perimeter of any rectangle whose area is exactly equal to N.

For example, given an integer N = 30, the function should return 22, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..1,000,000,000].



<br/>

### 알고리즘

N이 30이라면 N의 약수는 [1, 2, 3, 5, 6, 10, 15, 30]이다. 여기서 중간인 [5, 6]이 A와 B에 해당하므로 약수 리스트에서 중간을 찾으면 된다.

<br/>

### 코드 작성

```python
def solution(N):
    number = []
    for i in range(1,N+1):
        if N % i == 0:
            number.append(i)
    # print(number)
    a = number[int(len(number)/2)]
    b = number[int(len(number)/2)-1]
    # print(a, b)
    return 2 * (a + b)
```

요즘은 테스트케이스 통과되서 제출하고도 매우 걱정되는데 역시...! ^^;;; `The following issues have been detected: wrong answers, timeout errors. For example, for the input 36 the solution returned a wrong answer (got 20 expected 24).` 로 40%의 성공률을 보였다. 아 알았어 다시풀게;;;;

<br/>

```python
import math

def solution(N):
    number = []
    for i in range(1,int(math.sqrt(N))+1):
        if N % i == 0:
            if i ** 2 == N:
                number.append(i)
            else:
                number.append(i)
    # print(number)
    b = int(N / number[-1])
    # print(b)
    return 2 * (number[-1] + b)
```

[저번에 푼 문제](https://suyeon12.github.io/2022/10/09/codility-countfactors)를 참고하였다. 근데 음... `number.append(i)`가 두 번 들어간게 살짝 신경쓰인다. 다시 해보니 `if i ** 2 != N: number.append(i)` 이렇게만 해도 될 것 같다 ㅋㅋㅋ

<br/>

```python
import math

def solution(N):
    number = []
    for i in range(1, int(math.sqrt(N))+1):
        if N % i == 0:
            if i ** 2 != N:
                number.append(i)
    b = int(N / number[-1])
    return 2 * (number[-1] + b)
```

더 깔끔하고 좋구나~ 코드 해석을 하자면 1부터 루트 N에 1을 더한 숫자까지 for문을 돌린다. 여기서 N을 i로 나눴을 때 나머지가 0인 i 중 i를 제곱할 때 N이 아닌 숫자들만 배열에 추가해준다. 만약 N이 30이라면 리스트에는 [1, 2, 3, 5]만 추가된다. 여기서 가장 마지막에 있는 값이 A이다! `A * B = N`이므로 B는 쉽게 구할 수 있다. 시간복잡도는 O(sqrt(N)) 나왔던 듯 하다.

<br/>

### 다른 코드

```python
def solution(N):
    # 가장 인접해 있는 factor 두개 찾기?
    import math
    sN = math.sqrt(N)
    
    for i in range(int(sN),0,-1):
        if N%i == 0:
            break
    
    return 2*(i + N//i)
```

비슷한데 더 짧아서 좋아보여..~

<br/>

```python
import math

def solution(N):
    div = int(math.ceil(math.sqrt(N)))
    while N % div != 0:
        div -= 1

    return int((N/div*2) + div + div)
```

div 이렇게 한 줄로 구하는 것도 정말 괜찮네 ㅋㅋㅋ 저 while 문에 들어가는 케이스가 궁금한데.. 근데 신기한 건 return 값이다 저렇게 구해도 원하는 값이 나오는게 무척 신기함 ㅋㅋㅋ 수학적으로 뭔가 있는 것 같은데!!

<br/>

### 함수 정리

`range(n, m)` n부터 m까지인데 m은 포함되지 않음

`math.sqrt(n)` n의 제곱근을 반환함

`math.ceil(n)` 올림 cf) `math.floor()`: 내림, `math.round()`: 반올림

<br/>

### 느낀점

이건 다른 말인데 지금... 카카오 관련 서비스가 안되서 티스토리 블로그도 접속이 안된다 ㅋㅋㅋ ㅜㅜㅜㅜㅜ,,,, 그래서 다른 코드 분석 불가능 흑흑... 아마 정상적으로 접근되면 나중에 다시 쓸 듯

아 그리고 number말고 리스트명을 약수로 하고 싶어서 언제나 사랑하는 파파고에 검색해봤는데,, mineral water ㅋㅋㅋㅋㅋㅋㅋ 아 그거 아니라고~~~!
