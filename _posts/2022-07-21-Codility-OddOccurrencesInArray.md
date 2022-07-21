---
layout: post

title: Codility OddOccurrencesInArray

summary: list[], pop(), insert()

tags: array

---

## Lesson2. Arrays - OddOccurrencesInArray

### 문제

A non-empty array A consisting of N integers is given. The array contains an odd number of elements, and each element of the array can be paired with another element that has the same value, except for one element that is left unpaired.

For example, in array A such that:

A[0] = 9 A[1] = 3 A[2] = 9
 A[3] = 3 A[4] = 9 A[5] = 7
 A[6] = 9

> - the elements at indexes 0 and 2 have value 9,
> - the elements at indexes 1 and 3 have value 3,
> - the elements at indexes 4 and 6 have value 9,
> - the element at index 5 has value 7 and is unpaired.

Write a function:

> def solution(A)

that, given an array A consisting of N integers fulfilling the above conditions, returns the value of the unpaired element.

For example, given array A such that:

A[0] = 9 A[1] = 3 A[2] = 9
 A[3] = 3 A[4] = 9 A[5] = 7
 A[6] = 9

the function should return 7, as explained in the example above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an odd integer within the range [1..1,000,000];
> - each element of array A is an integer within the range [1..1,000,000,000];
> - all but one of the values in A occur an even number of times.

 

### 상황 분석

이건 문제 분석부터 조금 애먹었다 ^^..

일단 홀수로 된 숫자들로 구성되어 있는 배열이 주어진다. 이 배열들의 값은 각각 짝이 지어지는데 여기서 짝이 지어지지 않는 숫자를 return하는 것이다.

일단 처음엔 어떻게 생각했냐면,, 검색해서 count한게 홀수가 나오는 경우의 값을 반환하자! 라고 생각했는데 당연히 안되고,, 다시 알고리즘을 짠 건 다음과 같다.

1. 배열을 순서대로 정렬한다.

2. 가장 첫번째 값과 가장 마지막에 있는 값을 더한다.

3. 더한 값이 짝수라면 넘어간다.

4. 더한 값이 홀수가 나올 경우 값들의 평균을 구한 후 소숫점을 버린다.

### 코드 작성

```python
import math

def solution(A):
    A.sort()
    ans = 0

    for i in A:
        if (A.pop() + A[i]) % 2 == 0:
            pass
        else:
            ans = math.trunc(A.pop()+A[i])
            break
    return ans
```

라고 했는데 당연히 안됐다~ ^^...

`if (A.pop() + A[i]) % 2 == 0:  IndexError: list index out of range`

다음과 같은 오류가 떴는데 당연함. range가 A기 때문에 그렇겠지.. 나는 홀수가 나오면 검색을 중단하고 바로 값을 return 하고 싶은데 저 코드는 그렇게 나오지 않는다. 사실 나처럼 구현한 사람이 있나? 싶어서 찾아봤는데 없더라^^...(물론 내가 못 찾았을수도) 사실 평균을 낸 후 값을 버린다는 것도 위 문제의 예시에는 잘 적용했는데 숫자가 엄청 커질 경우에도 그게 될까? 싶어서 결과를 제출하고 확인하고 싶었는데!!!! ... 이건 그 결과가 궁금해서라도 꼭 수정을 할 것이다,,

<br/>

### 다른 코드

```python
def test3(A):
    if len(A) == 1:
        return A[0]

    A = sorted(A)
    print(A)
    for i in range(0, len(A), 2):
        if i+1 == len(A):
            return A[i]
        if A[i] != A[i+1]:
            return A[i]
```

 `range(0, len(A), 2)` 라는 건 A의 길이까지 두 칸씩 이동하라는 뜻인데 이렇게 해도 값이 구해지구나. 신기하다. 어떤 원리일까..? 진짜 배열 기본 개념이 안 잡혀있으니까 실제로 되는 코드보고 신기해하기만 하고 어떻게 되는지는 잘 모르구나 ^^.... 이래서 자료구조가 필수인데,,  

<br/>

```python
def solution(A):
  return reduce(lambda x,y: x^y, A)
```

처음 이 코드 보고 느낀건... ???? 이렇게 해도 되는거야?? 였다. 저는 `lambda`함수를 처음 봤구요.. `^` 는 XOR 연산자이다. 결과값이 A에 저장되는 것 같은데 와 미쳤다. XOR 연산을 여기에 쓴다고...? 같은 수를 두 번 XOR 연산하면 없어진다니 ㅋㅋㅋ 근데 위치는 상관없다는 건 진짜 이 문제를 위한 연산 아닌가(?) 근데 이걸 딱 보고 어떻게 생각했지? 물론 너무 찰떡같은 연산이긴 하지만,, 진짜 최고같다.

<br/>

### 정리

`sort()` 는 정렬하는거고 `sorted()` 를 사용하면 정렬된 리스트를 반환해준다..!

`range(min, max, gap)` min부터 max-1까지 gap 만큼의 차이를 두고 숫자를 연속된 객체로 만들어 줌

`lambda 매개변수 : 표현식` 함수를 한 줄로 만들 수 있게 해준다

`XOR(^)` 두 값을 이진수로 바꾼 후 두 값의 각 자리수를 비교해서 같으면 0, 다르면 1을 계산한다. 이때 같은 수를 두 번 xor 연산하면 없어진다.

<br/>

### 느낀점

내가 쓴 코드 ... 어떻게든 수정해서 실행 되도록 한다,, 물론 결과가 영 꽝이라도... 근데 정말 나처럼 생각한 사람이 아무도 없는걸까? ㅜ 나중에 영어로도 한 번 검색해서 찾아봐야겠다. 또 제한시간 거의 끝나가기 전에 생각했는데 for문만 고집할게 아니라 반복문을 쓸 때 while도 좀 인지하고 있어야겠다!
