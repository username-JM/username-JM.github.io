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

## 문제 정리

genres = ["classic", "pop", "classic", "classic", "pop"]   
plays = [500, 600, 150, 800, 2500]

위와 같이 곡의 장르와 해당 곡의 재생 횟수가 주어졌을 때, 각 장르별로 최다 재생된 곡 2개를 출력하는 문제이다. 이 때 곡의 구분은 고유번호, 즉 genres리스트의 순서로 구분된다. 

위 예시에서 첫 데이터는 classic 음악의 곡으로 고유 번호는 0번이다. 해당 곡의 재생 횟수는 500번이다. classic 장르에서 최다 재생된 곡 2개는 3번, 0번이다. 또한 pop 장르의 경우는 4번, 1번이다.

그 후 가장 재생된 횟수가 많은 장르 순서대로, 각 장르별 최다 재생 곡 2개를 출력하는게 문제이다. 즉 위 예시에서는 pop장르가 총 3100번, classic장르가 1450번 출력되었기 때문에 pop -> classic 순서로 출력이 되어야하고, 장르별 최다 재생 곡은 pop: [4,1], classic: [3,0]이므로 출력결과는 [4,1,3,0]이 되어야 한다.

장르별 총 재생 수는 모두 다르지만 한 장르 안의 곡들은 재생 수가 같을 수도 있다. 이 경우, 고유 번호가 낮은 순서대로 출력해준다. 

자세한 내용은 링크를 참고하길 바란다. 

***
## 문제 해설

사실 문제를 읽어보면 어떻게 설계해야할지 바로 생각이 떠오르는 크게 어렵지 않은 문제이다. 먼저 장르별로 곡들을 정리해서 장르별 총 재생 수를 구하고, 각 장르마다 재생 수로 정렬해서 가장 많이 재생된 곡 2개를 찾아서 출력한다... 이런식으로 쉽게 접근할 수 있는 문제이다. 

```python

dict_songs = {}
cnt_dict = {}   

for i in range(0, len(genres)):
    if genres[i] in dict_songs:
        dict_songs[genres[i]].append([plays[i], i])
        cnt_dict[genres[i]] += plays[i] 
    else:
        dict_songs[genres[i]] = [] 
        cnt_dict[genres[i]] = plays[i] 
        dict_songs[genres[i]].append([plays[i], i]) 
        
print(dict_songs)
print(cnt_dict)

```
먼저 각 입력 리스트별로 장르와, 고유번호 및 재생 횟수 정보를 dict_songs와 cnt_dict 리스트에 저장하였다. dict_songs는 장르를 키 값으로 갖고 그 값을 곡의 재생 횟수 n과 고유번호 i를 [n, i] 꼴로 저장하는 딕셔너리다. cnt_dict는 장르를 키 값으로 받고 장르별 총 재생 횟수를 저장하는 딕셔너리다. 

곡들의 장르가 저장된 리스트(arg1)를 하나씩 읽으며 정보를 추출한다. 만약 해당 장르가 기존에 있던 장르라면 정보를 두 딕셔너리에 삽입한다. 만약 새로운 장르인 경우 각 딕셔너리에 키를 생성하고 값을 삽입한다.

위에 예시를 입력으로 넣은 경우 그 출력결과는 아래와 같다. 

{'classic': [[500, 0], [150, 2], [800, 3]], 'pop': [[600, 1], [2500, 4]]} : dict_songs
{'classic': 1450, 'pop': 3100} : cnt_dict

***

```python

dict_songs = sort_per_genres(dict_songs)

def sort_per_genres(target):
for t in target:
    target[t] = quick_sort(target[t]) #각 장르별로 정렬 시도
return target

print(dict_songs)

```
이제 각 장르마다 안에 있는 곡들을 재생 수를 기준으로 내림차순 정렬할 것이다. sort_per_genres는 장르별 재생 횟수를 기준으로 곡들을 내림차순 정렬해주는 함수이다. 이 때 정렬 함수는 quick_sort를 직접 구현해서 사용하였다. 

함수 실행 결과, dict_songs의 출력결과는 

{'classic': [[800, 3], [500, 0], [150, 2]], 'pop': [[2500, 4], [600, 1]]}

각 장르별로 재생 수를 기준으로 내림차순 정렬된 것을 알 수 있다. 

> 내장함수 sort()를 사용하지 않고 정의한 이유

