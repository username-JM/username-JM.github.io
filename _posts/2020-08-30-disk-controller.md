---
title: "SW Expert Academy -  디스크 컨트롤러"
excerpt: "[Python] 코딩테스트 - 힙 문제"
classes: wide

categories:
- Coding Test
tags:
- algorithm
- Python
- 알고리즘 문제
- SW Expert Academy 풀이
- 디스크 컨트롤러
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/42627)


***
## 문제 해설

프로세스의 작업 요청 시점과 작업 소요 시간이 주어졌을 때, 요청부터 작업 종료까지의 시간을 최소로 하도록 실행해주는 알고리즘을 설계하는 문제이다. 

실행 할 수 있는(작업 시점 기준 혹은 이전에 요청된 프로세스) 프로세스 중 작업 소요 시간이 가장 짧은 프로세스부터 처리하는게 핵심이다. 주의할 점은 작업 요청이 오지도 않았는데 소요 시간이 짧다고 먼저 처리하면 안된다. 

```python
answer = 0
time = 0
ps_list = sorted(jobs, key=lambda x : (x[0], x[1]))
waiting_ps = PriorityQueue()
```

현재 작업 시점을 표현하기 위해 "time" 변수를 선언했다. 그 후 실행 예정 프로세스 리스트(첫 번째 인자)를 작업 요청 시간 기준 오름차순 정렬 후 작업 소요 시간 기준 오름차순 정렬 하였다. 이렇게 하면 리스트의 앞으로 갈 수록 작업이 일찍 요청된, 소요 시간이 짧은 프로세스가 정렬되어 있다. 

또 "waiting_ps" 라는 priority queue를 선언했는데, 이는 작업이 요청 되었지만 우선순위가 밀려 실행 대기 중인 프로세스 리스트이다.     

waiting_ps에 데이터를 넣을 땐 [소요 시간, 요청 시간] 꼴로 넣는다. queue에 들어있는 프로세스들은 모두 실행 요청된 상태이므로 작업 소요 시간이 짧은 순으로 실행되면 되기 때문에 소요 시간을 key로 두기 위해 순서를 바꾸어 넣는다. 


```python
while len(ps_list) > 0 or not waiting_ps.empty():
    if waiting_ps.empty():
        ps = ps_list.pop(0)
        time = ps[0] + ps[1]
    else:
        ps = waiting_ps.get()
        ps = (ps[1], ps[0])
        time += ps[1]
    answer += time - ps[0]
    while len(ps_list) > 0:
        if time > ps_list[0][0]:
            tmp = ps_list.pop(0)
            waiting_ps.put((tmp[1], tmp[0]))
        else:
            break
```

프로세스 리스트인 ps_list와 대기 리스트인 waiting_ps가 모두 비어 있으면 더 이상 실행할 프로세스가 없기 때문에 while문의 조건을 위와 같이 설정한다.

만약 waiting_ps가 비어있다면, 실행 대기 중인 프로세스가 없다는 의미이므로 ps_list에서 요청 예정인 프로세스를 바로 실행한다. 해당 프로세스는 대기 없이 요청 시점에 바로 실행되기 때문에 실행 시간 및 종료 시간은 (작업 요청 시점)과 (작업 요청 시점 + 작업 소요 시간)이 된다. time의 값을 새로 업데이트 한다.

만약 waiting_ps에 대기 중인 프로세스가 있다면 우선순위가 높은 프로세스를 뽑아서 실행한다. 

실행한 프로세스의 요청 시점 ~ 종료 시점은 작업 종료 시점인 time - 작업 요청 시점인 ps[0]을 빼서 구해준다. 그 값을 answer에 더해준다. 

그 후, 새로 업데이트된 작업 시점 time에 대해, ps_list에서 time과 같은, 혹은 이전에 작업 요청된 프로세스가 있는지 확인한다. 있다면 waiting_ps에 넣는다. 


위 반복문이 모두 종료되면 answer에는 모든 프로세스의 작업 요청 시점 ~ 종료 시점까지의 소요 시간의 합이 저장되어 있을 것이다. 따라서 이를 프로세스의 수로 나누어(소수점 버림 처리) return 해준다. 

전체 코드는 아래와 같다. 


### python 코드

```python

from queue import PriorityQueue
def solution(jobs):
    answer = 0
    time = 0
    ps_list = sorted(jobs, key=lambda x : (x[0], x[1]))
    waiting_ps = PriorityQueue()

    while len(ps_list) > 0 or not waiting_ps.empty():
        if waiting_ps.empty():
            ps = ps_list.pop(0)
            time = ps[0] + ps[1]
        else:
            ps = waiting_ps.get()
            ps = (ps[1], ps[0])
            time += ps[1]

        answer += time - ps[0]
        print("time : " + str(time) + "  (" + str(ps[0]) + "," + str(ps[1]) + ")")

        while len(ps_list) > 0:
            if time > ps_list[0][0]:
                tmp = ps_list.pop(0)
                waiting_ps.put((tmp[1], tmp[0]))
            else:
                break

    return answer//len(jobs)
    
```
