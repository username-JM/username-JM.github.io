---
title: "[Programmers] 거리두기 확인하기"
classes: wide
use_math: true
categories:
- Coding Test
tags:
- algorithm
- Python
- coding test
- 알고리즘 문제
- 프로그래머스
- Programmers
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/81302?language=python3)


## 문제 해설

주석 참고.

### python 코드


```python
from itertools import combinations
def solution(places):
    return [check_dist(p) for p in places]  # 모든 반에 대해 반복

def check_dist(place):
    p_ind = []
    empt_ind = []
    for i in range(5):
        for j in range(5):
            if place[i][j] == "P":  # 사람의 위치 정보 저장
                p_ind.append([i, j])
            elif place[i][j] == "O": # 빈 자리의 위치 정보 저장
                empt_ind.append([i, j])
    if not p_ind or len(p_ind)==1:   # 사람이 없거나 한 명만 있는 경우 무조건 거리 두기 조건 만족.
        return 1
    comb = list(combinations(p_ind, 2)) # 사람끼리의 모든 조합 구성
    for c in comb:
        dist = get_dist(c[0], c[1]) # 두 사람의 거리 계산
        if dist == 1:   # 거리가 1인 경우 거리 두기 위반
            return 0
        elif dist == 2:
            if not check_social_dist(c[0], c[1], empt_ind, place):  # 거리가 2인 경우 가림막의 유무 확인 필요. 
                return 0
    return 1


def check_social_dist(p1, p2, empt_ind, place):
    for e in empt_ind:
        if get_dist(p1, e) == 1 and get_dist(p2, e) == 1:   # 두 사람으로 부터 같은 거리(1)에 빈 테이블이 있는 경우 위반.
            return False
    return True # 거리 두기 만족
    

def get_dist(s, t):
    return abs(s[0]-t[0]) + abs(s[1]-t[1])  # 맨허튼 거리 구하기
    
```
