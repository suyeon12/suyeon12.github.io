## Lesson 1. Iterations - BinaryGap

### 문제

A *binary gap* within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps. The number 32 has binary representation 100000 and has no binary gaps.

Write a function:

> def solution(N)

that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5. Given N = 32 the function should return 0, because N has binary representation '100000' and thus no binary gaps.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..2,147,483,647].
<br/>
### 상황 분석

1. 숫자를 이진수로 변환한다

2.  1과 1사이에 있는 0의 개수를 세고 가장 큰 값을 반환한다


</br>
### 알고리즘

![algorithm]<img width="390" alt="스크린샷 2022-07-18 오후 11 17 19" src="https://user-images.githubusercontent.com/72901045/179539312-2eda421c-2772-496a-80ec-50d3780e4b40.png">


예시로 직접 확인해봤다
<br/>




### 코드 작성

```python
def solution(N):
    num = bin(N)
    binum = num[2:]

    count = 0
    max_zero = 0

    for i in binum:
        if i == '1':
            if count > max_zero:
                max_zero = count
                count = 0
            else:
                count = 0
        else :
            count += 1
    return max_zero
```

먼저 숫자를 이진수로 바꾸고 `bin()`함수를 쓰면 0b가 붙기 때문에 앞에 두 글자를 제외하였다. 이후에는 어떻게 할 지 좀 고민했는데 코드 이름이 반복인만큼 반복문을 써서 해결했다. 0이 나왔을 때는 count에 1을 추가하여 넘어가고 count 숫자와 max_zero 변수를 이용하여 최댓값을 구했다.
<br/>


### 다른 코드

```python
def solution(N):
  return len(max(bin(N)[2:].strip('0').strip('1').split('1')))
```

한 줄로 끝날 수 있다니... 넘 혁신적이다

사실 최댓값 구하래서 max를 생각하긴 했는데 어떻게 써야할 지 몰랐다 ㅎ;;


<br/>
### 함수 정리

`bin()` 이진수로 변환
ex. 10010001

`strip()` 인자로 전달된 문자를 string의 왼쪽과 오른쪽에서 제거

strip(0): 10010001
strip(1): 001000

`split()`  구분자로 문자열을 구분함

00/1/000 -> `max()`이기 때문에 000 -> `len()`-> 3


<br/>
### 느낀점

난 정말 코딩 못하는 구나 ^ㅡ^... 괜찮아 그저께 파이썬 시작했으니까~~!!! 이렇게 함수들 하나씩 알아가야겠다.




