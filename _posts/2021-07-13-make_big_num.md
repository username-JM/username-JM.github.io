---
title: "[Programmers] 큰 수 만들기"
classes: wide

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
- greedy
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/42883)


## 문제 해설

None

### python 코드

```python

def solution(number, k):
    answer = ""
    for c in number:
        if not answer or answer[-1] >= c:
            if len(answer) < len(number)-k:
                answer += c
        else:
            while answer and k > 0 and answer[-1] < c:
                answer = answer[:-1]
                k -= 1
            answer += c

    return answer
    
```


```python

def solution(number, k):
    answer, number = pop(number)
    while k > 0 and number:
        n, number = pop(number)
        if answer[-1] < n:
            while answer and n > answer[-1]:
                answer = answer[:-1]
                k -= 1
                if k == 0:
                    break
            answer += n
        else:
            answer += n
    if len(number) > 0:
        answer += number
    elif k != 0:
        answer = answer[:-k]
    return answer

def pop(n_list):
    return n_list[0], n_list[1:]

```