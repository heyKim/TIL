# JavaScript Closure
* 간단히 말하면 함수 안에 함수를 만들면 그게 바로 클로저
```js
function outerFunction () {
  const outer = 'I see the outer variable!'
  function innerFunction() {
    console.log(outer)
  }
  return innerFunction
}
outerFunction()() // I see the outer variable!
```
* innerFunction()이 클로저가 된다. 클로저는 외부함수(outerFunction())의 변수에 접근 할 수 있다.
* 클로저는 외부 함수의 변수에 접근할 수 있기 때문에, 일반적으로 두 가지 목적을 위해 사용한다.
  1. 사이드 이펙트(side effects) 제어하기
  2. private 변수 생성하기
  
## Clousure 성능
클로저는 각자의 환경을 가진다. 이 환경을 기억하기 위해서는 당연히 메모리가 소모될 것이다.  
클로저를 생성해놓고 참조를 제거하지 않는 것은 C++에서 동적할당으로 객체를 생성해놓고 delete를 사용하지 않는 것과 비슷하다.  
클로저를 통해 내부 변수를 참조하는 동안에는 내부 변수가 차지하는 메모리를 GC가 회수하지 않는다. 따라서 클로저 사용이 끝나면 참조를 제거하는 것이 좋다.
```js
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('Kim');
var hello2 = hello('Lee');

hello1(); // 'Hello, Kim'
hello2(); // 'Hello, Lee'

// 여기서 메모리를 release 시키기 클로저의 참조를 제거해야 한다.
hello1 = null;
hello2 = null;
```

## Closure관련 참고 사이트
* [Closure관련 번역글](https://medium.com/@khwsc1/%EB%B2%88%EC%97%AD-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-javascript-scope-and-closures-8d402c976d19): 스코프부터 클로저까지 잘 설명 되어있음
* [자바스크립트의 스코프와 클로저](https://meetup.toast.com/posts/86)
