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




따라서 나는 다음과 같은 방법으로 조금 더 효율적인 알고리즘을 구상하였다.

주어진 number의 앞 숫자부터 차례대로 answer에 이어 붙인다.

이 때 붙이려는 수를 $C$, 제거 횟수를 $K$, 그리고 제거 후 길이를 $L = len(number) - K$ 로 정의하자. 

만약 $C$가 마지막 수보다 작은 경우, 현재 answer의 길이가 $L$보다 작다면 answer에 $C$를 이어붙인다. 현재 answer의 길이가 이미 $L$이 되었다면 더이상 수를 이어붙일 수 없으므로 $C$를 버린다 (이 때 $C$라는 수를 제거했기 때문에 $K$가 차감된다). 

$C$가 answer의 마지막 수보다 큰 경우에는 answer의 뒤에서부터 $C$보다 큰 수가 나올 때 까지 차례대로 숫자를 제거한다 (마찬가지로 answer의 숫자를 제거할 때마다 $K$를 1씩 감소시킨다). 


위 알고리즘을 코드로 구현한 것이 아래와 두 풀이이다. 

### python 코드

```python
# Solution 1
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
# Solution 2
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

아래는 삽질 열심히 한 흔적들이다..

```python
# 시간 초과 using combination

채점 결과
정확성: 33.3
합계: 33.3 / 100.0

from itertools import combinations
def solution(number, k):
    tmp = [x for x in number]
    comb_list = sorted([int(''.join(x)) for x in list(combinations(tmp, len(tmp)-k))])
    return str(comb_list[-1])

=====================================================================

# 시간 초과 using recursive function 

채점 결과
정확성: 58.3
합계: 58.3 / 100.0

def func(sliced, l):
    if l == 1:
        return max(sliced)
    len_sliced = len(sliced)
    tmp_sliced = sorted(list(enumerate(sliced)), key=lambda x: x[1], reverse=True)

    for pair in tmp_sliced:
        if len_sliced - pair[0] >= l:
            return str(pair[1]) + str(func(sliced[pair[0]+1:], l-1))
    return []


def solution(number, k):
    tmp = [int(x) for x in number]
    if len(number)-1 == k:
        return str(max(tmp))
    answer = func(tmp, len(number) - k)
    return answer

=====================================================================

# 시간 초과....

채점 결과
정확성: 75.0
합계: 75.0 / 100.0

def solution(number, k):
    def find_max(tup):
        max_tup = tup[0]
        for t in tup:
            if t[0] > max_tup[0]:
                max_tup = t
        return max_tup

    tmp = [int(x) for x in number]
    l = len(number)
    if l-1 == k:
        return str(max(tmp))
    length = l-k
    tmp_list = list(enumerate(tmp))
    tmp_list = [(v, i) for (i, v) in tmp_list]
    org_list = [i for i in tmp_list]
    answer = ''
    while True:
        if length == 1:
            answer += str(max(tmp_list)[0])
            break
        max_val = find_max(tmp_list[:-length+1])
        answer += str(max_val[0])
        tmp_list = org_list[max_val[1]+1:]
        length -= 1
    return answer

=====================================================================

# 또 시간 초과...

채점 결과
정확성: 91.7
합계: 91.7 / 100.0

def solution(number, k):
    def find_max(start, end):
        max_tup = tmp_list[start]
        for i in range(start, end + 1):
            if tmp_list[i][0] > max_tup[0]:
                max_tup = tmp_list[i]
        return max_tup
    l = len(number)
    if l-1 == k:
        return max(number)
    num_to_pick = l-k
    tmp_list = [(int(v), i) for (i, v) in enumerate(number)]
    answer = ''
    start_ind = 0
    while True:
        if num_to_pick == 1:
            answer += str(find_max(start_ind, l-1)[0])
            break
        max_val = find_max(start_ind, l - num_to_pick)
        answer += str(max_val[0])
        start_ind = max_val[1]+1
        num_to_pick -= 1
    return answer


=====================================================================

# 찐막 가즈아~

채점 결과
정확성: 100.0
합계: 100.0 / 100.0

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