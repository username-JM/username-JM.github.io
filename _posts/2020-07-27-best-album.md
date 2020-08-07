---
title: "SW Expert Academy - 베스트 앨범"
excerpt: "[Python] 코딩테스트 - 해시 문제"

categories:
- Coding Test
tags:
- algorithm
- Python
- 알고리즘 문제
- SW Expert Academy 풀이
- 베스트 앨범
- 파이썬
---

## [문제링크](https://programmers.co.kr/learn/courses/30/parts/12077)


```python

def sort_per_genres(target):
    for t in target:
        target[t] = quick_sort(target[t]) #각 장르별로 정렬 시도
        target[t].reverse() #재생 수가 오름차순, 고유번호로는 내림차순 정렬되었기 때문에 reverse해준다
    return target


def best_album(dict_songs, cnt_dict):
    ans = []
    while len(cnt_dict) > 0:
        max_genre = max(cnt_dict.values())
        for c in cnt_dict:
            if cnt_dict[c] == max_genre:
                max_genre = c
        ans.append(dict_songs[max_genre][0][1])
        if len(dict_songs[max_genre]) > 1:
            ans.append(dict_songs[max_genre][1][1])
        del cnt_dict[max_genre]
    return ans


def quick_sort(arr): #퀵소트 정렬 알고리즘
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2] # 가운데 값을 피봇, 즉 기준으로 두고
    lesser_arr, center, greater_arr = [], [], [] # 기준보다 작은 값들, 기준, 큰값으로 나눔
    for i in range(0, len(arr)):
        if arr[i][0] < pivot[0]: # 기준보다 작은 애들은 왼쪽으로 가라~
            lesser_arr.append(arr[i])
        elif arr[i][0] > pivot[0]: # 기준보다 큰 애들은 오른쪽으로 가라~
            greater_arr.append(arr[i])
        else: # 같은경우
            if arr[i][1] > pivot[1]: # 고유 번호를 비교해서 끝내림차순 정렬한다.
                lesser_arr.append(arr[i])
            elif arr[i][1] < pivot[1]:
                greater_arr.append(arr[i])
            else:
                center.append(pivot) # 고유번호도 같다? 자기자신
                continue
    return quick_sort(lesser_arr) + center + quick_sort(greater_arr) # 병합 과정


def solution(genres, plays):
    dict_songs = {} # {장르 : [[재생횟수, 고유번호], [재생횟수, 고유번호]]} 형태의 딕셔너리
    cnt_dict = {}   # 장르 별로 총 재생 횟수를 저장하는 딕셔너리
        #dict_songs = {"classic" : [[500, 1], [800, 0]], "pop" : [[2000, 2], [5000, 3]]}
        #cnt_dict = {"classic" : 1300, "pop" : 7000}

    for i in range(0, len(genres)):
        if genres[i] in dict_songs: #이미 있는 장르인가? 
            dict_songs[genres[i]].append([plays[i], i]) #해당 장르에 [재생수,고유번호]형태로 넣는다
            cnt_dict[genres[i]] += plays[i] #장르 별 총 재생 수를 구하기 위해 cnt_dict[해당장르]에 재생수 추가
        else:
            dict_songs[genres[i]] = [] #새로운 장르이기 때문에 해당 장르로 키 생성
            cnt_dict[genres[i]] = plays[i] #cnt_dict에도 해당 장르로 키 생성 및 재생 수 삽입
            dict_songs[genres[i]].append([plays[i], i]) #해당 장르에 재생수,고유번호 삽입
    dict_songs = sort_per_genres(dict_songs) #장르 별로 있는 곡들을 재생 수로 내림차순 정렬, 이 때 재생 수가 같으면 고유번호로 오름차순 정렬
    answer = best_album(dict_songs, cnt_dict)
    return answer


```


## 내장함수 sort()를 사용하지 않고 정의한 이유
[a,b] 형태의 데이터에 대해 a에 대해 내림차순 정렬하고 싶음 (a = 곡 재생 횟수). 하지만 a가 같은 경우에는 b에 대해 오름차순 정렬해야함 (b = 고유번호, 고유번호는 낮은게 먼저 출력되어야 하기 떄문).

따라서 정렬 함수를 따로 정의하여 a가 같은 경우 예외처리를 직접 해준다. 해당 함수는 오름차순,내림차순 정렬했기 때문에 후에 reverse함수를 통해 순서를 바꿔주면 된다. 근데 이럴바에 그냥 부등호 방향만 바꾸면 굳이 reverse안해도 되긴 하다. 

추가 정리는 후에 continue..