[n,i] 형태의 데이터를 n에 대해 내림차순 정렬하고 싶음 (n = 곡 재생 횟수, i = 고유번호). 하지만 n이 같은 경우에는 i에 대해 오름차순 정렬해야함. 이해가 안되면 문제를 다시 꼼꼼하게 읽어보자. 

따라서 정렬 함수를 따로 정의하여 n이 같은 경우 i로 오름차순 정렬할 수 있도록 구현한다. 사실 c++에서는 연산자 오버로딩을 통해 쉽게 구현할 수 있었는데 python에 대한 지식이 짧다보니 python스러운 구현 방법은 아닌 것 같다. 다른 풀이를 보면 lambda식을 통해 많이 접근하던데 좀 더 공부해야 직접 사용할 수 있을 것 같다. 

***

```python

answer = best_album(dict_songs, cnt_dict)

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



print(answer)

```

dict_songs : {'classic': [[800, 3], [500, 0], [150, 2]], 'pop': [[2500, 4], [600, 1]]}
cnt_songs : {'classic': 1450, 'pop': 3100}

지금까지의 실행 결과, 두 딕셔너리에 저장된 정보는 위와 같다. 이제 베스트 엘범을 추천해줄 시간이다. 추천 함수는 best_album으로 정의하였다. 

두 딕셔너리를 입력으로 받고, cnt_songs에서 max값, 즉 총 재생 횟수가 가장 많은 장르를 선택한다. 그 후 dict_songs에서 해당 장르의 곡들 중 재생 횟수 상위 2개의 곡의 고유번호를 ans리스트에 넣는다. 이 때 장르에 곡이 하나밖에 없는 경우 하나의 곡만 넣는다. 그 후, cnt_dict에서 해당 장르를 제거하여 다음 max값을 찾는다. 이 과정을 cnt_dict가 빈 딕셔너리가 될 때까지, 즉 모든 장르에 대해 반복한다. 

> 실행결과 : [4, 1, 3, 0]


## 문제 풀이(코드) 및 주석

```python

def solution(genres, plays):
    dict_songs = {} # {장르 : [[재생횟수, 고유번호], [재생횟수, 고유번호]]} 형태의 딕셔너리
    cnt_dict = {}   # 장르 별로 총 재생 횟수를 저장하는 딕셔너리
        #dict_songs = {"classic" : [[500, 1], [800, 0]], "pop" : [[2000, 2], [5000, 3]]}
        #cnt_dict = {"classic" : 1300, "pop" : 7000}

    for i in range(0, len(genres)):
        if genres[i] in dict_songs: #이미 있는 장르인가? 
            dict_songs[genres[i]].append([plays[i], i]) # 해당 장르에 [재생수,고유번호]형태로 넣는다
            cnt_dict[genres[i]] += plays[i] # 장르 별 총 재생 수를 구하기 위해 cnt_dict[해당장르]에 재생수 추가
        else:
            dict_songs[genres[i]] = [] #새로운 장르이기 때문에 해당 장르로 키 생성
            cnt_dict[genres[i]] = plays[i] #cnt_dict에도 해당 장르로 키 생성 및 재생 수 삽입
            dict_songs[genres[i]].append([plays[i], i]) #해당 장르에 재생수,고유번호 삽입
    dict_songs = sort_per_genres(dict_songs) #장르 별로 있는 곡들을 재생 수로 내림차순 정렬, 이 때 재생 수가 같으면 고유번호로 오름차순 정렬
    answer = best_album(dict_songs, cnt_dict)
    return answer


def sort_per_genres(target):
    for t in target:
        target[t] = quick_sort(target[t]) #각 장르별로 정렬 시도
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
        if arr[i][0] > pivot[0]: # 기준보다 큰 애들은 왼쪽으로 가라~
            lesser_arr.append(arr[i])
        elif arr[i][0] < pivot[0]: # 기준보다 작은 애들은 오른쪽으로 가라~
            greater_arr.append(arr[i])
        else: # 같은경우
            if arr[i][1] < pivot[1]: # 고유 번호를 비교해서 오름차순 정렬한다.
                lesser_arr.append(arr[i])
            elif arr[i][1] > pivot[1]:
                greater_arr.append(arr[i])
            else:
                center.append(pivot) # 고유번호도 같다? 자기자신
                continue
    return quick_sort(lesser_arr) + center + quick_sort(greater_arr) # 병합 과정


```
