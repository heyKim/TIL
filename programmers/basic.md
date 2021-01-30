# Basic
## 가운데 글자 가져오기
```python
def solution(s):
    answer = ''
    if len(s) % 2 == 0:
        k = int(len(s) / 2 - 1)
        answer = s[k:k+2]
    else:
        k = int(len(s) / 2)
        answer = s[k:k+1]

    return answer
```
```python
def string_middle(str):
    return str[(len(str)-1)//2:len(str)//2+1]
```
