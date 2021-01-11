# mobx 기본

## 특징
* Decorator를 적극 활용한다.
* Typescript를 base로 만들어졌기 때문에 Typescript에 대한 지원이 좋다.
* Redux와 매우 큰 차이로, 단일 Store를 강제하지 않는다.

## 주의 사항
* @observabel은 Decorator인데 데코레이터를 사용해주기 위해서 experimentalDecorators가 디폴트 false이기 때문에 true를 주어야한다.  
js라면 jsconfig.json파일을 만들고 아래 내용을 추가하고, typescript면 tsconfig.json파일 만들고 아래 내용을 추가
```json
{
    "compilerOptions": {
        "experimentalDecorators": true
    }
}
```

## Core Concept

### Observable
* `observable`은 관찰하고자 하는 상태값이다. 즉 변화되는지 안되는지 체크할 값
* `observable`를 사용하는 방법
    1. 일반 function을 사용
    ```js
    const map = observable.map({ key: "value"});
    map.set("key", "new value");

    const list = observable([1, 2, 4]);
    list[2] = 3;

    const person = observable({
        firstName: "Clive Staples",
        lastName: "Lewis"
    });
    person.firstName = "C.S.";

    const temperature = observable.box(20);
    temperature.set(25)
    ```
    2. Decorator로 사용
    바벨 또는 typescript같은 transpiler를 사용할 수 있는 환경이라면 decorator방식으로 사용하는 것이 좋다.
    ```
    @observable price = 0;
    @observable amount = 1;
    ```
### Observer
* `observer`는 앞으로 거의 모든 컴포넌트에 사용하게 될 것이다. 
* `observer`는 mobx.autorun라는 함수를 가지고 컴포넌트의 render함수를 감싸게 되면서, `observable`한 데이터가 변경될 때마다 observer가 선언된 컴포넌트는 재 렌더링하게 해준다.
* Observer 사용하는 방법
    1. 일반 function을 사용
    ```
    import { observer } from 'mobx-react';
    const App = observer(()=> {
    });
    ```
    2. Decorator로 사용
    ```
    import React from 'react';
    import { observer } from 'mobx-react';
    @observer
    class AppContainer extends React.Component {
        //생략
    }
    export default AppContainer;
    ```
* 주의사항: observer를 데코레이터 방식으로 사용할 경우 Lifecycle이 mobx의 Lifecycle로 바뀐다.
```
1. componentWillReceiveProps(nextProps)
2. shouldComponentUpdate(nextProps, nextState)
3. componentWillUpdate(nextProps, nextState)
4. render()
5. componentDidUpdate(prevProps, prevState)
```
에서
```
1. componentWillReact
2. componentWillUpdate
3. render
4. componentDidUpdate
```
로 Lifecycle의 순서가 바뀐다.

### computed
* computed value는 state나 다른 computed value에서 파생된 값이다.(엑셀의 함수 같은 것)
* `@computed`는 Observable값에 대해서 적절한 계산이 필요할 때 사용하는 API이다.
* observable은 값이 변경될 때마다 무조건 렌더링한다. 실제 사용할 값을 `computed` 해놓고 사용하면 `observable` 값이 변경되더라도, `computed`에 명시된 값에 부합하지 않는다면 다시 렌더링 하지 않는다.
* `@computed`를 쓰든 쓰지않든 별 차이없이 값은 양 쪽 모두 변경되고 있다. 그럼 `@computed`는 왜 존재하는 걸까?
    * 동작에는 큰 차이가 없지만 성능상 차이는 존재함. 
    * `@computed`는 계산 결과 값을 캐싱하기 때문에 `@computed` 내부의 동작이 무거울 수록 성능상 이점이  더 커진다.
    
### autorun
* `autorun`은 Observable 값이 파생된다는 점은 `computed`와 같지만, 용도는 전혀 다르다.
* computed vs autorun
    * computed는 다른 observer가 사용할 수 있는 값을 생산하고 싶을 떄 사용한다.
    * autorun은 새로운 값을 생성하고 싶지 않는 경우 사용한다. logging이나 network 요청하거나..
  
### @action
* action은 Observable 값을 변경하는(Setter)에 사용하는 API다. 
* 기본적으로 MobX에서는 Observable 값을 변경하는 메소드에는 action을 달아줄 것을 권장하지만 쓰지 않아도 정상적으로 동작한다.
* 그렇다면 왜 만들어둠?;
    * computed와 마찬가지로, action을 사용하는 이유도 성능이다.  
    (만약 2개의 observable값을 업데이트 시킬 때) action 없이 Observable 값을 업데이트 했을 떄는, width와 height 각각의 값이 업데이트 되는 시점에서 Reaction 함수가 호출되지만, action을 사용했을 때는 그 두 값이 모두 업데이트 된 뒤에야, Reaction 함수가 호출된다는 차이에서 기인한다. 이렇게 Observable 값을 업데이트하는 동작을 묶어 일괄 실행하고 모든 동작이 끝났을 때 통지하는 것을 MobX에서는 트랜잭션(Transaction)이라고 부른다. 개념은 조금 다르지만 데이터베이스의 그 용어와 같다.
    
```js
import React from 'react';
import 'style/common.css';experimentalDecorators
import {observable, action} from 'mobx'
import { observer } from 'mobx-react'

@observer
class App extends React.Component {
    @observable email = ''
    @observable passwords = ''
 
    @action.bound
    onChange(event) {
        const { name, value} = event.target;
        this[name] = value;
    }

    onSubmit() {
        const {email, password} = this;
        console.log(`결과확인: ${email}/ ${password}`)
    }
  render(){
        const { email, password } = this;
        return (
          <div className="contents">
              <header>Login</header>
              <input name='email' placeholder='Email' onChange={this.onChange} value={email} fluid></input>
              <input 
                name="password"
                type="password"
                placeholder='Password'
                onChange={this.onChange}
                value={password}
                fluid
              ></input>
              <button onClick={this.onSubmit}>Submit</button>
          </div>
        );
    }
}

export default App;
```

모든 컴포넌트에서 observer를 사용하는것이 어플리케이션의 속도를 늦춘다고 걱정할 수도 있지만, mobx에서 이를 최적화하기 때문에 걱정할 필요는 없다. 가급적 모든 컴포넌트에 observer를 사용하는 것이 좋다.


mobx를 사용하게 되면 더이상 setState를 사용하지 않고 observable한 데이터를 직접 조작할 수 있게 된다.  
setState는 비동기적(asynchronous)으로 처리되기 때문에 가끔 react로만 프로젝트를 진행하다 보면 데이터가 변경되었는데도 렌더링이 즉각적으로 반영되지 않아 곤란할 때가 있었을 것이다.  
변수에 observable선언만 해주면 이를 해결해줄 수 있고, 좀 더 사용이 간편하다.
