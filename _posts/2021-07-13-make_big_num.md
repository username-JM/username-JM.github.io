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

주어진 string 형태의 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하는 문제이다. 
단 숫자의 순서는 바뀌어서는 안된다. 

가장 쉽게 접근할 수 있는 방법으로는 입력 숫자의 길이가 l일 때 (l-k) 길이의 모든 숫자 조합을 구하여 이들 중 max값을 찾으면 된다. 하지만 입력 숫자의 길이가 길어질수록 조합을 구하는데 많은 연산이 필요하기 때문에 이 방법으로 문제를 해결하게 되면 정답이 될 수는 있으나 효율성에서 통과하기 어렵다. 

따라서 조금 더 빠른 알고리즘 설계가 필요하다. 



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