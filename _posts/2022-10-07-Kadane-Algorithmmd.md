---

layout: post

title: Kadane’s Algorithm

summary: DP, BF, Kadane's Algorithm

---

배열에서 각각의 수를 더했을 때 가장 큰 수가 나오는 연속된 부분을 찾는 알고리즘이다. 이는 DP의 하나인데 DP가 뭔지 간략히 알아보도록 하자.

<br/>

### Dynamic Programming

[다음 기사](https://medium.com/@rsinghal757/kadanes-algorithm-dynamic-programming-how-and-why-does-it-work-3fd8849ed73d)에서 너무 귀엽게 설명한 부분이 있어서 소개해보려고 한다.

> 1+1+1+1+1+1+1+1 = 이 뭐게?
> 
> (세는 중) 8!
> 
> 여기에 1+를 하면?
> 
> 9!
> 
> 어떻게 그렇게 빨리 계산했어?
> 
> 이미 했던 거에 하나만 더하면 되니까~
> 
> 이렇듯 Dynamic Programming은 시간을 절약하기 위해서 어떤 걸 기억하는 것이다...

라고 훈훈하게 마무리되어 있는데 정말...... 너무 너무 너무 귀여운 예시이다 >< 이렇게 쉽고 깜찍하게 예를 들어 설명할 수 있는 사람이 되고싶다... 어쨌든

동적 프로그래밍(Dynamic Programming)은 **복잡한 문제를 간단한 하위 문제들의 모음으로 나누어 각각의 하위 문제를 한 번만 해결**하고 메모리 기반 데이터 구조(어레이, 맵 등)를 사용하여 **해결책을 저장**하는 방법이다. 따라서 다음에 동일한 하위 문제가 발생할 때 솔루션을 재계산하는 대신 이전에 계산한 솔루션을 찾아보는 것이므로 **계산 시간을 절약**할 수 있다.

<br/>

### Brute Force

![](https://miro.medium.com/max/1370/1*xm44zdqIv-pA2nGxCvbyBQ.png)

대부분 얘를 함께 설명하던데 결론부터 말하자면 시간 복잡도가 O(N^2)라 그다지 권장하는 방법은 아니다.

일단 알고리즘이 어떻게 되냐면 A[0]과 가능한 모든 부분합을 구하고 거기서 가장 큰 부분합을 저장한다. 이후 A[1], A[2], ... A[n-1]까지 반복하는데 그럼 각 인덱스마다 가장 큰 부분합 `maxIndex[i]`이 나올 것이다. 여기서 또 가장 큰 걸 찾으면 제일 큰 부분합 `max(maxIndex[0], maxIndex[1], ... maxIndex[n-1])`이 나온다.

편하긴 하겠으나 솔직히 딱봐도... 좋아보이지는 않는다 ㅋㅋㅋ 이래서 나온게 제목에도 쓴 Kadene's Algorithm이다.

<br/>

### Kadane's Algorithm

![](https://miro.medium.com/max/1400/1*0T4vufD3IKkBLC895NNtkA.png)

**각각의 최대 부분합은 이전의 최대 부분합이 반영된 결과값**이다. 

![](https://miro.medium.com/max/1352/1*UrQhblF8B-6QoEC6E7kWow.png)

그러니까 A[5]의 부분합은 A[4]의 부분합에 A[5]를 더하면 된다. 그럼 A[5]의 모든 부분합을 계산하지 않아도 A[5]의 부분합을 알 수 있는 것이다. 식으로 나타내면 

```python
localMaximum[i] = max(A[i], A[i] + lacalMaximum[i-1])
```

이다. 시간복잡도가 O(N)으로 Brute Force와 비교하면 굉장히 효율적인 방법임을 알 수 있다.

<br/>

- python
  
  [Codility MaxSliceSum](https://suyeon12.github.io/2022/10/04/codility-maxslicesum) 문제가 Kadane’s Algorithm을 활용한 것이다.

```python
def solution(A):
    sum = 0
    maxsum = A[0]
    for i in A:
        sum = max(sum + i, i)
        maxsum = max(maxsum, sum)
        # print(maxsum)
    return maxsum
```

[Codility MaxDoubleSliceSum](https://suyeon12.github.io/2022/10/06/codility-maxdoubleslicesum)

```python
def solution(A):
    left_sum = [0] * len(A)
    right_sum = [0] * len(A)
    max_sum = 0

    for i in range(1, len(A)-1):
        left_sum[i] = max(left_sum[i-1] + A[i], 0)
        # print(left_sum)

    for i in range(len(A)-2, 0, -1):
        right_sum[i] = max(right_sum[i+1] + A[i], 0)
        # print(right_sum)

    for i in range(1, len(A)-1):
        max_sum = max(max_sum, left_sum[i-1] + right_sum[i+1])
    
    return max_sum
```

<br/>

### 참고자료

[Kadane’s Algorithm (카데인 알고리즘)](https://medium.com/@vdongbin/kadanes-algorithm-카데인-알고리즘-acbc8c279f29)

[Kadane’s Algorithm — (Dynamic Programming) — How and Why does it Work?](https://medium.com/@rsinghal757/kadanes-algorithm-dynamic-programming-how-and-why-does-it-work-3fd8849ed73d)


