# ReactDOM
`react-dom`을 사용하려면 ES6에서는 `import ReactDOM from 'react-dom'`를 추가한다.
`react-dom` package는 아래와 같은 DOM과 관련된 method들을 제공한다.
* `render()`
* `hydrate()`
* `unmountComponentAtNode()`
* `findDOMNode()`
* `createPortal()`

## render()
```jsx 
ReactDOM.render(element, container[, callback])
```
* DOM안의 element에 container를 넣고 Component의 reference 또는 stateless component인 경우 null를 return 한다.
* ReactDOM.render() 는 container node를 수정하는 것이 아니라 그냥 insert만 한다. overwriting 하지 않음.

## hydrate()
```jsx
ReactDOM.hydrate(element, container[, callback])
```
* `render()`와 동일하나 클라이언트에서 서버렌더링시 render대신 `ReactDOM.hydrate` 를 통해 렌더링 하도록 api가 추가 되었다.

## unmountComponentAtNode()
```jsx
ReactDOM.unmountComponentAtNode(container)
```
* mounted된 React Component를 DOM에서 제거하고 event handler와 state들을 지워주는 기능이다.
* 이 method를 실행했는데 해당 `container`에 mount된 것이 없다면 이 method는 아무 것도 실행하지 않고 false return
* 꼭 이걸 써야할까?
  * [`unmountComponentAtNode`를 호출안하면 메모리 누수가 있을 거라는 기사](https://reactjs.org/blog/2015/10/01/react-render-and-top-level-api.html)

## findDOMNode()
```jsx
ReactDOM.findDOMNode(component)
```
* Component가 DOM안에 mounted되었다면 그에 맞는 native browser DOM 요소를 반환해주는 함수이다.
* DOM 외부의 값을 읽을 때 유용(form field 값이나 DOM measurement 측정 등)
* 하지만 대부분의 경우에는 DOM node에 ref를 붙여 사용하는 것을 권장

## createPortal()
```jsx
ReactDOM.createPortal(child, container)
```
* portal를 만드는 method
* portal은 DOM component의 hierarchy 구조 밖에 존재하는 DOM node에 자식을 rendering해주는 방법이다.


## 참고
* [공식 사이트](https://reactjs.org/docs/react-dom.html)
