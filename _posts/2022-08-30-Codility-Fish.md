---

layout: post

title: Codility Fish

summary: append(), pop()

tag: stack

---

## Lesson 7. Stack and Queues - Fish

### 문제

You are given two non-empty arrays A and B consisting of N integers. Arrays A and B represent N voracious fish in a river, ordered downstream along the flow of the river.

The fish are numbered from 0 to N − 1. If P and Q are two fish and P < Q, then fish P is initially upstream of fish Q. Initially, each fish has a unique position.

Fish number P is represented by A[P] and B[P]. Array A contains the sizes of the fish. All its elements are unique. Array B contains the directions of the fish. It contains only 0s and/or 1s, where:

> - 0 represents a fish flowing upstream,
> - 1 represents a fish flowing downstream.

If two fish move in opposite directions and there are no other (living) fish between them, they will eventually meet each other. Then only one fish can stay alive − the larger fish eats the smaller one. More precisely, we say that two fish P and Q meet each other when P < Q, B[P] = 1 and B[Q] = 0, and there are no living fish between them. After they meet:

> - If A[P] > A[Q] then P eats Q, and P will still be flowing downstream,
> - If A[Q] > A[P] then Q eats P, and Q will still be flowing upstream.

We assume that all the fish are flowing at the same speed. That is, fish moving in the same direction never meet. The goal is to calculate the number of fish that will stay alive.

For example, consider arrays A and B such that:

A[0] = 4 B[0] = 0
 A[1] = 3 B[1] = 1
 A[2] = 2 B[2] = 0
 A[3] = 1 B[3] = 0
 A[4] = 5 B[4] = 0

Initially all the fish are alive and all except fish number 1 are moving upstream. Fish number 1 meets fish number 2 and eats it, then it meets fish number 3 and eats it too. Finally, it meets fish number 4 and is eaten by it. The remaining two fish, number 0 and 4, never meet and therefore stay alive.

Write a function:

> def solution(A, B)

that, given two non-empty arrays A and B consisting of N integers, returns the number of fish that will stay alive.

For example, given the arrays shown above, the function should return 2, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - each element of array A is an integer within the range [0..1,000,000,000];
> - each element of array B is an integer that can have one of the following values: 0, 1;
> - the elements of A are all distinct.

<br/>

### 상황 분석

1. P < Q 상황에서 B[P] = 1, B[Q] = 0이어야 한다.

2-1. A[P] > A[Q]라면 P는 Q를 먹는다(=Q가 사라짐)

2-2. A[P] < A[Q]라면 Q는 P를 먹는다(=P가 사라짐)

사실 살아있는 물고기의 수를 반환하면 되기 때문에 P가 사라지든 Q가 사라지든 별 상관은 없다.

<br/>

### 알고리즘

스택이니까 이거 스택을 어떻게 써먹을까... 고민했다. 다른 사람들은 문제 보면 바로 이렇게 하면 되겠다는 아이디어가 떠오르는 걸까? 일단 첫번째 조건을 비교해야 하므로 B[i] 값이 1일때 A[i]를 스택에 넣어준 후 두번째 조건을 비교했다.

<br/>

### 코드 작성

```python
def solution(A, B):
    stack = []
    count = len(A)

    for i in range(len(A)):
        if B[i] == 1:
            stack.append(A[i])
        else:
            while(len(stack) > 0):
                if stack[-1] > A[i]:
                    count -= 1
                    break
                else:
                    count -= 1
                    stack.pop()
    return count 
```

전에 블로그에 올리고 왜 변수명을 lis로 했는지 살짝 후회되서 이번에는 스택으로 했다 ㅋㅋㅋ stack이라 `pop()` 을 썼는데 사실 count만 반환하니 딱히 필요없는게 아닌가? 싶기도 하고.. 아 시간복잡도는 O(N)

<br/>

### 다른 코드

```python
def solution(fish_sizes, fish_directions):
    """Solution method implementation."""
    # stack and count initialization
    ds_fishes = []
    count = 0
    # iterate through the list of fishes:
    for i, _ in enumerate(fish_directions):
        # downstream fish
        if fish_directions[i]:
            ds_fishes.append(fish_sizes[i])
            continue
        # upstream fish - fighting all downstream stack
        while ds_fishes:
            # upstream fish loses and dies     
            if ds_fishes[-1] > fish_sizes[i]:
                break
            # upstream fish wins the match and fights another one       
            ds_fishes.pop()
        # loop didn't end prematurely = upstream fish ate the stack 
        else:
            count += 1
    # return upstream winners + remaining downstream fishes
    return count + len(ds_fishes)
```

여기서는 count와 스택 길이를 반환했는데 궁금한게 스택은 pop()을 통해 테스트 케이스를 생각했을 때 0이 되지 않나..? 흠;;

<br/>

### 함수 정리

`A.append(obj)` 배열의 가장 마지막에 obj 추가

`list[-1]` 리스트에서 마지막 값을 가져옴

`A.pop()` 리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제

<br/>

### 느낀점

스택 문제는 easy인데도 어렵다.. 뭔 개념인지는 알겠는데 활용이 안됨 ^ㅡㅜ.. 아니 왜 이때 이걸... 스택으로 넣지?하는데 풀다보면 아 그렇구나 알겠는 그런...
