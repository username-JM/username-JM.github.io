---
title: "[Programmers] 뉴스 클러스터링"
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
- 카카오
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/17677)


## 문제 해설

주석 참고.

### python 코드


```python
def solution(str1, str2):
    i_val = 0
    u_val = 0

    # 두 단어를 2개의 문자 조합으로 구성. 딕셔너리 기반. key: 조합, value: 개수
    s1 = tokenize(str1.lower())
    s2 = tokenize(str2.lower())
    set1 = set(s1)
    set2 = set(s2)
    intersec = set1 & set2 # 교집합 리스트 구하기
    union = set1 | set2 # 합집합 리스트 구하기
    
    for i in intersec:
        i_val += min(s1[i], s2[i]) # 교집합 리스트에서 자카드 유사도 정의에 의해 최소값 구하기
    for u in union:
        if u in intersec:
            u_val += max(s1[u], s2[u])  # 교집합에 있는 조합인 경우 정의에 의해 최대값 구하기
        elif u in set1:
            u_val += s1[u]
        else:
            u_val += s2[u]
    
    return int(65536 * i_val / u_val) if u_val else 65536


def tokenize(s):
    dic = {}
    for i in range(len(s)-1):
        if is_char(s[i]) and is_char(s[i+1]):
            if s[i:i+2] in dic:
                dic[s[i:i+2]] += 1
            else:
                dic[s[i:i+2]] = 1
    return dic


def is_char(c):
    return True if 97 <= ord(c) <= 122 else False
    
```



### 다른 풀이

```python
from collections import Counter
def solution(str1, str2):
    # make sets
    s1 = [str1[i:i+2].lower() for i in range(len(str1)-1) if str1[i:i+2].isalpha()]
    s2 = [str2[i:i+2].lower() for i in range(len(str2)-1) if str2[i:i+2].isalpha()]
    if not s1 and not s2:
        return 65536
    c1 = Counter(s1)
    c2 = Counter(s2)
    answer = int(float(sum((c1&c2).values()))/float(sum((c1|c2).values())) * 65536)
    return answer

```
### 새로 알게된 점
1. is_alpha()로 손쉽게 알파벳으로만 구성되어있는지 확인할 수 있다. 와우!

2. c1 & c2 => intersection:  min(c1[x], c2[x])

3. c1 | c2 => union: max(c1[x], c2[x])