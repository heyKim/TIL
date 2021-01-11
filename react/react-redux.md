# Redux

## Redux basic
* store.getState() : 현재 스토어에있는 데이터를 반환합니다.
* store.dispatch(ACTION) : 상태값을 수정 할 때 사용되는 메소드입니다. 인수로는 action 이 전달됩니다. 위 컴포넌트에서는 사전에 만들어둔 increase 함수가 action 객체를 반환합니다.
* store.subscribe(LISTENER): dispatch 메소드가 실행되면 리스너 함수가 실행됩니다. 즉, 데이터에 변동이 있을때마다 리렌더링하도록 설정합니다.

* connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
  * connect는 react-redux 의 내장 API 입니다. 이 함수는 React Component 를 Redux Store에  ‘연결’ 해줍니다.  
  * 이 함수의 리턴값은 특정 컴포넌트 클래스의 props 를 store의 데이터에 연결시켜주는 또 다른 함수를 리턴합니다.  
  * 리턴된 함수에 컴포넌트를 인수로 넣어 실행하면, 기존 컴포넌트를 수정하는게 아니라 새로운 컴포넌트를 return 합니다.  
  * 인수:
    * mapStateToProps(state, [ownProps]): (Function) store 의 state 를 컴포넌트의 props 에 매핑 시켜줍니다. ownProps 인수가 명시될 경우, 이를 통해 함수 내부에서 컴포넌트의 props 값에 접근 할 수 있습니다.
    * mapDispatchToProps(dispatch, [ownProps]): (Function or Object)  컴포넌트의 특정 함수형 props 를 실행 했을 때, 개발자가 지정한 action을 dispatch 하도록 설정합니다. ownProps의 용도는 위 인수와 동일합니다.
    * mapDispatchToProps 를 객체형으로 전달하는 방법 및 기타 인수들은 매뉴얼을 참조
    
* bindActionCreators : 
  ```jsx
  const mapDispatchToProps = (dispatch) => ({
      handleSelect: (view) => dispatch(baseActions.setView(view))
  })
  ```
  위의 코드는 다음 코드와 동일하다
  ```jsx
  (dispatch) => ({
      BaseActions: {
          setView: (payload) => dispatch(baseActions.setView(payload)),
          changeSearch: (payload) => dispatch(baseActions.changeSearch(payload))
      }
  })
  ```
-----------------------------
## Ways to Connect Redux Action
```jsx
export default connect(
  mapStateToProps,
  mapDispatchToProps,
)(ChatComponent)
```

```jsx
 // option 1
 const mapDispatchToProps = (dispatch) => ({
   action: () => dispatch(action())
 })
```
Pros: 이해하기 쉽다.
Cons: 복잡해지기 쉽고 wrapping을 깜빡할 가능성이 높다.

```jsx
 // option 2
  const mapDispatchToProps = {
   action: action,
   action1: action1
 }
 or
 // option 3
 const mapDispatchToProps = {
   action,
   action1
 }
 ```
 Pros: 암묵적 wrapping, boilerplate를 작성할 필요가 없다.
 Cons: all properties will be wrapped into a dispatch call, so no way to inject non-action functions into props from here.
 ```
 // option 3
 const mapDispatchToProps = (dispatch) => ({
   action: bindActionCreators(action, dispatch)
 })
```
* 여기 [참고](https://blog.benestudio.co/5-ways-to-connect-redux-actions-3f56af4009c8)
-----------------------------
## Component 밖에서 store접근
`store.js` :
```jsx
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);

export default store;
```
`Provider` :
```jsx
import store from './store'

ReactDOM.render((
<Provider store={store}><Cart showControls={true} /></Provider>)
, document.getElementById('root'));
```
  
이제 component밖에서 store에 접근해보자
```jsx
import * as authActions from 'modules/auth';
import store from '../../store';
store.dispatch(authActions.logout());
```
store를 따로 선언하고 공유하여 사용하면 component 밖에서 store에 접근하여 reducer 호출할 수 있다. 인자로 action을 전달.

(이 방법으로 inceptor안에서 store에 접근할 수 있다.
store를 따로 선언안하고 store를 인자로 전달하여 사용하면 interceptor를 발생시킨 화면에서만 상태가 바뀐다.)
