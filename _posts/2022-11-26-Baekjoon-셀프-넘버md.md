---

layout: post

title: Baekjoon 셀프 넘버

summary: sorted(), A.remove(), str()

---



[셀프 넘버](https://www.acmicpc.net/problem/4673)

### 문제

셀프 넘버는 1949년 인도 수학자 D.R. Kaprekar가 이름 붙였다. 양의 정수 n에 대해서 d(n)을 n과 n의 각 자리수를 더하는 함수라고 정의하자. 예를 들어, d(75) = 75+7+5 = 87이다.

양의 정수 n이 주어졌을 때, 이 수를 시작해서 n, d(n), d(d(n)), d(d(d(n))), ...과 같은 무한 수열을 만들 수 있다. 

예를 들어, 33으로 시작한다면 다음 수는 33 + 3 + 3 = 39이고, 그 다음 수는 39 + 3 + 9 = 51, 다음 수는 51 + 5 + 1 = 57이다. 이런식으로 다음과 같은 수열을 만들 수 있다.

33, 39, 51, 57, 69, 84, 96, 111, 114, 120, 123, 129, 141, ...

n을 d(n)의 생성자라고 한다. 위의 수열에서 33은 39의 생성자이고, 39는 51의 생성자, 51은 57의 생성자이다. 생성자가 한 개보다 많은 경우도 있다. 예를 들어, 101은 생성자가 2개(91과 100) 있다. 

생성자가 없는 숫자를 셀프 넘버라고 한다. 100보다 작은 셀프 넘버는 총 13개가 있다. 1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, 97

10000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 출력하는 프로그램을 작성하시오.



<br/>

### 알고리즘

모든 인덱스에서 생성자가 안 나오는 걸 빼면 어떨까? 이걸로 시작했습니다,,

<br/>

### 코드 작성

```python
def selfnumber(n):
    sum = n
    
    while n != 0:
        sum += (n % 10) 
        n = n // 10

    return sum


answer = []
allnumber = []

for i in range(10000):
    number = selfnumber(i)
    if number <= 10000:
        answer.append(number)

for i in range(10000):
    allnumber.append(i)
    
selfnumbers = sorted(set(allnumber) - set(answer))

for i in selfnumbers:
    print(i)
```

1. 먼저 생성자를 구함

2. 생성자들을 `answer` 이 배열에 넣음(이거 이름 마음에 안 든다..)

3. 10000 까지의 모든 숫자가 있는 `allnumber` 배열에서 `answer` 를 차집합 한 후 정렬

4. `selfnumbers`의 모든 요소를 print



<br/>

### 다른 코드

```python
numbers = list(range(1, 10_001))
remove_list = []  # 이후에 삭제할 숫자 list
for num in numbers :
    for n in str(num):
        num += int(n)  # 생성자가 있는 숫자
    if num <= 10_000:  # 10,000보다 작거나 같을 때만,
        remove_list.append(num)  # append: 리스트에 요소를 추가할 때

for remove_num in set(remove_list) :  # set 으로 중복값 제거
    numbers.remove(remove_num)
for self_num in numbers :  # 생성자가 있는 숫자를 삭제한 리스트
    print(self_num)
```

난 차집합으로 구했는데 여기에서는 `remove`를 이용함! 근데 나도 이렇게 좀.. 간단하게 쓰고 싶다~

<br/>

```python
# 1부터 10000까지 숫자 저장
num_set = set(range(1, 10001))

# 셀프 넘버가 아닌 수를 저장
not_self_num= set()

for i in range(1, 10001):
    for j in str(i): # 숫자를 문자열로 변환하여 102이면 1, 0, 2로 접근
        i += int(j)  # d(i) = i + j1 + j2 + j3 + .. = 102 + 1 + 0 + 2

    not_self_num.add(i) # 셀프 넘버가 아닌 수 저장

# 셀프 넘버만 있는 set = 전체 수 set - 셀프 넘버가 아닌 수 set
self_num = num_set - not_self_num

# 오름차순으로 정렬 후 출력
for i in sorted(self_num):
    print(i)
```

이건 숫자를 문자열로 변환해서 한다는 게 신기해서

<br/>

### 함수 정리

`%` 왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후, 그 나머지를 반환함

`//` 몫을 구하는 연산자(정확히 말하면 나누고 난 후 뒤 소수점 부분을 버림)

`sorted()`  정렬한 새로운 리스트를 반환

`A.remove(obj)` 배열에서 obj로 지정한 내용과 동일한 첫 항목을 찾아 삭제하고 나머지를 좌측으로 이동

`str()` 문자열로 바꿈



<br/>

### 느낀점

생성자에 대한 설명이 좀 부족한 것 같기도,.. 근데 항상 영어로 제목 쓰다가 한글 쓰려니 어색하다
