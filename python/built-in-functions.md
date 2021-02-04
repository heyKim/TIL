# Built-in Functions
|               |               | Build-in function |              |                |
|---------------|---------------|:-----------------:|--------------|----------------|
| `abs()`       | delattr()     | hash()            | memoryview() | `set()`        |
| `all()`       | `dict()`      | `help()`          | `min()`      | setattr()      |
| `any()`       | dir()         | hex()             | next()       | slice()        |
| `ascii()`     | divmod()      | id()              | object()     | sorted()       |
| bin()         | `enumerate()` | `input()`         | oct()        | staticmethod() |
| `bool()`      | eval()        | `int()`           | `open()`     | str()          |
| breakpoint()  | exec()        | isinstance()      | `ord()`      | `sum()`        |
| bytearray()   | filter()      | issubclass()      | pow()        | super()        |
| bytes()       | `float()`     | iter()            | `print()`    | tuple()        |
| callable()    | `format()`    | `len()`           | property()   | type()         |
| chr()         | frozenset()   | `list()`          | `range()`    | vars()         |
| classmethod() | getattr()     | locals()          | repr()       | `zip()`        |
| compile()     | globals()     | `map()`           | reversed()   | __import__()   |
| complex()     | hasattr()     | `max()`           | `round()`    |                |


* `all()`
    * 사용법: all(iterableValue)
    * 전달받은 자료형의 element가 모두 True일 경우 True를 돌려준다.(만약 empty 값을 argument로 넘겨주었다면 True를 돌려준다.)
    * 내부 구현
    ```python
    def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
    ```
* `any()`
    * 사용법: any(iterableValue)
    * 설명: 전달받은 자료형의 element 중 하나라도 True일 경우 True를 돌려준다.(만약 empty 값을 argument로 넘겨주었다면 False를 돌려준다.)
    * 내부구현
    ```python
    def any(iterable):
        for element in iterable:
            if element:
                return True 
        return False
    ```
