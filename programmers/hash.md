# Hash
## 위장
```python
# 딱 1개가 시간초과남ㅠ엉엉ㅠ
def solution(clothes):
    answer = 0
    dict={}
    for item in clothes:
        # 해당 key가 dict에 있는 경우
        if item[1] in dict:
            dict[item[1]].append(item[0])
        # 새로운 key인 경우
        else:
            dict[item[1]]=[item[0]]

    key_list=dict.keys()
    for i in range(1,len(dict)+1):
        for items in itertools.combinations(key_list,i):
            sub_sum=1
            for q in items:
                sub_sum*=len(dict[q])
            answer+=sub_sum
    return answer
```
```python
def solution(clothes):
    answer = 0
    dict={}
    for item in clothes:
        # 해당 key가 dict에 있는 경우
        if item[1] in dict:
            dict[item[1]].append(item[0])
        # 새로운 key인 경우
        else:
            dict[item[1]]=[item[0]]
    # 모든 조합의 수: (옷종류A+1)*(옷종류B+1) (여기서 1은 안입는 경우)
    for key in dict:
            answer *= (len(dict[key])+1)
        return answer-1 # 둘다 안입는 경우 제외
```
```python
def solution(clothes):
    from collections import Counter
    from functools import reduce
    cnt = Counter([kind for name, kind in clothes])
    answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
    return answer
```
```python
import collections
from functools import reduce

def solution(c):
    return reduce(lambda x,y:x*y,[a+1 for a in collections.Counter([x[1] for x in c]).values()])-1
```
```python

2
3
4
5
6
7
8
9
10
11
12
13
14
15
def solution(clothes):
    clothes_type = {}

    for c, t in clothes:
        if t not in clothes_type:
            clothes_type[t] = 2
        else:
            clothes_type[t] += 1

    cnt = 1
    for num in clothes_type.values():
        cnt *= num

    return cnt - 1
```