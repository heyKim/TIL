# React Lifecycle

![리액트 라이프 사이클](https://github.com/heyKim/myMemo/blob/master/images/react/react_lifecycle.jpeg)

## Mount 
* 컴포넌트가 처음 실행될 때 그것을 Mount라고 표현함
  * `contructor()`
  * `static getDerivedStateFromProps()`:new: 
  * `render()`
  * `componentDidMount()`
* :warning: `componentWillMount()` will be deprecated


## Props/State Update
* props, state 가 업데이트될 때의 과정
  * `static getDerivedStateFromProps()` :new: 
  * `shouldComponentUpdate()`:  아직 render하기 전이기 때문에 return false를 하면 render을 취소가능. 주로 여기서 성능 최적화
  * `render()`
  * `getSnapshotBeforeUpdate()` :new: 
  * `componentDidUpdate()` ( 여기서 render완료되어 DOM에 접근 가능)
* :warning: `componentWillReceiveProps()`, `componentWillUpdate()` will be deprecated

## Unmount
* 컴포넌트가 제거되는 것은 Unmount라고 표현
  * `componentWillUnmount`

## Error
* 렌더링 되는 동안 error 발생하면 해당 method들이 호출된다.
  * `static getDerivedStateFromError()`:new: 
  * `componentDidCatch()`
* ex)
```
componentDidCatch(error, info) {
  console.error(error, info);
}
```
* 위와 같이 사용하고, 최상위 컴포넌트에 한 번만 넣어주면 된다.

:pushpin: 리액트 17부터는 `componentWillMount`, `componentWillUpdate`, `componentWillReceiveProps` 라이프 사이클이 deprecated 됨!
