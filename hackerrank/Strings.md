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
## Separate the Numbers
```python
def separateNumbers(s):
    targetStr=str(s)
    targetLen=len(targetStr)
    idx=0
    if targetLen == 1:
        print('NO')
    else:
        for i in range(1,int(targetLen/2)+1):
            present=targetStr[idx:idx+i]
            result=chkNextNum(present, targetStr)
            if result:
                print('YES {}'.format(present))
                break
            elif i==int(targetLen/2) and not result:
                print('NO')
            
            
def chkNextNum(first,targetStr):
    idx=len(first)
    present=first
    while idx < len(targetStr):
        next=str(int(present)+1)
        nextLen=len(next)
        if next==targetStr[idx:idx+nextLen]:
            idx+=nextLen
            present=next
            pass
        else:
            return False
    return True
```

## Funny String
adjacent difference 문제
```python
def funnyString(s):
    r=list(s)
    r.reverse()

    prev=0
    sDifference=[]
    for idx,n in enumerate(list(s)):
        if idx!=0:
            sDifference.append(abs(ord(n)-prev))
        prev=ord(n)

    prev=0
    rDifference=[]
    for idx,n in enumerate(r):
        if idx!=0:
            rDifference.append(abs((ord(n)-prev)))
        prev=ord(n)


    return "Funny" if sDifference == rDifference else "Not Funny"
```
```python
def funnyString(s):
    st=list(s)
    diff=[abs(ord(a)-ord(b)) for a,b in zip(st,st[1:])]
    return "Funny" if (diff==diff[::-1]) else "Not Funny"
```
## Gemstones
intersection(교집합) 문제
```python
def gemstones(arr):
    return len(set.intersection(*map(set,arr)))
```

## Alternating Characters
```python
def alternatingCharacters(s):
    sA=re.sub(r'A{2,}','A',s)
    sB=re.sub(r'B{2,}','B',sA)
    return(len(s)-len(sB))
```

## Beautiful Binary String
```python
def beautifulBinaryString(b):
    return len(re.findall(r'010',b))
```
```python
# simpler
def beautifulBinaryString(b):
    return b.count('010')
```

## The Love-Letter Mystery
* palindrome(회문) 문제
* palindrome이란? 내 이름은 이효리 거꾸로 해도 이효리 같이 똑바로 읽어도 거꾸로 읽어도 같은 문장,단어,숫자
```python
def theLoveLetterMystery(s):
    halfLen=int(len(s)/2)
    count=0
    for i,j in zip(s[halfLen:],s[::-1][halfLen:]):
        count+=abs(ord(i)-ord(j))
    return count
```
```python
def theLoveLetterMystery(s):
    s = list(map(ord, s))
    return sum(map(abs, (s[x]-s[-x-1] for x in range(len(s)//2))))
```

## (참고)check if a string is palindrome or not
```python
# Method#1
def isPalindrome(s):
    return s == s[::-1]
```
```python
def isPalindrome(str):
 
    # Run loop from 0 to len/2 
    for i in range(0, int(len(str)/2)): 
        if str[i] != str[len(str)-i-1]:
            return False
    return True
```
```python
def isPalindrome(s):
    rev = ''.join(reversed(s))
    if (s == rev):
        return True
    return False
```
## Palindrome Index
```python
# Time limit exceeded
def palindromeIndex(s):
    for idx,elem in enumerate(s):
        temp=list(s)
        temp.pop(idx)
        if temp==temp[::-1]:
            return idx
    return -1
```
```python
# 일부 Time limit exceeded
def palindromeIndex(s):
    strObj=''
    for i,elem in enumerate(s):
        strObj = s[0 : i : ] + s[i + 1 : :]
        if strObj==strObj[::-1]:
            return i
    return -1
```
```python
# 제일 빠름
def palindromeIndex(s):
    for i,j in enumerate(range(0,len(s)//2),1):
        if s[-i] == s[j]:
            continue
        if s[j:-i]==s[j:-i][::-1]:
            return len(s)-i
        elif s[j+1:-i+1]==s[j+1:-i+1][::-1]:
            return j
        return -1
    return -1
```
```python
def isPalindrome(str): 
    for i in range(0, int(len(str)/2)): 
        if str[i] != str[len(str)-i-1]:
            return False
    return True

def palindromeIndex(s):
    for i in range(int((len(s)+1)/2)):
        if s[i] != s[len(s)-i-1]:
            if isPalindrome(s[:i]+s[i+1:]):
                return i
            else:
                return len(s)-i-1
    return -1
```

## Making Anagrams
```python
def makingAnagrams(s1, s2):
    sum=0
    for elem in string.ascii_lowercase:
        sum+=abs(s1.count(elem)-s2.count(elem))
    return sum
```
## Game of Thrones - I
```python
def gameOfThrones(s):
    count_odd=0
    for i in string.ascii_lowercase:
        if s.count(i)%2 == 1:
            count_odd+=1
    return 'NO' if count_odd>1 else 'YES'
```

## Two String
* 두 문자열에서 문자 비교
```python
def twoStrings(s1, s2):
    for i in s1:
        if i in s2:
            return 'YES'
    return 'NO'
```
```python
def twoStrings(s1, s2):
    return 'YES' if set(s1) & set(s2) else 'NO'
```