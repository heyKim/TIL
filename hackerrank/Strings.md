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
