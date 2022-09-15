---

layout: post

title: Codility StoneWall

summary: append(), pop()

tag: stack

---

## Lesson 7. Stack and Queues - StoneWall

### 문제

You are going to build a stone wall. The wall should be straight and N meters long, and its thickness should be constant; however, it should have different heights in different places. The height of the wall is specified by an array H of N positive integers. H[I] is the height of the wall from I to I+1 meters to the right of its left end. In particular, H[0] is the height of the wall's left end and H[N−1] is the height of the wall's right end.

The wall should be built of cuboid stone blocks (that is, all sides of such blocks are rectangular). Your task is to compute the minimum number of blocks needed to build the wall.

Write a function:

> def solution(H)

that, given an array H of N positive integers specifying the height of the wall, returns the minimum number of blocks needed to build it.

For example, given array H containing N = 9 integers:

H[0] = 8 H[1] = 8 H[2] = 5
 H[3] = 7 H[4] = 9 H[5] = 8
 H[6] = 7 H[7] = 4 H[8] = 8

the function should return 7. The figure shows one possible arrangement of seven blocks.

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/stone_wall/static/images/auto/4f1cef49cc46d451e88109d449ab7975.png)

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array H is an integer within the range [1..1,000,000,000].



<br/>

### 상황 분석

H[I]를 길이라고 가정할 때 각 길이들은 블록처럼 쌓을 수 있다. 이때 필요한 블록 개수를 리턴하면 된다.

<br/>

### 알고리즘

말은 쉽지만 ^^... 처음에는 현재값과 다음값을 비교해서 높이가 같다면 합쳐지게끔 할까.. 싶었는데 이렇게 되니 너무 복잡했다. stack에 있으니 stack을 활용해야 할텐데 이걸 어떻게 하지??를 외치며,, 구글링을 했다 ㅋㅋ..

<br/>

### 코드 작성

```python
def solution(H):
    stack = []
    count = 0
    for i in range(len(H)):
        while len(stack) != 0 and stack[-1] > H[i]:
            stack.pop()
            count += 1
        while len(stack) != 0 and stack[-1] < H[i]:
            stack.append(i)
        if len(stack) == 0:
            stack.append(i)
    
    return len(stack) + count

```

이해할 수 있는 설명과 코드라 작성해 봤는데 `TIMEOUT ERROR (Killed. Hard limit reached: 5.000 sec.)` 가 떴다. 아 이게 아닌가.. 싶어 다른 코드를 찾아봤다.

<br/>

```python
def solution(H):
    count = 0
    stack = []

    for i in range(len(H)):
        while len(stack) > 0 and stack[-1] > H[i]:
            stack.pop()

        if len(stack) == 0 or stack[-1] < H[i]:
            count += 1
            stack.append(H[i])

    return count

    pass
```

stack을 이용해 index마다 높이와 stack 값을 비교해 H[I]가 stack보다 작으면 `pop`하고 크다면 `push(append)`한다. 그렇구나... 싶었는데 실제 예시로 디버그하듯이 해보니까 잘 모르겠어서 이제 이해함 ㅋㅋ 나는 저 크거나 작은게 한 번 한 다음 바로 넘어가는 줄 알았는데 while문이라 계속 돌아가는 것이다....

<br/>

| index 값 | stack                                                | count |
|:-------:| ---------------------------------------------------- | ----- |
| 8       | [8]                                                  | 1     |
| 8       | pass                                                 | 변동없음  |
| 5       | 기존 8이 `pop`되어 [], [5]                                | 2     |
| 7       | `append` [5, 7]                                      | 3     |
| 9       | `append` [5, 7, 9]                                   | 4     |
| 8       | 기존 9가 `pop`되어 [5, 7], `append`해서 [5, 7, 8]           | 5     |
| 7       | 기존 8이 `pop`되어 [5, 7], pass                           | 변동없음  |
| 4       | 기존 7이 `pop`되어 [5], 기존 5가 `pop`되어 [],  `append`해서 [4] | 6     |
| 8       | `append` [4, 8]                                      | 7     |

그래도 이해하니까 매우 뿌듯하다 ^ㅡ^..

<br/>

### 함수 정리

`while` 조건문이 참인 동안에 while문 아래의 문장이 반복해서 수행됨

`A.pop()` 리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제

`A.append(obj)` 배열의 가장 마지막에 obj 추가




<br/>

### 느낀점

<img width="568" alt="스크린샷 2022-09-15 오후 8 19 59" src="https://user-images.githubusercontent.com/72901045/190390954-68851d5f-aa3b-4393-bb7f-de3cc76279cb.png">

<img width="627" alt="스크린샷 2022-09-15 오후 8 20 25" src="https://user-images.githubusercontent.com/72901045/190391019-7ad6a2fa-1189-41a7-bcdf-a91100af01bc.png">

<img width="519" alt="스크린샷 2022-09-15 오후 8 21 17" src="https://user-images.githubusercontent.com/72901045/190391176-0474e755-ad3a-421c-95b8-e79e9a0f1224.png">

<img width="559" alt="스크린샷 2022-09-15 오후 8 21 36" src="https://user-images.githubusercontent.com/72901045/190391226-305fa2fa-ca91-4f4e-afcd-9ba67f8af934.png">

오늘도 참 많은 페이지를 썼구나 ^ㅡ^.. 오늘은 내 힘으로 못 풀었더라도 이해하고 넘어가면 다음에는 풀 수 있겠지~~.. ~
