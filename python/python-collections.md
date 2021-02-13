# Collections 모듈
* deque 객체

## Counter 객체
### Counter Basic
* dict을 이용한 카운팅
```python
def countLetters(word):
    counter = {}
    for letter in word:
        if letter not in counter:
            counter[letter] = 0
        counter[letter] += 1
    return counter

countLetters('hello world'))
# {'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1}
```
* Counter를 이용한 카운팅
```python
from collections import Counter
Counter('hello world') # Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```

### Counter 유용한 메서드
* count.elements(): 개수만큼 반복되는 요소에 대한 이터레이터를 반환. 단, 요소의 갯수가 0이면 반환 안함
```python
c = Counter(a=4, b=2, c=0, d=-2)
sorted(c.elements())
# ['a', 'a', 'a', 'a', 'b', 'b']
```
* most_common(n): 빈도 높은 순으로 n개의 요소를 리턴
```python
Counter('abracadabra').most_common(3)
# [('a', 5), ('b', 2), ('r', 2)]
```
* update(): `dict.update()`와 비슷하지만 교체하는 대신 더하는 방식
* substract(): `dict.update()`와 비슷하지만 교체하는 대신 빼는 방식
```python
counter = Counter('ababab')
print(counter)  # Counter({'a': 3, 'b': 3})
c = Counter('abc')
print(c)  # Counter({'a': 1, 'b': 1, 'c': 1})

# subtract
counter.subtract(c)
print(counter)  # Counter({'a': 2, 'b': 2, 'c': -1})

# update
counter.update(c)
print(counter)  # Counter({'a': 3, 'b': 3, 'c': 0})
```

### Counter 객체를 이용하는 패턴
```python
from collections import Counter
# s='aabbc'
# c=Counter(s)
c = Counter(a=4, b=2, c=0, d=-2, e=2)
sum(c.values())                 # 모든 개수의 합
c.clear()                       # 모든 개수를 재설정합니다
list(c)                         # 고유한 요소들의 리스트
set(c)                          # 집합으로 변환합니다
dict(c)                         # 일반 딕셔너리로 변환합니다
c.items()                       # (elem, cnt) 쌍의 목록으로 변환합니다
Counter(dict(list_of_pairs))    # (elem, cnt) 쌍의 리스트를 변환합니다
c.most_common()[:-n-1:-1]       # 가장 드문 n 개의 요소
+c                              # 0과 음의 개수를 제거합니다
```

------
## deque 객체
* 스택과 큐 기능을 합친 객체
* 선언 방법: `from collections import deque`
* 초기화: `queue = deque(['name','age','DOB'])`
* **큐를 list로 이용하지 않는 이유**: list.append와 list.pop(0)을 이용하면 리스트를 큐처럼 사용할 수 있다. 하지만 pop()의 time complexity는 O(1)인 반면 pop(0)의 time complexity는 O(N)이기 때문에 시간이 오래 걸린다. 따라서 시간 복잡도를 고려해 리스트는 큐로 사용하지 않는다.

### deque basic
* `clear()`: deque의 모든 원소 삭제
* `copy()`: deque의 shallow copy를 만든다
* `index(ele, beg, end)`: returns the first index of the value mentioned in arguments, starting searching from beg till end index
* `count()`: counts the number of occurrences of value mentioned in arguments
* `reverse()`: reverse order of deque elements
* `rotate()`: deque의 요소들을 k번씩 오른쪽으로 이동시킨다. argument로 -1을 넣으면 왼쪽으로 한칸씩 rotate된다.
    ```python
    from collections import deque
    de=deque([9, 8, 7, 1, 2, 3, 4, 5, 6])
    de.rotate(-4)
    print(de)
    ```

### deque as Stack
* `append()`: 오른쪽 끝에 항목 추가
* `pop()`: 오른쪽 끝에 항목 삭제

### deque as Queue
* `appendleft()`: 왼쪽에 항목추가
* `pop()`: 오른쪽에 항목 삭제
* `append()`: 오른쪽에 항목 추가
* `popleft()`: 왼쪽에 항목 삭제

### deque as List
* `insert(i, a)`: inserts the value mentioned in arguments(a) at index(i)
* `remove()`: removes the first occurrence of value mentioned in arguments. 

### Extend deque
* `extend(iterable)`: deque의 오른쪽에 iterable로 받은 multiple values 추가
* `extendleft(iterable)`: deque의 왼쪽에 iterable로 받은 multiple values 추가
