---
title: "[Programmers] 타겟 넘버"
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
- 타겟 넘버
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/43165)


## 문제 해설

None

### python 코드

```python

def solution(numbers, target):
    return func(numbers, target, 0)


def func(numbers, target, val):
    if len(numbers) == 1:
        if val + numbers[0] == target or val - numbers[0] == target:
            return 1
        else:
            return 0
    else:
        return func(numbers[1:], target, val + numbers[0]) + func(numbers[1:], target, val - numbers[0])
    
```
