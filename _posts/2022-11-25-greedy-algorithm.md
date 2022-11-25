---

layout: post

title: Greedy algorithm

summary: greedy algorithm을 예시를 통해 정리

tag: greedy

---

여러 경우 중 **하나를 결정해야 할 때마다 그 순간에 최적이라고 생각되는 것을 선택**해 나가는 방식으로 진행하여 최종적인 해답에 도달

선택의 순간마다 **당장 눈앞에 보이는 최적의 상황만**을 쫓아 최종적인 해답에 도달하는 방법

최적해를 구하는 데에 사용되는 **근사적**인 방법 -> 전체에서 항상 최적값을 구할 수 없기 때문



<br/>

### 조건

1. 탐욕 선택 속성
   
   이전의 선택이 이후에 영향을 주지 않음



2. 최적 부분 구조
   
   
   
   부분 문제의 최적 결과가 전체에도 그대로 적용



<br/>

### 예시

> 당신은 음식점의 계산을 도와주는 점원이다. 카운터에는 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리의 동전이 무한히 존재한다고 가정한다. 손님에게 거슬러줘야 할 돈이 N원일 때 거슬러 줘야 할 동전의 최소 개수를 구하라. 단, 거슬러 줘야 할 돈 N은 항상 10의 배수이다.



가장 대표적인 예시인 거스름돈 거스르기 문제이다.

```python
n = 1260
count = 0

# 큰 단위의 화폐부터 차례대로 확인하기
coin_types = [500, 100, 50, 10]

for coin in coin_types:
    count += n // coin # 해당 화폐로 거슬러 줄 수 있는 동전의 개수 세기
    n %= coin

print(count)
```

<br/>

- 정당성 분석

가지고 있는 동전 중 큰 단위가 항상 작은 단위의 배수라 작은 단위의 동전을 종합해 다른 해가 나올 수 없음

> if. 800원을 거슬러주는데 화폐 단위가 500원, 400원, 100원 이라면?

그리디의 경우 500원 1개, 100원 3개로 총 4개이나 400원 2개가 최적해이다.

<br/>



- codility

[Codility MaxNonoverlappingSegments](https://suyeon12.github.io/2022/11/21/codility-maxnonoverlappingsegments) 

```python
def solution(A, B):

    if len(A) == 0:
        return 0
    
    count = 1
    end = B[0]

    for i in range(len(A)):
        if end < A[i]:
            count += 1
            end = B[i]
    return count
```

각각 엔드포인트 기준으로 크고 작음을 판단함

<br/>

[Codility TieRopes](https://suyeon12.github.io/2022/11/22/codility-tieropes)

```python
def solution(K, A):
    count = 0
    length = 0
    for i in A:
        length += i
        if length >= K:
            count += 1
            length = 0
    return count
```



<br/>

### 참고자료

[그리디(Greedy) 알고리즘과 예제 (파이썬)](https://velog.io/@cha-suyeon/알고리즘-그리디Greedy-알고리즘과-예제-파이썬)

[그리디 알고리즘(Greedy Algorithm, 탐욕법)](https://velog.io/@kyunghwan1207/그리디-알고리즘Greedy-Algorithm-탐욕법)


