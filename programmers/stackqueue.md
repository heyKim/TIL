# Stack&Queue
## 주식문제
```python
# 효율성 zero
def solution(prices):
    answer = []
    for i, n in enumerate(prices):
        count=0
        if i+1==len(prices):
            answer.append(0)
            break
        else:
            for j in prices[i+1:]:
                if n>j:
                    count+=1
                    break
                elif n<=j:
                    count+=1
                else:
                    break
            answer.append(count)       
    return answer
```

```python
def solution(prices):
    answer = []

    for i in range(len(prices)-1):
        c = 0
        for j in range(i,len(prices)-1):
            if prices[i] > prices[j]:
                break
            else:
                c += 1
        answer.append(c)
    answer.append(0)
    return answer
```
```python
def solution(prices):
    answer = [0] * len(prices)
    for i in range(len(prices)):
        for j in range(i+1, len(prices)):
            if prices[i] <= prices[j]:
                answer[i] += 1
            else:
                answer[i] += 1
                break
    return answer
```

## 기능개발
```python
import math
def solution(progresses, speeds):
    result=[]
    workdays=[]
    for i,j in zip(progresses,speeds):
        workdays.append(math.ceil((100-i)/j)) # 작업일수

    prev=0
    result=[]
    for i in workdays:
        count=1
        if i>prev:
            prev=i
            result.append(1)
        else:
            result[len(result)-1]+=1
    return result
```
```python
# queue 활용한 남의 코드..ㅎ
from math import ceil

def solution(progresses, speeds):
    daysLeft = list(map(lambda x: (ceil((100 - progresses[x]) / speeds[x])), range(len(progresses))))
    count = 1
    retList = []

    for i in range(len(daysLeft)):
        try:
            if daysLeft[i] < daysLeft[i + 1]:
                retList.append(count)
                count = 1
            else:
                daysLeft[i + 1] = daysLeft[i]
                count += 1
        except IndexError:
            retList.append(count)

    return retList
```

## 다리를 지나는 트럭
```python
def solution(bridge_length, weight, truck_weights):
    on_bridge=[0]*bridge_length
    second = 0
    while len(on_bridge)!=0:
        on_bridge.pop(0)
        if len(truck_weights)!=0:
            if sum(on_bridge)+truck_weights[0] <= weight:
                on_bridge.append(truck_weights[0])
                truck_weights.pop(0)
            else:
                on_bridge.append(0)
        second+=1
    return second
```
:star: `while` 조건으로 리스트를 줄수도 있음. 0이나 null 인 경우 False
```python
# 내가 짠 소스랑 비슷한데 더 간결함
def solution(bridge_length, weight, truck_weights):
    q=[0]*bridge_length
    sec=0
    while q:
        sec+=1
        q.pop(0)
        if truck_weights:
            if sum(q)+truck_weights[0]<=weight:
                q.append(truck_weights.pop(0))
            else:
                q.append(0)
    return sec
```