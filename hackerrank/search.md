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
            temp[m-arr[i]]=i+1
        else:
            return sorted([i+1,temp[arr[i]]])
```