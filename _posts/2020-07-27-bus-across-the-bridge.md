---
title: "SW Expert Academy -  다리를 지나는 트럭"
excerpt: "[Python] 코딩테스트 - 스택/큐 문제"
classes: wide

categories:
- Coding Test
tags:
- algorithm
- Python
- 알고리즘 문제
- SW Expert Academy 풀이
- 다리를 지나는 트럭
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/42583)

## 문제 정리

다리의 길이와 견딜 수 있는 무게, 그리고 트럭 리스트가 주어졌을 때 모든 트럭들이 다리를 건너는데 소요되는 최소 시간을 구하는 문제이다. 이 때 트럭들은 1초에 1의 길이만큼 이동한다. 

***
## 문제 해설

다른 효율적인 방법이 많겠지만 직관적인, 나쁘게 말하면 무식한 방법으로 알고리즘을 설계하였다. 

다리 길이가 2, 견딜 수 있는 무게가 10, 트럭이 [7,4,5,2]로 주어졌다고 해보자. 

> Initialization

우선 큐에 다리의 길이만큼 0을 집어넣었다. 그리고 다리가 견디고 있는 무게를 s로 정의하고 시간을 time으로 설정하였다. s와 time의 초기값은 모두 0인 상태이다. 

- s = 0
- time = 0
- truck_weights = [7,4,5,2]

> 0s : [0, 0]

우선 시간을 1 증가 시킨다. 그 후 큐에서 값을 pop시킨다. 먼저 들어간 0이 빠져나올 것이다. 빠져나온 값을 다리가 견디고 있는 무게 s에서 빼준다. 어차피 0을 뺐기 때문에 여전히 s=0이다.    
이제 트럭을 넣을 차례이다. 트럭을 넣기 전에 항상 다리가 견딜 수 있는지 확인을 한다. 다리가 현재 견디는 무게인 s와 넣고자 하는 트럭의 무게를 더해 다리가 견딜 수 있는 무게인 10보다 작은지 판단한다. 0+7 < 10, 따라서 트럭을 queue에 push한다. 이 때 s에도 7을 더해준다(다리 위에 트럭이 올라갔기 때문에 무게 업데이트). 

> 1s : [7, 0]

시간을 증가하고, 큐에서 0을 pop한다. s는 그대로 7인 상태이다.    
트럭을 넣기 전에 무게를 계산하는데 다음 트럭의 무게가 4이므로 7+4 > 10, 즉 트럭을 보낼 수 없다. 따라서 이런 경우에는 0을 집어넣는다. 

> 2s : [0, 7]

시간 증가는 항상 이루어지니 이제 생략하겠다. 큐에서 7을 pop한다. s는 0이 된다. 다음 트럭은 무게가 4이므로 4 + 0 < 10, 따라서 트럭을 보낸다. s는 4가 된다.

> 3s : [4, 0]

큐에서 0을 pop한다. s는 4이다. 다음 트럭은 무게가 5이기 때문에 4+5 < 10, 따라서 트럭을 보낼 수 있다. s는 9로 증가한다. 

> 4s : [5, 4]

큐에서 4를 pop한다. s는 5이다. 다음 트럭은 무게가 6이다. 6+5 > 10, 따라서 트럭을 보낼 수 없어 0을 집어넣는다. 

> 5s : [0, 5]

큐에서 5를 pop한다. s는 0이 된다. 다음 트럭의 무게는 6이다. 6 + 0 < 10, 따라서 트럭을 보낸다. 

이 때 time = 6, truck_weights = [] 가 된다. 즉 더이상 트럭이 없다. 이 경우 방금 출발한 무게 6의 트럭만 다리를 건너면 종료되므로 time에 다리의 길이인 2를 더해주고 반복문을 탈출한다. 즉 현재 time인 6에 다리의 길이 2를 더해 time=8이 되고, 알고리즘은 종료된다. 


### python 코드

```python

from queue import Queue

def solution(bridge_length, weight, truck_weights):
    q = Queue()
    # initialize
    s = 0;
    time = 0
    for i in range(bridge_length): #큐를 0으로 채운다. 
        q.put(0)
    while True:
        time += 1
        s -= q.get() # 큐에서 나갈 트럭 제거
        if s + truck_weights[0] > weight: #무게 초과?
            q.put(0)
            continue
        if len(truck_weights) == 1: #마지막 트럭?
            time += bridge_length
            break
        tmp = truck_weights.pop(0)  # 새로 넣을 트럭을 추출
        s += tmp
        q.put(tmp)
    return time
    
```
