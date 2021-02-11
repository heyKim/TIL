# Greedy(탐욕법)

## 체육복
```python
def solution(n, lost, reserve):
    cantAttend=0
    # 1. 도난 당한 학생은 자신의 여벌 옷이 있는지 확인
    intersection=set(lost) & set(reserve)
    lost=list(set(lost)-intersection)
    reserve=list(set(reserve)-intersection)
    
    for i in lost:
        # 2. 자신의 앞번호(first)에게 여벌 옷이 있는지 확인
        if reserve.count(i-1)>0:
            reserve.remove(i-1)
        # 3. 자신의 뒷번호(last)에게 여벌 옷이 있는지 확인
        elif reserve.count(i+1)>0:
            reserve.remove(i+1)
        # 세 경우 다 불가능하면 수업을 들을 수 없는 학생(cantAttend+=1)
        else:
            cantAttend+=1
    answer=n-cantAttend
    return answer
```