# 문제풀이

## 베스트 셀러
* keyword: `자료 구조`, `정렬`, `해시를 사용한 집합과 맵`
```python
n = int(input())
myDict=dict()
for _ in range(n):
    item=input()
    if item in myDict:
        myDict[item]+=1
    else:
        myDict[item]=1
mav_v=max(myDict.values())
temp=[]
for i in myDict:
    if myDict[i]==mav_v:
        temp.append(i)
sorted(temp)
print(temp[0])
```