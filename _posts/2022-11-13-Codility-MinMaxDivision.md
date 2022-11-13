---

layout: post

title: Codility MinMaxDivision

---

## Lesson 14. Binary search algorithm - MinMaxDivision

### 문제

You are given integers K, M and a non-empty array A consisting of N integers. Every element of the array is not greater than M.

You should divide this array into K blocks of consecutive elements. The size of the block is any integer between 0 and N. Every element of the array should belong to some block.

The sum of the block from X to Y equals A[X] + A[X + 1] + ... + A[Y]. The sum of empty block equals 0.

The *large sum* is the maximal sum of any block.

For example, you are given integers K = 3, M = 5 and array A such that:

A[0] = 2
 A[1] = 1
 A[2] = 5
 A[3] = 1
 A[4] = 2
 A[5] = 2
 A[6] = 2

The array can be divided, for example, into the following blocks:

> - [2, 1, 5, 1, 2, 2, 2], [], [] with a large sum of 15;
> - [2], [1, 5, 1, 2], [2, 2] with a large sum of 9;
> - [2, 1, 5], [], [1, 2, 2, 2] with a large sum of 8;
> - [2, 1], [5, 1], [2, 2, 2] with a large sum of 6.

The goal is to minimize the large sum. In the above example, 6 is the minimal large sum.

Write a function:

> def solution(K, M, A)

that, given integers K, M and a non-empty array A consisting of N integers, returns the minimal large sum.

For example, given K = 3, M = 5 and array A such that:

A[0] = 2
 A[1] = 1
 A[2] = 5
 A[3] = 1
 A[4] = 2
 A[5] = 2
 A[6] = 2

the function should return 6, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N and K are integers within the range [1..100,000];
> - M is an integer within the range [0..10,000];
> - each element of array A is an integer within the range [0..M].



<br/>

### 상황 분석

A 배열을 K개로 나눴을 때 최솟값을 return하면 된다

<br/>

### 알고리즘

- 이진 탐색 알고리즘
  
  1. 배열의 중간값을 가져온다
  
  2. 중간값 = 검색값: 종료
     
     중간값 < 검색값: 오른쪽으로 이동
     
     중간값 > 검색값: 왼쪽으로 이동

[5, 10, 14, 25, 27, 32, 39, 45, 52, 60] 다음의 배열에서 `32`를 이진 탐색 알고리즘으로 찾는 방법

1. mid = (0 + 9) // 2 = 4 -> 27이 mid

2. 27 < 32이므로 오른쪽으로 이동, low = mid + 1 = 5

3. mid = (5 + 9) // 2 = 7 -> 45가 mid

4. 45 > 32이므로 왼쪽으로 이동, high = mid - 1 = 6

5. mid = (5 + 6) // 2 = 5

6. 32 = 32이므로 탐색 종료



<br/>

### 코드 작성

```python
def isblocknumber(A, K, total):
    block_sum = 0
    block_count = 0

    for i in A:
        if block_sum + i > total:
            block_sum = i
            block_count += 1
        else:
            block_sum += i
        if block_count >= K:
            return False
    return True


def solution(K, M, A):
    min_value = max(A) 
    max_value = sum(A)

    if K == 1:
        return max_value
    if K >= len(A):
        return min_value
    
    while(min_value <= max_value):
        mid = (min_value + max_value) // 2
        #print("mid", mid)
        if isblocknumber(A, K, mid):
            max_value = mid - 1
            #print("max", max_value)
        else:
            min_value = mid + 1
            #print("min", min_value)
    
    return min_value
```

return되는 값의 최솟값은 `max(A)`이고 최댓값은 `sum(A)`이다. 만약 K가 1이라면 `sum(A)`가 return될 것이고 K가 배열 A의 총 길이보다 크다면 `max(A)`가 return될 것이다(ex. A가 [2, 1, 5]이고 k가 4라면 [2], [1], [5], []로 return값은 5).

따라서 max(A) <= return <= sum(A) 사이에 찾는 답이 있을텐데 여기에서 이진탐색을 수행하면 된다. 참고로 `print`함수를 통해 디버그한 결과는 다음과 같다.

| mid | max | min |
| --- | --- | --- |
| 10  | 9   |     |
| 7   | 6   |     |
| 5   |     | 6   |
| 6   | 5   |     |

시간복잡도는 O(N*log(N+M)) 

<br/>

### 다른 코드

```python
def resulting_blocks(target_no_of_blocks, numbers, target_sum):
    """Count blocks that have a sum at most equal to target_sum."""
    result, temp_sum = 0, 0
    for number in numbers:
        if temp_sum + number > target_sum:
            result += 1
            temp_sum = number
        else:
            temp_sum += number
    result += 1
    return max(result, target_no_of_blocks)

def solution(target_no_of_blocks, _, numbers):
    """Solution method implementation."""
    # initializations
    upper, lower, result = sum(numbers), max(numbers), -1
    # main binary search loop
    while upper >= lower:
        # get mid point
        mid = (upper + lower) // 2
        # split array into blocks summing up the mid point       
        blocks = resulting_blocks(target_no_of_blocks, numbers, mid)
        
        # the sweet spot's above mid point       
        if blocks > target_no_of_blocks:
            lower = mid + 1
        # found a minimal large sum candidate
        # seek an even lower candidate in [lower, mid-1]
        elif blocks == target_no_of_blocks:
            result = mid if result == -1 else min(result, mid)
            upper = mid - 1
    return result
```

True or False가 아니라 block의 값을 return함

<br/>

### 느낀점

<img width="603" alt="스크린샷 2022-11-13 오후 2 30 46" src="https://user-images.githubusercontent.com/72901045/201507391-77f6e4fe-c392-4159-9342-d87e5ea5122d.png">

블록개수 판단하는 부분이 살짝 어려웠다


