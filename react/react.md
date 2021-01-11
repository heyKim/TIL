# react

## React 에서 this란?
react에서 흔히 쓰는 this는 현재 컴포넌트의 reference 이다.

## 리액트에서 바인딩 (React Binding)
React에서 컴포넌트에 이벤트메서드를 연결하는 방법 (=바인딩(binding))

방법1. 주로 사용하는 방법은 컴포넌트의 생성자에서 바인딩 하기다.
```jsx
class App extends React.Component {
    constructor() {
        super();
        this.state = {
              hidden: false,
        };
        this.update = this.update.bind(this);
    }
    update() {
        this.setState({
            hidden: true
        });
    }
    render() {
        return <div
            onClick={ this.update }
        />;
    }
}
```
위에서 `this.update`가 실행되는 순간 `update() { this.setState({ hidden: true });}`가 실행되는데, 이때의 this는 window이다.   
이 함수 자체만 놓고 보면, 자바스크립트 엔진은 this가 뭔지 모르기 때문에 그냥 최상위와 연결시켜 버린다.  
그래서 constructor() 에서 `this.update = this.update.bind(this);` 문장을 집어넣어서 바인딩시키면 render()에서 onClick={this.update}를 할 때 this가 App컴포넌트의 this라는 것을 알게 되는 것이다.

방법2. `autobind-decorator`를 사용해서 바인딩
```jsx
import autobind from 'autobind-decorator'
 
class App extends React.Component {  
    @autobind
    update() {
        ...
    }
    //...생략...
}
```

방법3. ES2015에서 제공하는 기능인 arrow function을 사용하면 this를 자동으로 바인딩
```jsx
import React, { Component, PropTypes } from 'react';
export default class Basic extends Component {
    state = {
           hidden: false,
      };
 
      onClickButton = () => {
        this.setState(() => ({ hidden: true }));
      }
    
    render() {
        return (
              <div>
                <button onClick={this.onClickButton}>숨기기</button>
              </div>
        );
    }
}
```
대신 arrow function을 사용하려면 `@babel/plugin-proposal-class-properties`이 필요하다.

## shallow copy & Deep copy 
shallow copy란? affect the origin one
* Object.assign({}, object) : object는 deep copy가 가능하나, json object는 안됨

Deep copy란? never affect the origin one
* `$.extend`
  * jQuery.extend()는 다른 방식에 비해 느린 편
  * flag에 true를 줘야 deep copy가능
  ```
  //Deep Clone
  let a = { x: {z:1} , y: 2};
  let b = $.extend(true,{},a);
  ```
  
* JSON.parse && JSON.stringify
  ```jsx
  // Deep Copy
  let a = { x:{z:1} , y: 2};
  let b = JSON.parse(JSON.stringify(a));
  ```
  이 방법은 array의 deep copy에도 사용가능
  ```jsx
  //Deep Clone
  let a = [{ x:{z:1} , y: 2}];
  let b = JSON.parse(JSON.stringify(a));
  ```
* 단, object안에 Dates, functions, undefined, Infinity를 사용하지 않는 경우에 유용
  ```jsx
  // Deep Copy
  const a = {
    string: 'string',
    number: 123,
    bool: false,
    nul: null,
    date: new Date(),  // stringified
    undef: undefined,  // lost
    inf: Infinity,  // forced to 'null'
  }
  console.log(a);
  console.log(typeof a.date);  // Date object
  const clone = JSON.parse(JSON.stringify(a));
  console.log(clone);
  console.log(typeof clone.date);  // result of .toISOString()
  ```
* Lodash의 `_.cloneDeep`
  ```jsx
  // Deep Copy
  var obj = { 
    a: 1,
    b: { 
      c: 2
    }
  }

  var newObj = _.cloneDeep(obj);
  obj.b.c = 20;
  console.log(obj); // { a: 1, b: { c: 20 } }
  console.log(newObj); // { a: 1, b: { c: 2 } } 
  ```  
* Underscore.js의 `_.clone`
  ```jsx
  // Deep Copy
  var obj = { 
    a: 1,
    b: 2
  }

  var newObj = _.clone(obj);
  obj.b = 20;
  console.log(obj); // { a: 1, b: 20 }
  console.log(newObj); // { a: 1, b: 2 }  
  ```  
 
* Method 만들어서 사용하기
  ```jsx
  function cloneObject(obj) {
      var clone = {};
      for(var i in obj) {
          if(obj[i] != null &&  typeof(obj[i])=="object")
              clone[i] = cloneObject(obj[i]);
          else
              clone[i] = obj[i];
      }
      return clone;
  }

  var obj = { 
    a: 1,
    b: { 
      c: 2
    }
  }
  var newObj = cloneObject(obj);
  obj.b.c = 20;

  console.log(obj); // { a: 1, b: { c: 20 } }
  console.log(newObj); // { a: 1, b: { c: 2 } } 
  ```
