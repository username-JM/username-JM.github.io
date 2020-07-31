---
title: "Fibonacci with Memoization"
excerpt: "피보나치 수열 구현 with memoization"
classes: wide

categories:
    - Computer Science
tags:
    - python
    - fibonacci
    - memoization
    - 피보나치
---

피보나치 수열은 이전 두 개의 요소를 더해 다음 요소를 만들어 나가는 수열이다.
이 때 1, 2번째 요소는 1로 정의된다. 즉
1, 1, 2, 3, 5, 8, 13 ... 과 같이 진행된다. 

일반적으로 피보나치 수열을 구현하게 되면 재귀호출을 사용해서 구현한다.
어떤 요소를 결정하기 위해서는 이전 두 개의 요소를 알아야 하고, 이전 두 개의 요소는 각각의 이전 두 개의 요소를 알아야 하고...
이렇게 재귀적으로 정보를 탐색해야 하기 때문이다.

간단한 구현은 다음과 같다.


```python
def fibonacci(n):
    if n <= 2:
        number = 1 # 1,2번째 요소는 1로 고정
    else:
        number = fibonacci(n-1) + fibonacci(n-2) #이전 2개의 요소를 구하기 위해 재귀호출 실행
    return number
```

위 함수를 사용하게 되면 결과는 잘 나오지만, 입력값이 커질수록 연산 속도는 기하급수적으로 증가하게 된다. 입력값에 대하여 이전 두 값을 구해야 하고, 또 두 값에 대한 두 개의 값을 구하게 된다. 이렇게 되면 알고리즘의 시간복잡도가 O(2^n)이 된다.

이를 해결하기 위해 Memoization을 사용한다. Memoization은 실행 결과들을 메모리에 저장하여 이후 동일한 실행을 시도할 경우 재연산 없이 메모리에서 값을 불러오는 기법이다. 이를 통해 연산 속도를 매우 높여줄 수 있다. 

fibonacci(5)를 실행하면 fibonacci(4)와 fibonacci(3)을 구해야 한다.
fibonacci(4)는 2와 3에 대한 연산을, fibonacci(3)은 1과 2에 대한 연산을 수행해야 한다.
이 때, fibonacci(2) 연산이 중복되는 것을 알 수 있다. 따라서 먼저 연산되는 함수에서 fibonacci(2)의 실행결과를 메모리에 저장하고, 이후 실행되는 함수에서는 단순히 메모리에서 그 값을 불러온다.

이를 Python으로 구현하면


```python
memory = {1: 1, 2: 1} #메모리에 fibonacci(1), (2)의 연산결과를 저장.

def fibonacci_mem(n):
    if n in memory: #만약 메모리에 연산 결과가 있으면
        number = memory[n] #단순히 값만 불러온다
    else:
        number = fibonacci_mem(n-1) + fibonacci_mem(n-2)
        memory[n] = number #메모리에 저장한다. 
    return number
```

실제로 두 함수의 연산 속도를 비교해보자


```python
import time

start = time.time()

fibonacci(40)

print("Without memoization : ", time.time() - start)

start = time.time()

fibonacci_mem(40)

print("With memoization : ", time.time() - start)
```

    Without memoization :  13.095020771026611
    With memoization :  4.2438507080078125e-05


40이라는 작은 수에서도 큰 차이를 보인다. 이 차이는 인자의 값이 커질수록 더 증가할 것이다.
