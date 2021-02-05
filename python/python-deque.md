# deque
* 스택과 큐 기능을 합친 객체
* 선언 방법: `from collections import deque`
* 초기화: `queue = deque(['name','age','DOB'])`
* **큐를 list로 이용하지 않는 이유**: list.append와 list.pop(0)을 이용하면 리스트를 큐처럼 사용할 수 있다. 하지만 pop()의 time complexity는 O(1)인 반면 pop(0)의 time complexity는 O(N)이기 때문에 시간이 오래 걸린다. 따라서 시간 복잡도를 고려해 리스트는 큐로 사용하지 않는다.

## deque basic
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

## deque as Stack
* `append()`: 오른쪽 끝에 항목 추가
* `pop()`: 오른쪽 끝에 항목 삭제

## deque as Queue
* `appendleft()`: 왼쪽에 항목추가
* `pop()`: 오른쪽에 항목 삭제
* `append()`: 오른쪽에 항목 추가
* `popleft()`: 왼쪽에 항목 삭제

## deque as List
* `insert(i, a)`: inserts the value mentioned in arguments(a) at index(i)
* `remove()`: removes the first occurrence of value mentioned in arguments. 

## Extend deque
* `extend(iterable)`: deque의 오른쪽에 iterable로 받은 multiple values 추가
* `extendleft(iterable)`: deque의 왼쪽에 iterable로 받은 multiple values 추가
