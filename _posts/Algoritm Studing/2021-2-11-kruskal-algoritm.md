---
title: "알고리즘: 크루스칼 알고리즘(Kruskal alghritm)"
description: "크루스칼 알고리즘에 대해 공부하고 정리한 내용입니다."
comments: true
category: [Algorithm Studying]
tag: [Algorithm, Python]
---

> 이 포스트는 [나동빈 님의 강좌](https://www.youtube.com/watch?v=qQ5iLNjpxSk&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz)를 보고 정리한 내용입니다.

# 크루스칼 알고리즘(Kruskal alghritm)

크루스칼 알고리즘이란 가장 적은 비용으로 모든 노드를 연결하기 위해 사용하는 알고리즘이다. 즉 **최소 비용 신장 트리(Minimum Spanning Tree; MST)**를 만들기 위한 알고리즘으로, 탐욕법을 이용하는 방법 중 하나이다.

* 신장 트리(Spanning Tree)란 사이클을 형성하지 않는 트리를 말한다. 즉, A-B-C-A와 같이 순환되는 경로가 없는 트리를 말한다.

![graph](https://user-images.githubusercontent.com/55024033/107632177-7e598180-6ca9-11eb-9e07-87c79319aef6.png)

```python
nodes = [1, 2, 3, 4, 5, 6, 7]

# 임의의 i에 대해 
# edges[i][0], edges[i][1]은 간선이 연결된 노드를 의미하고
# edges[i][2]는 해당 간선의 비용을 의미한다
edges = [
    (1, 2, 20),
    (1, 5, 27),
    (1, 7, 68),
    (2, 3, 31),
    (2, 4, 84),
    (3, 4, 45),
    (4, 6, 42),
    (4, 7, 33),
    (5, 6, 56),
    (5, 7, 12)
]
```

원은 그래프의 각 노드를 의미하고 선은 노드간의 연결, 즉 간선(edge)을 의미한다. 간선 위의 숫자는 각 간선에 부여된 비용을 의미한다. 이 비용이 최소가 되도록 모든 노드를 연결할 때 크루스칼 알고리즘을 사용한다.

크루스칼 알고리즘의 순서는 다음과 같다.

1. 모든 간선을 비용을 기준으로 오름차순 정렬한다.
2. 간선을 차례대로 그래프에 포함시킨다.
3. 이때, 사이클이 발생하는 경우 간선을 포함시키지 않는다.

사이클 여부를 확인하기 위해 **유니온-파인드(서로소 집합 자료구조)**를 사용한다. 간선을 추가할 때마다 간선에 연결된 두 노드를 하나의 집합으로 합치는데, 만약 두 노드가 이미 같은 집합이라면 해당 간선을 추가할 시 사이클을 형성한다는 의미이므로 그 간선은 무시한다.

```python
# 이전에 구현한 서로소 집합 자료구조를 그대로 사용
class disjoint_set:
    def __init__(self, array:list):
        self.data = {}
        for v in array:
            self.data[v] = v
    
    def find(self, x):
        if self.data[x] == x:
            return x
        
        return self.find(self.data[x])
    
    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        self.data[y_root] = x_root

        
def kruskal(nodes, edges):
    ds = disjoint_set(nodes)
    mst_edges = []
    
    # 비용을 기준으로 오름차순 정렬
    _edges = sorted(edges, key=lambda x: x[2])
    
    for edge in _edges:
        # 두 노드가 같은 그래프라면 무시
        if ds.find(edge[0]) == ds.find(edge[1]):
            continue
        
        ds.union(edge[0], edge[1])
        mst_edges.append(edge)
        print("추가된 간선:", edge)
        
        # 최소 비용 신장 트리의 간선 갯수는 노드 갯수 - 1 이므로
        # 최소 비용 신장 트리가 완성되었다면 break한다
        if len(mst_edges) == len(nodes) - 1:
            break
    
    return mst_edges
```

```python
mst_edges = kruskal(nodes, edges)
print("최소 비용 신장 트리의 간선:", mst_edges)

# <출력>
# 추가된 간선: (5, 7, 12)
# 추가된 간선: (1, 2, 20)
# 추가된 간선: (1, 5, 27)
# 추가된 간선: (2, 3, 31)
# 추가된 간선: (4, 7, 33)
# 추가된 간선: (4, 6, 42)
# 최소 비용 신장 트리의 간선: [(5, 7, 12), (1, 2, 20), (1, 5, 27), (2, 3, 31), (4, 7, 33), (4, 6, 42)]
```



![MST](https://user-images.githubusercontent.com/55024033/107636666-502b7000-6cb0-11eb-93cb-ac44217c6b42.png)

