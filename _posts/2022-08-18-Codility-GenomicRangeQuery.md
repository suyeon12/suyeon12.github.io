---

layout: post

title: Codility GenomicRangeQuery

summary: slice, append(), enumerate(), dictiionary

---

## Lesson 5. Prefix Sums - GenomicRangeQuery

### 문제

A DNA sequence can be represented as a string consisting of the letters A, C, G and T, which correspond to the types of successive nucleotides in the sequence. Each nucleotide has an *impact factor*, which is an integer. Nucleotides of types A, C, G and T have impact factors of 1, 2, 3 and 4, respectively. You are going to answer several queries of the form: What is the minimal impact factor of nucleotides contained in a particular part of the given DNA sequence?

The DNA sequence is given as a non-empty string S = S[0]S[1]...S[N-1] consisting of N characters. There are M queries, which are given in non-empty arrays P and Q, each consisting of M integers. The K-th query (0 ≤ K < M) requires you to find the minimal impact factor of nucleotides contained in the DNA sequence between positions P[K] and Q[K] (inclusive).

For example, consider string S = CAGCCTA and arrays P, Q such that:

P[0] = 2 Q[0] = 4
 P[1] = 5 Q[1] = 5
 P[2] = 0 Q[2] = 6

The answers to these M = 3 queries are as follows:

> - The part of the DNA between positions 2 and 4 contains nucleotides G and C (twice), whose impact factors are 3 and 2 respectively, so the answer is 2.
> - The part between positions 5 and 5 contains a single nucleotide T, whose impact factor is 4, so the answer is 4.
> - The part between positions 0 and 6 (the whole string) contains all nucleotides, in particular nucleotide A whose impact factor is 1, so the answer is 1.

Write a function:

> def solution(S, P, Q)

that, given a non-empty string S consisting of N characters and two non-empty arrays P and Q consisting of M integers, returns an array consisting of M integers specifying the consecutive answers to all queries.

Result array should be returned as an array of integers.

For example, given the string S = CAGCCTA and arrays P, Q such that:

P[0] = 2 Q[0] = 4
 P[1] = 5 Q[1] = 5
 P[2] = 0 Q[2] = 6

the function should return the values [2, 4, 1], as explained above.

Write an ****efficient**** algorithm for the following assumptions:

> - N is an integer within the range [1..100,000];
> - M is an integer within the range [1..50,000];
> - each element of arrays P and Q is an integer within the range [0..N - 1];
> - P[K] ≤ Q[K], where 0 ≤ K < M;
> - string S consists only of upper-case English letters A, C, G, T.

<br/>

### 코드 작성

```python
def solution(S, P, Q):
    new_list = list(S)
    ans = []

    for i in range(len(P)):
        new_list = S[P[i]:Q[i]+1]
        if 'A' in new_list:
            ans.append(1)
        elif 'C' in new_list:
            ans.append(2)
        elif 'G' in new_list:
            ans.append(3)
        else:
            ans.append(4)
    return ans
```

처음에 range(P[i],Q[i]) 뭐 이런 식으로 했는데 슬라이스 개념을 이용했다. 다 풀고 시간 복잡도도 O(N + M)이라 100% 통과인데 뭔가.. 찝찝해...... prefix sum 이거 사용 안 해서 그런가? 뭔가 흠... 사실 이거 제출할 때도 설마..?! 하면서 테스트를 돌려봤는데 통과되고 헉 설마..??! 하면서 제출했는데 결과가 흠 ^ㅡ^... 넘 얼떨떨해서 if 구문 작동 순서 검색해서 찾아봄,, 순서대로라는 대답을 얻었다. 그럼 저게 맞긴 한데 흐으으음...

<br/>

### 다른 코드

```python
def solution(seq, seq_beg, seq_end):
    """Solution method implementation."""
    # initialization of variables
    factors = {
        "A": 1,
        "C": 2,
        "G": 3,
        "T": 4
    }
    result = []
    count_states = [[0, 0, 0, 0]]
    # build counter states timeline ??
    for i, nucleotide in enumerate(seq):
        count_states.append(list(count_states[i]))
        count_states[i + 1][factors[nucleotide] - 1] += 1
    # main query processing loop
    for i, _ in enumerate(seq_end):

        # query substring is of length 1
        if seq_beg[i] == seq_end[i]:
            result.append(factors[seq[seq_beg[i]]])

        else:

            # get counter states before and after
            state_after_end = count_states[seq_end[i] + 1]
            state_before_beg = count_states[seq_beg[i]]
            # find minimum impact nucleotide in query substring
            for j in range(4):
                if state_after_end[j] > state_before_beg[j]:
                    result.append(j + 1)
                    break

    return result
```

