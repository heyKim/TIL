# Babel
Babel은 ES6+ 코드를 ES5 이하의 버전으로 트랜스파일링한다.
모든 브라우저가 최신 자바스크립트 문법을 지원하지 않는다.  
그래서 최신 자바스크립트 문법이 오래된 브라우저에서도 동작할 수 있게 해당 브라우저에 맞게 자바스크립트 문법을 변환해주는 라이브러리다.

## Proposal의 다양한 stage
바벨의 공식문서에 의하면
TC39는 다음과 같이 단계를 구분합니다.
stage-0 - Strawman: 구체적이지 않은 구상안, 바벨 plugin 이 될 수 있음.
stage-1 - Proposal: 받아들일 가치가 있는 제안
stage-2 - Draft: 초기 표준
stage-3 - Candidate: 표준의 정의를 마쳤으며 초기 형태의 브라우저 구현이 존재(stage-3 이전 단계는 주의를 요함)
stage-4 - Finished: 다음 표준에 포함될 예정
이런 preset 이나 plugin 을 추가함으로써 미래에 추가될 기능이나 실험적인 기능을 사용할 수 있습니다.

## 바벨 plugin 과 preset 을 사용하는 방법
1. package.json을 이용하는 방식
```
// package.json
{
  "babel": {
    // nest config under "babel"
    "presets": ["es2015", "react", "stage-3"],
    "plugins": ["transform-class-properties"]
  }
}
```
2. .babelrc 파일을 이용하는 방식
```
// .babelrc
{
  "presets": ["es2015", "react", "stage-3"],
  "plugins": ["transform-class-properties"]
}
```
주로 이 방법을 추천함.

그러나 바벨설정파일만 설정한다고 바벨을 사용할수는 없음. 웹팩 설정을 해주어야함. 웹팩설정참고

```
{
    "presets": [
      "@babel/preset-env",
      "@babel/preset-react",
      "mobx"
    ],
    "plugins": [
        ["@babel/plugin-proposal-class-properties", {"loose": true}],
        [
          "module-resolver",{
            "root": ["./src"],
            "alias": {
              "public": "./public"
            }
          },
        ]
    ]
  }

```
* @babel/preset-env: 자동으로 브라우저의 polyfill을 맞춘다는 의미. 즉, 함께 사용되어야 하는 Babel 플러그인을 모아 둔 것으로 Babel preset이라고 부른다. 
  * babel-presets-es2015는 babel-presets-env로 교체되었다.
  * (참고) preset이란? 특정 기능을 지원하는 여러 plugin 의 모음
  * 혹시나 브라우저 타겟군을 맞추고 싶다면 아래와 같이 쓰거나 package.json 혹은 .browserslistrc 에 설정하거나 할 수 있다.
  ```
  presets: [
    ["@babel/preset-env", {
      "targets": {
        "browsers": [ "last 2 versions" ]
      }
    }]
  ]
  ```
  * @babel/preset-react: 리액트 환경(JSX)을 위한 라이브러리
  * @babel/proposal-class-properties: class property 관련된 문제를 해결하기 위해 사용한다.
  @babel/proposal-object-rest-spread 는 …변수 spread 과 관련된 문제를 해결하기 위해 사용한다.
