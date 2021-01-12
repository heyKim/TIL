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
    result='YES'
    originStr='hackerrank'
    tempStr=s[index:]
    for i in originStr:
        print(index, tempStr, len(tempStr))
        if tempStr.find(i) == -1:
            result='NO'
            break
        else:
            index=tempStr.find(i)+1
            tempStr=tempStr[index:]
    return result
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

