# heap

## 더 맵게
```python
# heap 안쓰고(근데 시간초과)
def make_scoville(scoville):
    sorted_scov=sorted(scoville, reverse=True)
    scov_len=len(scoville)
    # 리스트에서 가장 지수가 낮은 2개 선택
    last1=sorted_scov[scov_len-1]
    last2=sorted_scov[scov_len-2]
    sorted_scov.pop(scov_len-1)
    sorted_scov.pop(scov_len-2)
    sorted_scov.append(last1+last2*2)
    return sorted_scov
    
def solution(scoville, K):
    answer = 0
    while True:
        # 리스트의 지수들이 K 이상인지 체크
        isOverScov=any(list(map(lambda x:x<K, scoville)))
        if isOverScov:
            if len(scoville)==1:
                answer=-1
                break
            scoville=make_scoville(scoville)
            answer+=1
        else:
            break
    return answer
```    
```python
# heapq 쓴 버전
import heapq
def solution(scoville, K):
    heapq.heapify(scoville)
    answer = 0
    while True:
        # 제일 작은 원소가 K 이하인지
        if scoville[0]<K:
            if len(scoville)==1:
                answer=-1
                break
            add_scov=heapq.heappop(scoville)
            add_scov+=heapq.heappop(scoville)*2
            heapq.heappush(scoville, add_scov)
            answer+=1
        else:
            break
    return answer
```
