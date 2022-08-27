---

layout: post

title: Codility Brackets

summary: append(), pop()

tag: stack

---

## Lesson 7. Stack and Queues - Brackets

### 문제

A string S consisting of N characters is considered to be *properly nested* if any of the following conditions is true:

> - S is empty;
> - S has the form "(U)" or "[U]" or "{U}" where U is a properly nested string;
> - S has the form "VW" where V and W are properly nested strings.

For example, the string "{[()()]}" is properly nested but "([)()]" is not.

Write a function:

> def solution(S)

that, given a string S consisting of N characters, returns 1 if S is properly nested and 0 otherwise.

For example, given S = "{[()()]}", the function should return 1 and given S = "([)()]", the function should return 0, as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [0..200,000];
> - string S consists only of the following characters: "(", "{", "[", "]", "}" and/or ")".

<br/>

### 코드 작성

```python
def solution(S):
    lis = []
    ans = 0

    for i in S:
        lis.append(i)
    
    for i in lis:
        if "(" in lis:
            if (lis.index("(") - lis.index(")"))%2 == 1:
                ans = 1

        elif "{" in lis:
            if (lis.index("{") - lis.index("}"))%2 == 1:
                ans = 1

    
        elif "[" in lis:
            if (lis.index("[") - lis.index("]"))%2 == 1:
                ans = 1

        else:
            ans = 0
            
    return ans
```

10% 정도 성공률이 나온 것 같다. 처음에 elif 없이 했는데 두번째 테스트케이스에서 계속 "{"이 없다길래 흠;;; 어떻게 풀까 고민하다 인덱스를 이용했는데........ 정확도도 그렇고 시간복잡도도 그렇고 이건 아닌 것 같다 ^^! 싶어서 stack을 파보기 시작..

<br/>

```python
def solution(S):
    lis = []

    for i in S:
        lis.append(i)

    if len(lis) == 0:
        return 1
    
    for i in lis:
        if i == '(':
            if lis.pop() == ')':
                return 1
        if i == '{':
            if lis.pop() == '}':
                return 1
        if i == '[':
            if lis.pop() == ']':
                return 1
        else:
            return 0

```

'[()}' 여기에서 걸렸다. 왜 그런지는 잘 모르겠는데 이거 !=로 바꿔주니까 되더라? 한 다음 돌려봤는데 이번에는 ')(' 여기에서 걸렸다. 진짜 도저히 모르겠어서 다른 블로그 찾아서 풀어봄.......... ㄱ-

<br/>

```python
def solution(S):
    lis = []

    for i in S:
        if i == '(' or i == '{' or i == '[':
            lis.append(i)
        elif len(lis) == 0:
            return 0
    
        if i == ')':
            if lis.pop() != '(':
                return 0
        if i == '}':
            if lis.pop() != '{':
                return 0
        if i == ']':
            if lis.pop() != '[':
                return 0

    if len(lis) == 0:
        return 1
    else:
        return 0
```

생각보다 예외 케이스가 되게 많아서 이것저것 생각해줘야 하는 문제였다. 일단 '(' or '{' or '['를 새 리스트에 넣는 것도 좀 신기했는데 주어진 S 안에서 오른쪽 닫기일 때 비교해야 했다. 저기 `elif`에는 }}} 이런 경우들이 들어가겠지.. 어쨌거나 저쨌거나 못 풀기는 했지만 정말 알고리즘 문제같은 문제였다. 시간복잡도는 O(N)

<br/>

### 다른 코드

```python
def solution(S):
    brackets = {"(":")", "{":"}","[":"]"}
    stack = []
    for char in S:
        if brackets.get(char, False): # 여는 괄호이면
            stack.append(char)
        else: # 닫는 괄호이면
            if not stack or brackets[stack.pop()] != char: return 0 # stack 마지막의 괄호와 매칭이 안되면
    if stack: return 0 # stack에 남은 요소가 있으면    
    else: return 1 # stack이 비어있으면

```

딕셔너리 활용하는 것 좋아보인다. 나도 딕셔너리를 좀 자주 많이 활용하고 싶다..!!!

<br/>

### 함수 정리

`A.append(obj)` 배열의 가장 마지막에 obj 추가

`A.pop()` 리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제



<br/>

### 느낀점

<img width="503" alt="스크린샷 2022-08-27 오후 11 33 12" src="https://user-images.githubusercontent.com/72901045/187034865-515cb44d-ce02-4be9-a3dc-241ef58c5221.png">

<img width="480" alt="스크린샷 2022-08-27 오후 11 33 33" src="https://user-images.githubusercontent.com/72901045/187034870-7680afbc-19f4-429a-b248-bc2538acd179.png">

<img width="253" alt="스크린샷 2022-08-27 오후 11 33 58" src="https://user-images.githubusercontent.com/72901045/187034871-665a44f5-f364-41b6-834a-512a1b3c3149.png">

뭔가 오늘 문제에 가장 페이지를 많이 쓴 것 같다 ㅋㅋㅋ 아니 근데 나는 진짜 왼쪽 열기 부분만 스택에 추가한다고 상상도 못했네,, ㅎㅁㅎ 이번 기회에 확실히 스택 구조는 이해한 것 같다!
