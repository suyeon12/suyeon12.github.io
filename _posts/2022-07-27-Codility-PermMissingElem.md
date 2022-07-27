---

layout: post

title: Codility PermMissingElem

summary: sum()

tags: Time-Complexity

---



## Lesson 3. Time Complexity - PermMissingElem

### 문제

An array A consisting of N different integers is given. The array contains integers in the range [1..(N + 1)], which means that exactly one element is missing.

Your goal is to find that missing element.

Write a function:

> def solution(A)

that, given an array A, returns the value of the missing element.

For example, given array A such that:

A[0] = 2
 A[1] = 3
 A[2] = 1
 A[3] = 5

the function should return 4, as it is the missing element.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..100,000];
> - the elements of A are all distinct;
> - each element of array A is an integer within the range [1..(N + 1)].

<br/>

### 알고리즘

11 + return = 1+2+3+4+5

다음을 이용해서 풀었다. 좌변은 값 2,3,1,5를 모두 더한 값에 return값을 더한 것이고 우변은 A의 길이에서 1을 더한 숫자를 모두 더한 값이다. 시간복잡도인만큼 for 문을 쓰지 않고 합을 더하니 예전에 배운 등차수열의 합이 생각났다.

<br/>

### 코드 작성

```python
def solution(A):
    a = len(A) + 1
    aa = int(a*(a+1)/2)

    return aa- sum(A)
```

처음에 아무 생각 없이 변수에 sum 썼다가 `TypeError: 'int' object is not callable` 떠서 ^^;;... 임의로 aa 쓰긴 했는데 변수 이름을 너무 대충 쓰긴 한 것 같다.

<br/>

### 다른 코드

```python
def solution(A):
  return sum (range(len(A)+2)) - sum(A)
```

`range()` 에 2를 추가한 게 신기하다.

<br/>

```python
def solution(A):
    A.sort()
    if len(A) == 0:
        return 1
    for i in range(len(A)):
        if A[i] != i+1:
            return i+1
    return len(A)+1
```

이건 대부분 sum으로 풀었는데 sum이 아니라 신기해서 넣어봄. 예외처리를 한 부분도 그렇고 발상이 신선한 것 같다.

<br/>

### 느낀점

<img width="715" alt="스크린샷 2022-07-27 오후 11 44 24" src="https://user-images.githubusercontent.com/72901045/181277200-fca462c4-afe8-44cf-b5fc-f22d61c6f7ea.png">

정말 뿌듯하군아 ^ㅡ^.. 근데 다음에는 변수명도 조금 신경 써봐야겠다!! 그리고 정말 조금 더 머리쓰면 효율적인 코드 작성 가능하네..


