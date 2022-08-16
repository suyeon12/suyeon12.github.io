---

layout: post

title: Codility CountDiv

summary: divmod(), %, **, //

tags: Prefix-Sum

---

## Lesson 5. Prefix Sums - CountDiv

### 문제

Write a function:

> def solution(A, B, K)

that, given three integers A, B and K, returns the number of integers within the range [A..B] that are divisible by K, i.e.:

> { i : A ≤ i ≤ B, i **mod** K = 0 }

For example, for A = 6, B = 11 and K = 2, your function should return 3, because there are three numbers divisible by 2 within the range [6..11], namely 6, 8 and 10.

Write an ****efficient**** algorithm for the following assumptions:

> - A and B are integers within the range [0..2,000,000,000];
> - K is an integer within the range [1..2,000,000,000];
> - A ≤ B.

<br/>

### 코드 작성

```python
def solution(A, B, K):
    count = 0
    for i in range(A, B+1):
        if i % K == 0:
            count += 1
    return count
```

이거.. 답이 너무 보이는 것 아닌가? 하면서 4분만에 풀었는데 O(B-A)로 50%의 완성도를 지닌 코드였다 ^ㅡ^... 역시 아직까지는 가장 먼저 생각한 방법은 정답이 아닌 것 같다.

<br/>

```python
def solution(A, B, K):
    count1 = (A - 1) // K
    count2 = B // K
    return count2 - count1
```

아 이거.. 어떻게 풀지? 정확히는 어떻게 하면 효율적인 코드 작성이 가능할까.. 고민했는데 제목을 슬쩍 봤다. Prefix Sum.. 검색하니 구간합,,! 사실 젤 처음에는 (range A)하고 range(B+1) 한 다음 빼는 걸 생각해봤는데 음 이건 아니었고 ^^; 그리고 구간'합'이라길래 진짜 저걸 다 더하고 뭐 어케 하면 규칙을 발견할 줄 알았다. 그래서 숫자 가지고 이것저것 해봤는데 뭐 답은 아니었고,, 마지막으로는 개수에 주목했다. 다 일일히 하는 게 아니라,, B 까지랑 A 전까지 개수를 구한 후에 빼면..? 이라는 생각으로 접근해봤더니

![스크린샷 20220816 오후 8 52 14](https://user-images.githubusercontent.com/72901045/184874906-081edeee-46bf-4a29-aed9-73e9079fb33b.png)

꺅.. ㅋㅋㅋㅋ 시간복잡도 1은 처음 나오는 것 같다 짱 신기해 ~~

<br/>

### 다른 코드

```python
def solution(A, B, K):
    QA = A//K
    RA = A%K
    QB = B//K
    count = QB-QA
    if RA ==0:
        count +=1
    return count
```

비슷한데 A에서 나머지가 0일 경우를 추가해서 계산했다

<br/>

```python
def solution(A, B, K):
    # write your code in Python 3.6
    len_AB = B + 1 - A

    if len_AB < K:
        for n in range(A, B + 1):
            if n % K == 0:
                return 1

    div, mod = divmod(len_AB, K)
    # last chunk
    for n in range(A + div * K, B + 1):
        if n % K == 0:
            return div + 1
    return div
```

`divmod()` 쓴 게 좀 신기하다. 나도 사실 나눗셈 몫, 나머지 뭐 이런 이야기 나오면 divmod부터 생각나서 ㅎㅎ... 이후에 쓴 건 몫 위주로 쓴 것 같긴 하지만...

<br/>

### 정리

`divmod()` 매개변수로 두 개의 값을 입력받아 몫과 나머지를 계산하여 튜플 자료형 타입으로 반환

- 이번 기회에 하는 산술 연산자 정리

%: 나머지 (ex. 20 % 10 = 0)

**: 제곱 (ex. 10 ** 3 = 1000)

//: 몫 (ex. 10 // 3 = 3)

<br/>

### 느낀점

사실 나는 for문 한 번만 써서 시간복잡도가 n이니까 설마.. 설마?! 하면서도 통과될 줄 알았다 ㅎ.. 숫자가 매우매우 크게 주어지면 문제였음을,,~~ 산술 연산자 기본이지만 한 번 더 외우고 구간합 개념도 알아가기로..!
