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