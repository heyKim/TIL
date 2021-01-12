# Control Flow Tools

## break/pass/continue/return
* `break`: 해당 반복문을 탈출한다. 그리고 코드를 계속해서 실행해나간다.*(in loop)*
* `pass`: 아무것도 하지 않는다. 그리고 코드를 계속해서 실행해나간다.*(in Anywhere)*
```python
def test_break():
    for i in range(5):
        if i == 2:
            pass
        print("This is break test")     
    print("This code is below the loop.")
```
```
#결과
This is break test
This is break test
This is break test
This is break test
This is break test
This code is below the loop.
```
* `continue`: 반복문의 처음으로 돌아간다.*(in loop)*
```python
def test_break():
    for i in range(5):
        if i == 2:
            continue
        print("This is break test")            
    print("This code is below the loop.")
```
```
# 결과
This is break test
This is break test
This is break test
This is break test
This code is below the loop.
```
* `return`: 함수 즉시 탈출(중단), 함수 내부값 외부로 반환.*(in function)*
```python
def test_break():
    for i in range(5):
        if i == 2:
            return
        print("This is break test")           
    print("This code is below loops.")
```
```
#결과
This is break test
This is break test
```