
### 어떤 Router를 사용할 것인가?
* `react-router`: 코어 모듈(현재 v4)react-router-dom, react-router-native 등이 추가되었습니다. 
* `react-router-dom`: react-router 모듈에 dom 이 바인딩 되어있음.
* `react-router-native`: react-native 개발할 때 사용하는 모듈
* `react-router-redux`: redux를 사용시 react-router와 redux를 integrate하는데 유리, but Project Deprecated됨, react-router 2.x and 3.x이랑만 호환됨
* `connected-react-router`: React Router v4 and v5에 binding

-----------------------------

### history package
session history를 manage하는 패키지
* 3가지 타입의 history object가 있다.
 * `createBrowserHistory`: HTML5 history API를 지원하는 웹브라우저용 (see cross-browser compatibility)
  * (tmi: HTML5라고 불리우는 개념은 단순히 웹 문서를 작성할 때 사용되는 마크업 랭귀지(HTML)의 문법적(syntactic) 버전뿐만 아니라 새로운 DOM API 스펙을 포함하는 것)
 * `createMemoryHistory` : DOM 기반이 아닌 환경에서 사용(React Native 같은 곳..)
 * `createHashHistory`: 기존 legacy 브라우저용

-----------------------------

### URL 이동시키기
* `Link`: a 태그와 같다. 하지만 SPA 특성상 a태그 처럼 새로고침이 발생하면 안되기에, a 태그를 기반으로 기능상의 개선을 통해 새로고침없이 다른 뷰를 렌더 하기위해 사용하는 것이다.
* `history.push`: Route는 컴포넌트에 기본적으로 match, history, location 이라는 것을 넘겨준다.  
이때 histroy.push(‘/인자’) 함수에 인자를 넣어주면 해당 인자로 url을 새로고침 없이 이동시켜준다.

-----------------------------

### match/historylocation
Route의 컴포넌트에 기본적으로 props로 match, history, location을 넘겨준다.
* history
  * 브라우저의 window.history 와 유사
  * 주소를 임의로 변경하거나 되돌아 갈 수 있도록 한다.
  * 주소 변경시, SPA 특성을 지키기 위해 페이지 전체를 리로드 하지 않는다.
  * location(제일 중요), replace, push 등 이 포함되어 있다.
  * Q. replace와 push의 차이점은?  
  push와 차이점은 페이지 방문 기록을 남기지 않아서 페이지 이동 후 뒤로가기 버튼을 눌렀을 때 방금 전의 페이지가 아니라 방금 전의 전 페이지가 나타난다.
  
* location
  * 브라우저의 window.location 와 유사 
  * 현재 페이지가 어디있는지(where) 정보를 지니고 있다.
  * url의 query 정보를 search라는 프로퍼티에 가지고 있다.

* match
  * Route의 path에 정의한 것과 매칭된 정보를 가지고 있다.
  * params 에 설정한 파라미터를 담고 있다.
  
-----------------------------

### Route 컴포넌트가 아닌 컴포넌트에서 history object 접근하는 방법: `withRouter`
```jsx
 import {
   BrowserRouter as Router, 
   Route,
   Link
 } from 'react-router-dom'
(생략)
ReactDOM.render(
  <Provider store={store}>
      <Router  history={history}>
        <div className="wrap">
          <HeaderContainer/>
                <Route exact path="/" component={App} />
                <Route exact path="/intro" component={IntroContainer} />
            <Footer/>
        </div>
    </Router>
   </Provider>
  , rootElement);
```
HeaderContainer처럼 Route밖에 있는 Component는 history 접근이 안된다.  
그래서 쓰는 방법이 아래와 같이 해당 Component를 withRouter 로 감싸주면 된다.  
```jsx
import {withRouter} from 'react-router-dom'

class HeaderContainer extends Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }
  render () {
    const { match, location, history } = this.props

    return <h2>The Header</h2>
  }
}

export default withRouter(HeaderContainer)
```
* 자세한 설명은 [여기서](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/redux.md)

### withRouter  
* withRouter는 `history`객체의 properties와 match 속성에 접근할수 있게 하는 higher-order component 이다.  
* withRouter will pass updated match, location, and history props to the wrapped component whenever it renders.

-----------------------------


### 컴포넌트 밖에서 페이지이동
* history.push outside of Component
#### 방법1
* dumb Component 또는 simply render하는 Component를 `withRouter`로 wrapping하여 연결하는 방법
* 문제: side-effects 이 있으니 recommend 하는 방법아님.

#### 방법2 
`history.js` :
```jsx
import createBrowserHistory from 'history';
const history = createBrowserHistory();
export default history;
```
`index.js`:
```jsx
import { Router } from "react-router-dom";
import history from "./history.js";

// and then in your JSX:
return (
  <Router history={history}>
    {/* routes as usuall */}
  </Router>
)
```
* 문제: BrowserRouter는 자신만의 history instance를 생성하여 사용하지 때문에 이런 url변경을 감지하지 못한다. url은 변경되나 component가 re-rendering 되지 않는 현상 발생.
* 해결: `BrowserRouter` 대신 `Router`를 사용함.
* [관련 comment](https://github.com/ReactTraining/react-router/issues/3498)


-----------------------------
