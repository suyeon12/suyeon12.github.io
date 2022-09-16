---

layout: post

title: Stack and Queue

summary: 자료구조 중 하나인 스택과 큐를 알아보고 파이썬을 통해 정리

tags: stack queue python

---

# stack

![Untitled](https://user-images.githubusercontent.com/72901045/190635952-f4ecf548-ae79-428a-b45d-ae7c9c17834f.png)

- LIFO (Last In First Out): 마지막으로 들어온 값이 처음으로 나감

- 완전히 꽉 찼을 때 `Overflow`, 완전히 비어 있으면 `Underflow`

- 그래프의 깊이 우선 탐색(DFS), 재귀적(Recursion) 함수를 호출 할 때 사용

<br/>

### push()

item을 스택 가장 윗 부분에 추가

1. 스택이 가득 찼는지 확인
2. 스택이 가득 차면 오류가 발생하고 종료
3. 스택이 가득 차지 않으면 Top을 증가
4. Top이 가리키는 스택 위치에 데이터를 추가

<br/>

### pop()

스택의 가장 위에 있는 item을 제거

1. 스택이 비어 있는지 확인
2. 스택이 비어 있으면 오류가 발생하고 종료
3. 스택이 비어 있지 않으면 Top 이 가리키는 데이터를 제거
4. Top 값을 감소
5. 성공을 반환

<br/>

### peek()

스택의 가장 위에 있는 item을 반환

cf) `pop`은 가장 위에 있는 item을 제거하는 것이지만 `peek`은 반환만 할 뿐 제거하지는 않음

<br/>

### isEmpty()

스택이 비어있을 때 true를 반환

<br/>

### python

파이썬의 경우 리스트를 스택처럼 사용함

```python
stack = []
stack.append(1) # push
stack.pop()
# pop, 제거한 원소를 리턴받을 수 있음
top = stack[-1]

# isEmpty
if len(stack) = 0:
  return True
else:
  return False
```

<br/>

<br/>

# Queue

![images-hanif-post-efe7e323-0986-4abe-8a38-4900194c7bc4-image](https://user-images.githubusercontent.com/72901045/190638538-2446b570-40fe-45d7-abad-0d3bef1b712c.png)

한 쪽에서는 삽입이 이루어지고(그림의 Rear) 한 쪽에서는 삭제(그림의 Front)만 이루어지는 유한 순서 리스트

- FIFO(First in First out)으로 먼저 들어온 것이 먼저 나가는 형식

- 너비 우선 탐색(BFS), 캐시 구현에 이용
  

<br/>

### enQueue()

큐에 데이터를 넣음

Rear의 값이 1 증가된 후 데이터가 삽입되고 큐가 꽉 차면 더 이상 삽입할 수 없음

<br/>

### deQueue()

큐에서 데이터를 뺌

front가 가리키는 데이터를 삭제한 뒤 front 값을 증가시킴

rear와 front의 위치가 동일하다면 큐에 남아있는 데이터가 없다는 의미로 더 이상 삭제하지 않음

<br/>

### peek()

가장 앞에 있는 원소를 삭제하지 않고 반환

cf) deQueue()는 데이터를 삭제한 후 읽고 peek()은 데이터를 삭제하지 않고 확인

<br/>

### isEmpty()

큐가 비어있는지 확인

비어있다면 true, 비어 있지 않다면 false

<br/>

### 큐의 다른 형식

- 원형 큐(circle queue)

- 우선순위 큐(priority queue)

<br/>

### python

- list

```python
queue = [1, 2, 3]
queue.append(4) # enqueue
queue.pop(0) # dequeue, print 1
```

`pop(0)`과 `insert(0, x)`는 시간복잡도가 O(N)이라 데이터 개수가 많아질수록 느려짐 -> 성능 면에서 불리

<br/>

- deque

```python
from collections import deque
# double-ended queue, 데이터를 양방향에서 추가하고 제거할 수 있음

queue = deque([1, 2, 3])
queue.append(4) # enqueue
queue.popleft() # dequeue, print 1

# appendleft(x): 데이터를 맨 앞에 삽입할 수 있음
queue.appendleft(0)

```

`popleft()`와 `appendleft(x)`는 시간복잡도가 O(1). 그러나 무작위 접근할 때는 시간복잡도가 O(N)

<br/>

- Queue

```python
from queue import Queue
# deque와 달리 방향성이 없어 데이터 추가와 삭제가 하나의 메서드로 처리

que = Queue()

# enqueue
que.put(1)
que.put(2)
que.put(3)

# dequeue
que.get() # print 1
que.get() # print 2
```

deque와 마찬가지로 데이터 추가와 삭제는 O(1), 데이터 접근은 O(N)
