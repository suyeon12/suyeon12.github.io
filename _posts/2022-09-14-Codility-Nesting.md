---

layout: post

title: Codility Nesting 

summary: append(), pop()

tag: stack

---

## Lesson 7. Stack and Queues - Nesting

### 문제

A string S consisting of N characters is called *properly nested* if:

> - S is empty;
> - S has the form "(U)" where U is a properly nested string;
> - S has the form "VW" where V and W are properly nested strings.

For example, string "(()(())())" is properly nested but string "())" isn't.

Write a function:

> def solution(S)

that, given a string S consisting of N characters, returns 1 if string S is properly nested and 0 otherwise.

For example, given S = "(()(())())", the function should return 1 and given S = "())", the function should return 0, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..1,000,000];
> - string S consists only of the characters "(" and/or ")".



<br/>

### 알고리즘

이전에 풀었던 [Brackets](https://suyeon12.github.io/2022/08/27/codility-brackets) 문제와 굉장히 비슷했다. 오히려 더 쉬운 것 같다.

1. 문자열 요소가 왼쪽 열기인 경우 스택에 넣는다.

2. 스택 길이가 0이라면(ex. `))))`인 경우) 0을 리턴한다.

3. 문자열 요소가 오른쪽 닫기이고 스택의 마지막 요소가 왼쪽 열기가 아닌 경우(`pop`) 0을 리턴한다.

4. 이후 남아있는 스택 길이가 0이라면 1을 반환하고 아니라면 0을 반환한다.

<br/>

### 코드 작성

```python
def solution(S):
    stack = []
    for i in S:
        if i == "(":
            stack.append(i)
        elif len(stack) == 0:
            return 0
        
        if i == ")":
            if stack.pop() != "(":
                return 0
                
    if len(stack) == 0:
        return 1
```

이전에 풀었던 문제를 알고리즘 스터디 시간에 소개했는데 그때 팀원분께서 위에 `len(stack) == 0`을 했다면 마지막 부분에는 필요가 없는 게 아니냐는 의견을 주셔서 그 부분을 반영해봤다. 근데 막상 이렇게 적어보니 75%의 정확도가 나왔는데...

`The following issues have been detected: runtime errors. For example, for the input '()(()()(((()())(()()))' the solution terminated unexpectedly.` 흠.... 그래서 다시 `else` 부분을 넣기로 했다.

<br/>

```python
def solution(S):
    stack = []

    for i in S:
        if i == "(":
            stack.append(i)
        
        if len(stack) == 0:
            return 0
        
        if i == ")":
            if stack.pop() != "(":
                return 0
    
    if len(stack) == 0:
        return 1
    else:
        return 0
```

이왕 할 거 복붙 하지말고 아예 첨부터 다시 써보자!! 하고 썼는데 살짝 차이가 있네... 어쨌든 O(N)으로 통과~

<br/>

### 다른 코드

```python
def solution(S):
    
    nest = {"(":")"}
    stack = []

    for s in S:
        if s in nest.keys():
            stack.append(s)
        elif s in nest.values():
            if stack:
                stack.pop()
            else:
                return 0        
    
    if not stack:
        return 1
    else:
        return 0
```

맨날 딕셔너리 활용한 것 보면서 좋아보인다고 다짐만 하는 사람...

<br/>

### 함수 정리

`A.append(obj)` 배열의 가장 마지막에 obj 추가

`A.pop()` 리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제

<br/>

### 느낀점

brackets 문제의 간단 버전이라서 처음에 내가 문제를 잘못 클릭했나? 싶었다. 저번에 이 문제 풀었을 때는 너~~~무 어려웠는데 이제는 3분컷이 된 거에 뿌듯해하며 ^ㅡ^ 오늘은 필기도 안했다! 푸하하
