---

layout: post

title: Codility Peaks

summary: //

---

## Lesson 10. Prime and composite numbers - Peaks

### 문제

A non-empty array A consisting of N integers is given.

A *peak* is an array element which is larger than its neighbors. More precisely, it is an index P such that 0 < P < N − 1,  A[P − 1] < A[P] and A[P] > A[P + 1].

For example, the following array A:

A[0] = 1
 A[1] = 2
 A[2] = 3
 A[3] = 4
 A[4] = 3
 A[5] = 4
 A[6] = 1
 A[7] = 2
 A[8] = 3
 A[9] = 4
 A[10] = 6
 A[11] = 2

has exactly three peaks: 3, 5, 10.

We want to divide this array into blocks containing the same number of elements. More precisely, we want to choose a number K that will yield the following blocks:

> - A[0], A[1], ..., A[K − 1],
> - A[K], A[K + 1], ..., A[2K − 1],  
>   ...
> - A[N − K], A[N − K + 1], ..., A[N − 1].

What's more, every block should contain at least one peak. Notice that extreme elements of the blocks (for example A[K − 1] or A[K]) can also be peaks, but only if they have both neighbors (including one in an adjacent blocks).

The goal is to find the maximum number of blocks into which the array A can be divided.

Array A can be divided into blocks as follows:

> - one block (1, 2, 3, 4, 3, 4, 1, 2, 3, 4, 6, 2). This block contains three peaks.
> - two blocks (1, 2, 3, 4, 3, 4) and (1, 2, 3, 4, 6, 2). Every block has a peak.
> - three blocks (1, 2, 3, 4), (3, 4, 1, 2), (3, 4, 6, 2). Every block has a peak. Notice in particular that the first block (1, 2, 3, 4) has a peak at A[3], because A[2] < A[3] > A[4], even though A[4] is in the adjacent block.

However, array A cannot be divided into four blocks, (1, 2, 3), (4, 3, 4), (1, 2, 3) and (4, 6, 2), because the (1, 2, 3) blocks do not contain a peak. Notice in particular that the (4, 3, 4) block contains two peaks: A[3] and A[5].

The maximum number of blocks that array A can be divided into is three.

Write a function:

> def solution(A)

that, given a non-empty array A consisting of N integers, returns the maximum number of blocks into which A can be divided.

If A cannot be divided into some number of blocks, the function should return 0.

For example, given:

A[0] = 1
 A[1] = 2
 A[2] = 3
 A[3] = 4
 A[4] = 3
 A[5] = 4
 A[6] = 1
 A[7] = 2
 A[8] = 3
 A[9] = 4
 A[10] = 6
 A[11] = 2

the function should return 3, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [0..1,000,000,000].



<br/>

### 상황 분석

제일 처음 이거 문제 이해를 잘못함... 같은 크기로 나눠야하는 줄 몰랐어

어쨌든 하나 이상의 peak를 포함하는 크기가 같은 block으로 나눌 때, 최대 몇 개로 block을 나눌 수 있을까?

<br/>

### 코드 작성

```python
def solution(A):
    peak=[]

    for p in range(1, len(A)-1):
        if A[p-1] < A[p] and A[p] > A[p+1]:
            peak.append(p)

    # print(peak)
    # print(5 // 2) # // 몫

    if len(peak) == 0:
        return 0

    for n in range(len(peak), 0, -1):
        if len(A) % n == 0:
            count = 0
            block_size = len(A) // n
            block = [0] * n
            for m in range(len(peak)):
                index = peak[m] // block_size
                if block[index] == 0:
                    block[index] = 1
                    count += 1
                # print(block)
                # print(count)
            if count == n:
                return n
```

오늘도 구글링해서 풀었다 ㅎ... 긍정적인걸 말하자면 코드 이해가 되서 좋았는데 먼저 같은 개수의 block으로 나눠야해서 정수일 경우만 생각한다.(`block_size = len(A) // n`, `index = peak[m] // block_size`) peak의 개수만큼 반복해 같은 개수의 block이 가능한지 구하고 count를 추가해준다. 그리고 count와 peak가 같다면 return한다.

<br/>

### 함수 정리

`//` 몫을 구하는 산술 연산자



<br/>

### 느낀점

정말 코테 문제 두 번 풀어봐야겠다는 필요성을 여실히 느끼는 중,,!
