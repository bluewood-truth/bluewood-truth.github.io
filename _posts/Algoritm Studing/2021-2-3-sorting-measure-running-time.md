---
title: "알고리즘: 정렬(Sorting) - 성능 확인"
description: "각 정렬 알고리즘의 성능을 확인하고 비교해봤습니다."
comments: true
category: [Algorithm Studying]
tag: [Algorithm, Python]
---

이전까지 공부하고 파이썬으로 구현했던 정렬 알고리즘들의 실제 실행 시간을 측정해봤다. 우선 함수의 실행 시간을 측정하기 위한 함수를 작성했다.

```python
import time
from typing import Callable
import copy

def measure_running_time(func:Callable, args:list, iteration:int = 1, average:bool = True) -> float:
    total_time = 0
    for _ in range(iteration):
        args_copy = copy.deepcopy(args)
        start = time.time()
        func(*args_copy)
        total_time += time.time() - start
    
    if average:
        total_time /= iteration
    
    return total_time
```

`func`과 `args`는 각각 실행할 함수와 그 함수에 전달할 인수를 의미한다. 시간 측정은 `iteration`으로 지정한 횟수만큼 이뤄지고, `average`의 True/False 여부에 따라 측정 시간(단위: 초)의 평균값 또는 합계를 return한다.

다음으로 정렬에 사용할 배열을 생성했다. 정렬된 배열(오름차순), 역순으로 정렬된 배열(내림차순), 무작위 배열 세 가지를 생성했고, 무작위 배열은 혹시나 편향으로 인해 성능에 영향이 있을까봐 세 가지 서로 다른 무작위 배열의 정렬 시간의 평균을 측정하기로 했다.

```python
import random

n = 10000
ascending_list = list(range(n))
descending_list = ascending_list[::-1]
random_list = [ random.sample(ascending_list, n) for _ in range(3) ]
```

다른 함수들은 지난 포스트에서 구현한 것을 그대로 사용했지만, 퀵 정렬은 그대로 사용하면 RecursionError가 발생했기에 재귀에서 반복으로 수정 후 사용했다.

<br>

# 측정 결과

| 정렬 알고리즘 | 무작위 배열 | 오름차순 배열 | 내림차순 배열 |
| :-----------: | :---------: | :-------: | :---------: |
|   **선택 정렬**   |   2.412s    |   2.418s    |  3.335s   |
|   **버블 정렬**   |   9.151s    |   4.702s    |  13.926s  |
|   **삽입 정렬**   |   7.938s    |  **0.000s** |  15.881s  |
|    **퀵 정렬**    |  **0.031s** |   5.691s    |  5.612s   |
|   **병합 정렬**   |   0.042s    |   0.047s    | **0.031s** |
|    **힙 정렬**    |   0.069s    |   0.078s    |  0.078s   |

* 무작위 배열에 대해서는 전체적으로 *O(n<sup>2</sup>)* 알고리즘(선택, 버블, 삽입)보다 *O(n log n)* 알고리즘(퀵, 병합, 힙)의 성능이 훨씬 좋았다.
* 무작위 배열에 대해 가장 좋은 성능을 보인 것은 **퀵 정렬**이다. 하지만 오름차순 배열이나 내림차순 배열에 대해서는 *O(n<sup>2</sup>)* 알고리즘과 비교될 만큼 나쁜 성능을 보였다. (실제로 최악의 경우 *O(n<sup>2</sup>)*이니까...)
* 전체적으로 좋은 성능을 보인 것은 **병합 정렬**이다. 어떤 배열이든 상관 없이 빠른 속도를 보였다.
* **힙 정렬**도 전체적으로 괜찮은 성능을 보였지만 병합 정렬보다는 조금 뒤떨어졌다. 단, 병합 정렬은 정렬 과정에서 추가적인 배열이 필요하다는 점을 감안하면 힙 정렬은 메모리 측면에서 이점이 있다.
* 이미 오름차순으로 정렬된 배열에 대해 가장 좋은 성능을 보인 것은 **삽입 정렬**이다. (최선의 경우 *O(n)*이니 당연하다) 단 무작위 배열이나 내림차순 배열에 대해서는 성능이 좋지 못한데, 완전 무작위가 아니라 어느 정도 정렬된 배열일수록 좋은 성능을 낼 것으로 보인다.

