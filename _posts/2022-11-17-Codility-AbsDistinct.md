---

layout: post

title: Codility AbsDistinct

summary: dict.fromkeys(keys).items()

---

## Lesson 15. Caterpillar method - AbsDistinct

### 문제

A non-empty array A consisting of N numbers is given. The array is sorted in non-decreasing order. The *absolute distinct count* of this array is the number of distinct absolute values among the elements of the array.

For example, consider array A such that:

A[0] = -5
 A[1] = -3
 A[2] = -1
 A[3] = 0
 A[4] = 3
 A[5] = 6

The absolute distinct count of this array is 5, because there are 5 distinct absolute values among the elements of this array, namely 0, 1, 3, 5 and 6.

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N numbers, returns absolute distinct count of array A.

For example, given array A such that:

A[0] = -5
 A[1] = -3
 A[2] = -1
 A[3] = 0
 A[4] = 3
 A[5] = 6

the function should return 5, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [−2,147,483,648..2,147,483,647];
> - array A is sorted in non-decreasing order.



<br/>

### 코드 작성

```python
def solution(A):
    answer = []
    for i in A:
        answer.append(abs(i))
    #print(answer)
    return len(set(answer))
```

절댓값 해준 걸 배열 안에 넣고 집합화시킨 다음 개수를 return 해줬다. 너무 빨리 끝나서 당황 ㅇ.ㅇ;;;;; O(N) or O(N*log(N)) 시간 복잡도가 나왔다.

<br/>

### 다른 코드

```python
def solution(A):
    # 이미 카운트된 숫자인지 체크할 딕셔너리. 리스트보다 조회가 빨라서 사용했다.
    dic = {key : 0 for key, value in dict.fromkeys(A).items()}
    count = 0
    for i in range(len(A)):
        # 이미 앞에서 나온 수는 카운트 안하고 다음으로 넘어감
        if dic.get(A[i]) != 0:
            continue
        # 새로 나온 숫자와 그 반대 부호 수는 딕셔너리에서 value를 1로 해주고 개수 카운트    
        else:
            dic[A[i]], dic[-A[i]] = 1, 1
            count += 1

    return count
```

딕셔너리 활용한 거 좋아보인다!

<br/>

```python
def solution(A):
    # write your code in Python 3.6
    return len(set(list(map(lambda a:abs(a), A)))
```

람다식 멋있네

<br/>

### 함수 정리

`dict.fromkeys(keys).items()` key와 value를 이용해 딕셔너리를 만듦

`dict.fromkeys(keys, value)` 해당 값이 키의 값으로 저장됨

`items()` 딕셔너리의 키-값 쌍을 모두 가져옴

<br/>

### 느낀점

코딜리티는 easy랑 medium 차이가 좀 많이 커보여
