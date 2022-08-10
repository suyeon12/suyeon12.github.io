---

layout: post

title: Codility MissingInteger

summary: sort(), sorted()

---

## Lesson 4. Counting Elements - MissingInteger

### 문제

This is a demo task.

Write a function:

> def solution(A)

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.

Given A = [−1, −3], the function should return 1.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [−1,000,000..1,000,000].

<br/>

### 알고리즘

<img width="563" alt="스크린샷 2022-08-10 오후 10 51 29" src="https://user-images.githubusercontent.com/72901045/183918765-7445e4a5-fc0d-4de9-bb35-3509fe4ab42a.png">

오늘도 참 많은 고민이 있었다 푸핫... 다음에 봐도 알 수 있도록 좀 정돈해서 쓰고 싶다 ^ㅡ^!!!

<br/>

### 코드 작성

```python
def solution(A):
    ans = sorted(set(A))
    if max(ans) < 0:
        return 1
    for i in ans:
        if ans[i] +1 != ans[i+1]:
            return ans[i] + 1
        else:
            return max(ans) + 1
```

처음에 이렇게 생각하고 풀었는데 계속 `max(ans)+1` 이 쪽으로 빠지는 거다. else 부분을 없애면 [1, 3, 6, 4, 1, 2]은 맞는데 [1, 2, 3]이 틀리고 전자가 틀리면 후자가 맞고.. 진짜 왜 이러냐?? 싶었는데 순서대로 다시 해보니,, 저기로 빠지는 게 맞았다 ㅎ 역시 코드는 죄가 없다 문제는 나일 뿐...^ㅡ^!!

<br/>

```python
def solution(A):
    ans = sorted(set(A))
    minans = 1
    if max(ans) < 0:
        return 1
    for i in ans:
        if minans < i:
            break
        else:
            minans += 1
    return minans
```

그래서 어제 새로 변수를 추가해서 한 게 생각나서 `minans`를 이용해 풀어봤다. 이렇게 하니 정확도가 50이던가,, [-100, 100]에서 1이 나와야하는데 2가 나온다고 했다. 해보니 정말.. 그렇다 ㅎ;;;

<br/>

### 다른 코드

```python
def solution(A):
    A = set(A)
    count = 1
    while count in A:
        count += 1
    return count
```

sort를 안 쓰고도 할 수 있군... 사실 대부분 sort 혹은 set 둘 중 하나만 쓰더라. 깔끔하고 간단해서 보기 좋다.

<br/>

### 함수 정리

`리스트명.sort()` 리스트형의 메소드이며 리스트 원본값을 직접 수정. 리턴값은 None

`sorted(리스트명)`  내장 함수이며 리스트 원본값은 그대로이고 정렬 값을 반환

<br/>

### 느낀점

또 오늘 배운 건 내가 for 구문을 좀 잘못 생각했던 것 같다. if에서도 `minans < A[i]` 이런 식으로 했는데 사실 이게 아니었다. 다음을 참고해서([03-3 for문 - 점프 투 파이썬](https://wikidocs.net/22)) for문을 이해하였다.

```python
marks = [90, 25, 67, 45, 80]

number = 0 
for mark in marks: 
    number = number +1 
    if mark >= 60: 
        print("%d번 학생은 합격입니다." % number)
    else: 
        print("%d번 학생은 불합격입니다." % number)
```

지금까지 `for i in A` 에 너무 익숙해져서 그런가? 그 i++에... ㅋㅋㅋ 실행 결과는 `1번 학생은 합격입니다. 2번 학생은 불합격입니다.` 이런 식으로 나오는데 이는 리스트 marks에서 차례로 점수를 꺼내어 mark 변수에 대입하고 for문 안 문장을 수행한 것이다. 따라서 다른 코드를 참고로 해 내 코드를 고쳐보자면

```python
def solution(A):
    ans = sorted(set(A))
    minans = 1
    if max(ans) < 0:
        return 1
    for i in ans:
        if i == minans:
            minans += 1
    return minans
```

여기서 i는 리스트 ans에서 차례로 점수를 꺼내 i에 대입하고 i가 minans와 같다면 minans가 1씩 증가하는 형식인 것이다. 이것도 처음에 minans == ans 이런 식으로 썼다가 ㅋㅋㅋㅜ 괜찮아 이렇게,,, 기본 채워가면 되는거지~ 하하하


