# Optimizing React Component
* Use many small components
* Render lists in dedicated components
* Don't use array indexes as keys
* Dereference values late
* Bind functions early

## 컴포넌트를 작게 만들어라
`observer` 컴포넌트는 사용하는 모든 value를 tracking하고 변경이 있으면 re-render시킨다.
그래서 컴포넌트가 작을수록 변화가 작아진다. 컴포넌트를 작게만들고 각각 독립적으로 redering되게 하는 것이 성능상 좋다.

## List를 렌터링 시킬 때는 별도의 컴포넌트를 사용하자
사이즈가 큰 collection을 rendering하는 것은 React에서 매우 성능을 떨어뜨린다.  
reconciler는 collection이 렌더링 될때 각각 collection의 변경될 때마다 그 해당 컴포넌트를 evaluate해야한다.  
그래서 해당 collection을 map으로 돌리는 component를 만드는 것을 권장한다.  
아래의 소스 보면 이해 쉬움.
Bad:
```jsx
@observer class MyComponent extends Component {
    render() {
        const {todos, user} = this.props;
        return (<div>
            {user.name}
            <ul>
                {todos.map(todo => <TodoView todo={todo} key={todo.id} />)}
            </ul>
        </div>)
    }
}
```
위에서는 `user.name`의 값이 바뀔 때, 모든 `TodoViews` 컴포넌트를 불필요하게 체크해야한다.
`user.name`을 쓰지도 않는데 체크를 해야하니 비효율이 발생하고 굉장히 cost가 많이든다.

Good: 
```jsx
@observer class MyComponent extends Component {
    render() {
        const {todos, user} = this.props;
        return (<div>
            {user.name}
            <TodosView todos={todos} />
        </div>)
    }
}

@observer class TodosView extends Component {
    render() {
        const {todos} = this.props;
        return <ul>
            {todos.map(todo => <TodoView todo={todo} key={todo.id} />)}
        </ul>)
    }
}
```
이렇게 소스를 짜면 `MyComponent`와 `TodosView`는 다른 컴포넌트이기 때문에 `MyComponent`의 `user.name`값이 변경되도 `TodosView`에 영향을 주지않는다.

## array의 indexes를 key로 사용하지 마라

## Dereference values는 느리다
Dereference value, 역참조되는 값은 속도가 느리다.  
왜냐하면 MobX가 derefernce observable value가 있는 컴포넌트를 자동으로 re-render한다.
Fast: 
```
<DisplayName person={person} />
```
Slower:
```
<DisplayName name={person.name} />
```
name property의 값이 바뀌면 그 영향으로 `DisplayName` 컴포넌트가 re-render되고, 이 컴포넌트를 가지는(owner) 컴포넌트도 re-render된다.
그래서 컴포넌트를 작게 만들어야한다.
example: 
`const PersonNameDisplayer = observer((props) => <DisplayName name={props.person.name} />)`
`const CarNameDisplayer = observer((props) => <DisplayName name={props.car.model} />)`
`const ManufacturerNameDisplayer = observer((props) => <DisplayName name={props.car.manufacturer.name} />)`
근데 컴포넌트를 작게 만드려다 보니 뭔가 비슷한 코드를 계속 만들어야 한다. 귀찮다...
그래서 아래와 같이 코드를 짜는 것을 권장한다.
```jsx
render() {
  const { person, car } = this.props;
  return (
    <>
      <GenericNameDisplayer getNameTracked={() => person.name} />
      <GenericNameDisplayer getNameTracked={car.getModelTracked} />
      <GenericNameDisplayer getNameTracked={this.getManufacturerNameTracked} />
    </>
  );
}

getManufacturerNameTracked = () => this.props.car.manufacturer.name;

...
class Car {
  @observable model
  getModelTracked = () => this.model
}
```
여기서 문제는 function을 어디에 둘 것인가 이다.
* render method안에 function을 만든다.(비추)
* 컴포넌트 안에 function을 둔다.(`getManufacturerNameTracked` 처럼)
* 데이터가 포함된 object에 function을 둔다.(`getModelTracked`처럼)

## function을 빨리 bind시킨다?
Bad:
```jsx
render() {
    return <MyWidget onClick={() => { alert('hi') }} />
}
```
Good: 
```jsx
render() {
    return <MyWidget onClick={this.handleClick} />
}

handleClick = () => {
    alert('hi')
}
```
bad example에서는 `MyWidget`에서 사용되는 `PureRenderMixin`의 `shouldComponent`를 항상 실행시킨다.
parent가 re-rendered될 때 마다 new function을 넘기기 때문이다.





