---
title: "알고리즘: 스택과 큐 (Stack and Queue)"
description: "스택과 큐에 대해 정리한 내용입니다."
comments: true
category: [Algorithm Studying]
tag: [Algorithm, Python]
---

# 스택(Stack)

자료를 쌓아 올린 다음 위에서부터 하나씩 꺼내서 쓸 수 있는 자료구조이다. 나중에 들어온 자료가 먼저 나간다는 의미로 **후입선출(Last In First Out; LIFO)** 방식이라고 한다. 스택은 다음 두 가지 연산을 통해 자료를 추가하고 제거한다.

- `.push()`: 스택의 맨 위에 데이터를 추가한다.
- `.pop()`: 스택의 맨 위에 있는 데이터를 삭제하고 해당 값을 return한다.

파이썬에서는 기본 자료형인 **리스트(List)**를 스택처럼 사용 가능하다. `.push()` 연산은 `.append()`로 대체되고, `.pop()` 연산은 파이썬 리스트에도 기능과 명칭이 동일한 연산이 존재한다.

```python
stack = []
stack.append(1)
stack.append(2)
stack.append(3)
print("현재 스택:", stack)

while stack:
    print("꺼낸 값:", stack.pop(), "/ 현재 스택:", stack)

# <출력>
# 현재 스택: [1, 2, 3]
# 꺼낸 값: 3 / 현재 스택: [1, 2]
# 꺼낸 값: 2 / 현재 스택: [1]
# 꺼낸 값: 1 / 현재 스택: []
```

`.pop()`하면 추가한 순서의 역순으로 데이터를 꺼내는 것을 알 수 있다.

<br>

# 큐(Queue)

대기열이라는 단어 자체의 의미처럼, 차례로 데이터를 넣고 먼저 들어온 순서대로 데이터를 꺼낼 수 있는 자료구조이다. 먼저 들어온 데이터가 먼저 나간다는 의미로 **선입선출(First In First Out; FIFO)** 방식이라고 한다. 큐는 다음 두 가지 연산을 통해 데이터를 추가하고 제거한다.

* `.enqueue()`: 큐의 맨 뒤에 데이터를 추가한다.
* `.dequeue()`: 큐의 맨 앞에서 데이터를 삭제하고 해당 값을 return한다.

파이썬으로 큐를 사용하기 위해서는 `collections` 모듈의 **데크(deque)**를 사용해야 한다.  리스트도 `.append()`와 `.pop(0)`을 통해 큐처럼 사용할 수는 있지만, 리스트의 맨 뒤 값을 꺼내는 `.pop()` 연산은 *O(1)* 연산인 반면 리스트 맨 앞의 값을 꺼내는 `.pop(0)` 연산은 ***O(n)*** 연산이다. 따라서 성능이 훨씬 떨어진다.

데크는 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조로, 큐로서 사용할 경우에는 `.append()` 연산으로 추가하고 `.popleft()` 연산으로 삭제한다.

```python
import collections

queue = collections.deque()
queue.append(1)
queue.append(2)
queue.append(3)
print("현재 큐:", queue)

while queue:
    print("꺼낸 값:", queue.popleft(), "/ 현재 큐:", queue)

# <출력>
# 현재 큐: deque([1, 2, 3])
# 꺼낸 값: 1 / 현재 큐: deque([2, 3])
# 꺼낸 값: 2 / 현재 큐: deque([3])
# 꺼낸 값: 3 / 현재 큐: deque([])
```

데이터를 넣은 순서대로 꺼내는 것을 알 수 있다.