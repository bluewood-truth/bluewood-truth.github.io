---
title: "알고리즘: 너비 우선 탐색(BFS), 깊이 우선 탐색(DFS)"
description: "탐색 알고리즘 대해 공부하고 정리한 내용입니다."
comments: true
category: [Algorithm Studying]
tag: [Algorithm, Python]
---

너비 우선 탐색과 깊이 우선 탐색은 정해진 순서에 따라 그래프의 각 노드를 방문하는 **맹목적 탐색(blind search)**의 대표적인 알고리즘이다. 이 포스트에서는 아래와 같은 그래프를 이용하여 두 알고리즘을 설명한다.

![graph](https://user-images.githubusercontent.com/55024033/107477986-137e4c80-6bbc-11eb-8544-57735c190de1.png)

```python
graph = {
    "A": ["B", "C", "D"],
    "B": ["E", "F"],
    "C": [],
    "D": ["G", "H"],
    "E": [],
    "F": [],
    "G": [],
    "H": ["G"]
}
```

<br>

# 너비 우선 탐색(Breath First Search; BFS) [🎥](https://en.wikipedia.org/wiki/Breadth-first_search#/media/File:Animated_BFS.gif)

탐색을 할 때 너비를 우선해서 탐색하는 알고리즘이다. 탐색 과정에서 **큐(Queue)**를 이용한다. 주로 최단경로를 찾을 때 사용한다.

너비 우선 탐색은 다음 수행을 반복하며 이루어진다.

1. 큐에서 노드를 하나 꺼내 방문한다.
2. 해당 노드에 연결된 노드 중 아직 발견되지 않은 노드를 큐에 추가한다.

![bfs](https://user-images.githubusercontent.com/55024033/107478223-753eb680-6bbc-11eb-917c-590eedfb4c85.png)

1. A를 큐에 추가하고 탐색을 시작한다.
2. 큐에서 A를 꺼내 방문하고, A의 다음 노드인 B, C, D를 큐에 추가한다.
3. 큐에서 B를 꺼내 방문하고, B의 다음 노드인 E, F를 큐에 추가한다.
4. 큐에서 C를 꺼내 방문한다.
5. 큐에서 D를 꺼내 방문한다. D의 다음 노드인 G, H를 큐에 추가한다.
6. 큐에서 E를 꺼내 방문한다.
7. 큐에서 F를 꺼내 방문한다.
8. 큐에서 G를 꺼내 방문한다.
9. 큐에서 H를 꺼내 방문한다. H의 다음 노드인 G는 이미 방문했으므로 무시한다.

따라서 방문하는 순서는 A -> B -> C -> D -> E -> F -> G -> H이다.

```python
import collections

def bfs(graph, start):
    discorvered = [start]
    queue = collections.deque([start])
    while queue:
        print("큐:", list(queue))
        node = queue.popleft()
        for next_node in graph[node]:
            if next_node not in discorvered:
                discorvered.append(next_node)
                queue.append(next_node)
                
    return discorvered
```

```python
visit_order = bfs(graph, "A")
print("방문 순서:", visit_order)

# <출력>
# 큐: ['A']
# 큐: ['B', 'C', 'D']
# 큐: ['C', 'D', 'E', 'F']
# 큐: ['D', 'E', 'F']
# 큐: ['E', 'F', 'G', 'H']
# 큐: ['F', 'G', 'H']
# 큐: ['G', 'H']
# 큐: ['H']
# 방문 순서: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
```

<br>

# 깊이 우선 탐색(Depth First Search; DFS) [🎥](https://en.wikipedia.org/wiki/File:Depth-First-Search.gif)

탐색을 할 때 깊이를 우선해서 탐색하는 알고리즘이다. 깊이 우선 탐색에는 **스택(Stack)**이 사용되며, 재귀로도 구현이 가능하다. (※ BFS는 재귀로 풀이할 수 없다)

깊이 우선 탐색은 다음 수행을 반복하며 이루어진다.

1. 스택에서 노드를 하나 꺼낸다.
2. 해당 노드에 방문하지 않았다면 방문하고, 해당 노드의 다음 노드들을 스택에 추가한다.

![dfs](https://user-images.githubusercontent.com/55024033/107478249-7ff94b80-6bbc-11eb-8f6f-9357a4ddfeaa.png)

(스택이냐 재귀냐에 따라 방문 순서가 바뀔 수는 있으나 깊이 우선 탐색이라는 점은 동일하다. 아래는 스택을 기준으로 한 설명이다.)

1. A를 스택에 추가하고 탐색을 시작한다.
2. 스택에서 A를 꺼낸다. A를 방문하고 B, C, D를 스택에 추가한다.
3. 스택에서 D를 꺼낸다. D를 방문하고 G, H를 스택에 추가한다.
4. 스택에서 H를 꺼낸다. H를 방문하고 G를 스택에 추가한다.
5. 스택에서 G를 꺼낸다. G를 방문한다.
6. 스택에서 G를 꺼낸다. 이미 G를 방문했으므로 무시한다.
7. 스택에서 C를 꺼낸다. C를 방문한다.
8. 스택에서 B를 꺼낸다. B를 방문하고 E, F를 스택에 추가한다.
9. 스택에서 F를 꺼낸다. F를 방문한다.
10. 스택에서 E를 꺼낸다. E를 방문한다.

따라서 방문 순서는 A -> D -> H -> G -> C -> B -> F -> E이다.

(재귀의 호출 순서는 스택과 방향이 반대다. 따라서 방문 순서는 A -> B -> E -> F -> C -> D -> G -> H가 된다.)

- 스택을 이용한 구현

```python
def dfs(graph, start):
    visited = []
    stack = [start]
    while stack:
        print("스택:", stack)
        node = stack.pop()
        if node not in visited:
            visited.append(node)
            for next_node in graph[node]:
                stack.append(next_node)
    
    return visited
```

```python
visit_order = dfs(graph, "A")
print("방문 순서:", visit_order)

# <출력>
# 스택: ['A']
# 스택: ['B', 'C', 'D']
# 스택: ['B', 'C', 'G', 'H']
# 스택: ['B', 'C', 'G', 'G']
# 스택: ['B', 'C', 'G']
# 스택: ['B', 'C']
# 스택: ['B']
# 스택: ['E', 'F']
# 스택: ['E']
# ['A', 'D', 'H', 'G', 'C', 'B', 'F', 'E']
```

- 재귀를 이용한 구현

```python
def dfs(graph, node, visited=[]):
    visited.append(node)
    for next_node in graph[node]:
        if next_node not in visited:
            dfs(graph, next_node, visited)
    
    return visited
```

```python
visit_order = dfs(graph, "A")
print("방문 순서:", visit_order)

# <출력>
# 방문 순서: ['A', 'B', 'E', 'F', 'C', 'D', 'G', 'H']
```