[다음 기사](https://medium.com/@deck451/codility-algorithm-practice-lesson-5-prefix-sums-task-3-genomic-range-query-a-python-approach-83841ec3dec7)를 참고해서 가져왔는데 와 진짜 모르겠다!! 분명 예시까지는 알아들었는데 막상 코드 얘기하니까 왜... 모르겠지? ㅎㅎ;; 중간에 물음표 해 둔 단락이 이해가 안 간다... 실제 배열로 표현해줘 ㅜㅜ.. 일단 알아듣겠는건 2차원 배열로 나타냈다는 거 하나..

<br/>

```python
def solution(S, P, Q):
    N, M = len(S), len(Q)

    answer = [0] * M
    Al, Cl, Gl = [0] * N, [0] * N, [0] * N
    A, C, G = -1, -1, -1

    for i, x in enumerate(S):
        if x == "A": A = i
        if x == "C": C = i
        if x == "G": G = i
        Al[i], Cl[i], Gl[i] = A, C, G

    for i in range(M):
        answer[i] = 4
        if Al[Q[i]] <= Q[i] and Al[Q[i]] >= P[i]:
            answer[i] = 1
            continue
        if Cl[Q[i]] <= Q[i] and Cl[Q[i]] >= P[i]:
            answer[i] = 2
            continue       
        if Gl[Q[i]] <= Q[i] and Gl[Q[i]] >= P[i]:
            answer[i] = 3
            continue

    return answer
```

그래서 찾다가 그나마 이해가 조금 더 잘 가는 걸 가져왔다. 좀 더 직관적으로 보이기는 한데 `for i, x in enumerate(S)` 부분 알아는 듣겠는데... 그래서 이게,, 어떻게 된다는 건지 잘 모르겠다 살려줘,, ㅜㅜ

![스크린샷 20220817 오후 11 41 38](https://user-images.githubusercontent.com/72901045/185167717-81d0fa0e-1ea4-4772-a7c6-40b308d22d67.png)

해보니까 어떤 식으로 동작하는지는 알겠다. 근데 이걸 내가 생각하는 건... 매우 어려울 것 같다 ^ㅡㅜ

<br/>

### 정리

`a[start: end: step]` 연속적인 객체들에(예: 리스트, 튜플, 문자열) 범위를 지정해 선택해서 객체들을 가져오는 방법 및 표기법. 끝 인덱스는 가져오려는 범위에 포함되지 않기 때문에 실제로 가져오려는 인덱스보다 1을 더 크게 지정해야 함

ex) a = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90], a[0:4] => [0, 10, 20, 30]

`A.append(obj)` 배열의 가장 마지막에 obj 추가

`enumerate()` 인자로 넘어온 목록을 기준으로 인덱스와 원소를 차례대로 접근하게 해주는 반복자 객체를 반환해주는 함수. [다음 블로그](https://www.daleseo.com/python-enumerate/)에서 자세한 내용을 확인할 수 있다

`Dictionary` {Key1:Value1, Key2:Value2, Key3:Value3, ...}. a[Key]로 입력해서 Key에 해당하는 Value를 얻음. [딕셔너리 관련 자세한 정보](https://wikidocs.net/16)
<br/>

### 느낀점

정석으로 접근하는 거 너무 어렵다..............!!!!!!!!!!! 문제 이해하는데만 사용하고 내 코드 작성에도 별로 안 썼던 노트를 다른 코드 분석할 때 쓰다니 ^ㅡ^... 근데 진짜 예시를 통해 적지 않으면 이해가 안 갔다,, 다시 한 번 감탄나오는 코드 접근 방식,, 근데 이 코드 이해하고(여기에 시간을 매~~우 많이 쏟았다 ㅋㅋ) 정리하니까 하루를 넘겨버렸다 흠,, 내일 이걸로 진행해야 하는데 ㄱ-.. ㅜㅜ
