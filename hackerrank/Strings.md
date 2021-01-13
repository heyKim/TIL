# Strings
## Mars Exploration
```python
# Complete the marsExploration function below.
def marsExploration(s):
    count=0
    slength = len(s)
    originStr = 'SOS'*(int(slength/3))
    
    u=zip(originStr,s)
    for i,j in list(u):
        if i != j:
            count+=1
    return count
```

## HackerRank in a String!
```python
def hackerrankInString(s):
    index=0
    tempStr=s[index:]
    for i in list('hackerrank'):
        if tempStr.find(i) == -1:
            return 'NO'
        else:
            index=tempStr.find(i)+1
            tempStr=tempStr[index:]
    return 'YES'
```
```python
def hackerrankInString(s):
    for idx, char in enumerate(list('hackerrank')):
        try:
            index = s.index(char)
            s = s[index+1:]
        except ValueError:
            return "NO"
    return "YES"
```
```python
# more pritter
def hackerrankInString(s):
    return 'YES' if re.search(r'.*h.*a.*c.*k.*e.*r.*r.*a.*n.*k.*',s.strip()) else 'NO'
```
```python
# more pritter
def hackerrankInString(s):
    return (re.search('.*'.join("hackerrank"), s) and "YES") or "NO"
```

## Pangrams
```python
def pangrams(s):
    lowerStr=s.lower()
    for i in list('abcdefghijklmnopqrstuvwxyz'):
        if lowerStr.find(i) == -1:
            return 'not pangram'
    return 'pangram'

```
```python
def pangrams(s):
    return ('pangram' if set(string.ascii_lowercase).issubset(set(s.lower())) else 'not pangram')
```

## Weighted Uniform Strings
```python
# timeout남
def weightedUniformStrings(s, queries):
    prev=0
    prevWeight=0
    weightList=[]
    result=[] # return값

    for idx, now in enumerate(s):
        if s[idx] == prev:
            prevWeight+=(ord(s[idx])-ord('a')+1) # ord('a')안쓰고 97써도 되긴함
        else:
            prevWeight=(ord(s[idx])-ord('a')+1)
        prev=s[idx]
        weightList.append(prevWeight)

    for i in queries:
        if i in set(weightList):
            result.append('Yes')
        else:
            result.append('No')
    return result
```
```python
# success
# list에 담고 set으로 중복제거하는 것이 아닌 처음부터 set으로 정의하고 중복된 데이터를 넣지 않음
# 효율적인 알고리즘을 구현하려면 항상 dictionary(HashMap) 또는 set(HashSet) 자료구조를 고려해야 한다. 탐색 비용이 O(1)이기 때문이다. list(ArrayList)에서 in 또는 count 연산을 반복 수행하는 행위는 최악의 결과를 가져온다.
# 결론: List와 비교했을 때 단지 중복자료를 보관할 수 없다는 점 밖에 보이지 않지만 검색 속도에 있어서 엄청난 차이를 보이기 때문에 많은 자료를 관리할 때는 Set을 쓰자
def weightedUniformStrings(s, queries):
    prev=0
    prevWeight=0
    weightList=set()
    result=[] # return값

    for idx, now in enumerate(s):
        if s[idx] == prev:
            prevWeight+=(ord(s[idx])-ord('a')+1) # ord('a')안쓰고 97써도 되긴함
        else:
            prevWeight=(ord(s[idx])-ord('a')+1)
        prev=s[idx]
        weightList.add(prevWeight)

    for i in queries:
        if i in weightList:
            result.append('Yes')
        else:
            result.append('No')
    return result
```
```python
# generator 쓰면 더 좋겠쥬?
def weightedUniformStrings(s, queries):
    prev=0
    prevWeight=0
    weightList=set()

    for idx, now in enumerate(s):
        if s[idx] == prev:
            prevWeight+=(ord(s[idx])-ord('a')+1) # ord('a')안쓰고 97써도 되긴함
        else:
            prevWeight=(ord(s[idx])-ord('a')+1)
        prev=s[idx]
        weightList.add(prevWeight)

    for i in queries:
        if i in weightList:
            yield 'Yes'
        else:
            yield 'No'
```