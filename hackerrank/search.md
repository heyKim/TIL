# Search

## Ice Cream Parlor
```python
# O(n^2) due to two nested loop
def icecreamParlor(m, arr):
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):
            if m==arr[i]+arr[j]:
                return(i+1, j+1)
```
```python
#  O(n) 
def icecreamParlor(m, arr):
    temp=dict()
    for i in range(len(arr)):
        if arr[i] not in temp:
            # arr[i]과 쌍이 되는 m-arr[i]를 key로, arr[i]의 위치를 value로 넣음
            temp[m-arr[i]]=i+1
        else:
            return sorted([i+1,temp[arr[i]]])
```
## Missing Numbers
처음에 "Only include a missing number once, even if it is missing multiple times." 이 설명 제대로 안 읽고 풀어서 계속 Test case 1을 통과 못함ㅎㅎ;
```python
def missingNumbers(arr, brr):
    for i in arr:
        brr.remove(i)
    return sorted(list(set(brr)))
```
```python
# 나의 사랑 Counter로 푼 방법
from collections import Counter

def missingNumbers(arr, brr):
    a = Counter(arr)
    b = Counter(brr)
    return sorted((b - a).keys())
```