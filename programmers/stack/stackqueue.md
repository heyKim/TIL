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