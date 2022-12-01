---
title: "[Programmers] 행렬 테두리 회전하기"
classes: wide
use_math: true
categories:
- Computer Engineering
tags:
- algorithm
- Python
- coding test
- 알고리즘 문제
- 프로그래머스
- Programmers
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/lessons/77485)


## 문제 해설

None

### python 코드

```python

def solution(rows, columns, queries):
    answer = []
    mat = []
    tmp = []
    # build matrix
    for i in range(rows * columns):
        if (i + 1) % columns == 0:
            tmp.append(i + 1)
            mat.append(tmp)
            tmp = []
        else:
            tmp.append(i + 1)
    # mat = [[i+(j)*columns for i in range(1,columns+1)] for j in range(rows)]
    
    
    for q in range(len(queries)):
        index_list = []
        num_list = []
        sr, sc, er, ec = [x - 1 for x in queries[q]]  # 2,2,5,4
        for i in range(sc, ec):  # move right
            num_list.append(mat[sr][i])  # mat[2][2~4]
            index_list.append([sr, i])
        for i in range(sr, er):  # move down
            num_list.append(mat[i][ec])  # mat[2~5][4]
            index_list.append([i, ec])
        for i in range(ec, sc, -1):  # move left
            num_list.append(mat[er][i])  # mat[5][4~2]
            index_list.append([er, i])
        for i in range(er, sr, -1):  # move up
            num_list.append(mat[i][sc])  # mat[5~2][2]
            index_list.append([i, sc])
        # find min value of rotated numbers
        answer.append(min(num_list))
        # rotation
        num_list = [num_list[-1]] + num_list[:-1]
        # matrix update
        for i, (x, y) in enumerate(index_list):
            mat[x][y] = num_list[i]
    return answer
    
```
