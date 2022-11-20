---

layout: post

title: Codility MinAbsSumOfTwo

summary: lambda

---

## Lesson 15. Caterpillar method - MinAbsSumOfTwo

### 문제

Let A be a non-empty array consisting of N integers.

The *abs sum of two* for a pair of indices (P, Q) is the absolute value |A[P] + A[Q]|, for 0 ≤ P ≤ Q < N.

For example, the following array A:

A[0] = 1
 A[1] = 4
 A[2] = -3

has pairs of indices (0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2).   
The abs sum of two for the pair (0, 0) is A[0] + A[0] = |1 + 1| = 2.   
The abs sum of two for the pair (0, 1) is A[0] + A[1] = |1 + 4| = 5.   
The abs sum of two for the pair (0, 2) is A[0] + A[2] = |1 + (−3)| = 2.   
The abs sum of two for the pair (1, 1) is A[1] + A[1] = |4 + 4| = 8.   
The abs sum of two for the pair (1, 2) is A[1] + A[2] = |4 + (−3)| = 1.   
The abs sum of two for the pair (2, 2) is A[2] + A[2] = |(−3) + (−3)| = 6.   

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N integers, returns the minimal abs sum of two for any pair of indices in this array.

For example, given the following array A:

A[0] = 1
 A[1] = 4
 A[2] = -3

the function should return 1, as explained above.

Given array A:

A[0] = -8
 A[1] = 4
 A[2] = 5
 A[3] =-10
 A[4] = 3

the function should return |(−8) + 5| = 3.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [−1,000,000,000..1,000,000,000].



<br/>

### 코드 작성

```python
def solution(A):
    A.sort()
    front = 0
    back = len(A)-1
    minsum = abs(A[front] + A[back])

    while front < back:
        minsum = min(minsum, abs(A[front] + A[back]))
        if abs(A[front]) > abs(A[back]):
            front += 1
        else:
            back -= 1
    return minsum
```

front랑 back 설정해준다음 최솟값을 구해줬다. 그 다음 `abs(A[front]) > abs(A[back])`으로 Caterpillar를 설정해봤는데 그 전에 minsum에서 최솟값이 걸려져서 원하는 답이 나오는 구조더라구,, 근데!!

`For example, for the input [8, 5, 3, 4, 6, 8] the solution returned a wrong answer (got 7 expected 6).`

이렇게 떠서 다시 수정했다 ^^,

<br/>

```python
def solution(A):
    A.sort()
    front = 0
    back = len(A)-1
    minsum = abs(A[front] + A[back])

    while front <= back:
        minsum = min(minsum, abs(A[front] + A[back]))
        if abs(A[front]) > abs(A[back]):
            front += 1
        else:
            back -= 1
    return minsum
```

생각해보니까 (0,0), (1,1), (2,2) 이런 식으로 같아도 되더라 그래서 front = back 요소를 넣어줬다. 그러니까 안정적으로 100퍼 뜨더라~ 시간 복잡도는 O(N * log(N))

<br/>

### 다른 코드

```java
import java.util.Arrays;

class Solution {
    public int solution(int[] A) {
        int N = A.length;
        Arrays.sort(A);
        int tail = 0;
        int head = N - 1;
        int minAbsSum = Math.abs(A[tail] + A[head]);
        while (tail <= head) {
            int currentSum = A[tail] + A[head];
            minAbsSum = Math.min(minAbsSum, Math.abs(currentSum));
            // If the sum has become
            // positive, we should know that the head can be moved left
            if (currentSum <= 0)
                tail++;
            else
                head--;
        }
        return minAbsSum;
    }
}
```

점점 파이썬 코드가 없어짐.. ㄱ- 여기선 `if (currentSum <= 0)` 이걸로 판단한 게 신기하다!

<br/>

```python
def solution(A):
    A.sort(key=lambda x: abs(x))  # 인자의 절대 값 기준으로 정렬

    minimum = abs(A[0] + A[0])  # 중복된 인자로 구성 된 pair는 가장 작은 인자로만 체크하면 된다
    
    for i in range(len(A) - 1):
    	# 정렬을 했기 때문에 서로 붙어있는 인자가 절대 값의 차이가 가장 작을 수 밖에 없다
        minimum = min(minimum, abs(A[i] + A[i + 1]))  
    
    return minimum
```

이거 진짜 너무 신기해 ㅋㅋㅋ 하긴 붙어 있는 게 절댓값의 차이가 가장 작지.. 람다식도 좋아보인다

<br/>

### 함수 정리

`lambda 인자 : 표현식`  [다음 블로그](https://kingofbackend.tistory.com/98) 참고



<br/>

### 느낀점

마지막 문제를 푸니 Caterpillar가 뭔지 살짝 알 것 같다 ^ㅡ^,
