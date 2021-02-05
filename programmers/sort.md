# Sort

## K번째 수
```python
def solution(array, commands):
    answer = []
    for n in commands:
        answer.append(sorted(array[n[0]-1:n[1]])[n[2]-1])
    return answer
```