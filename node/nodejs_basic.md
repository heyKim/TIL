# Nodejs 기본
*  하나의 REST block 안에서는 1개의 `res.send`만 가능하다.
* next() : 현재의 미들웨어 함수가 요청-응답 주기를 종료하지 않는 경우에는 next()를 호출하여 그 다음 미들웨어 함수에 제어를 전달할 수 있다.


# Express
Express의 장점 중 하나가 Middleware를 사용한다는 것이다.
미들웨어에는 대표적으로 Morgan, Compression, Session, Body-parser, Cookie-parser, Method-override, Cors, Multer 등이 있다.
 * Morgan: 익스프레스 프레임워크가 동작하면서 나오는 메세지들을 콘솔에 표시해준다.
 * Compression: 이름처럼 페이지를 압축해서 전송함
 * Session: 세션을 사용할 수 있게함.
 * Body-parser은 폼에서 전송되는 POST 값을 사용하게 해줌
 * Cookie-parser는 쿠키를 사용할 수 있게 해줌
 * Method-override는 REST API에서 PUT과 DELETE 메소드를 사용할 수 있게 해줌
 * Cors는 크로스오리진(다른 도메인 간의 AJAX 요청)을 가능하게 해줌
 * Multer는 파일업로드를 할 때 주로 쓰임

-------------------------------
## Express Application에서 사용할 수 있는 미들웨어
* Application-level middleware
* Router-level middleware
* Error-handling middleware
* Built-in middleware
* Third-party middleware

### Application-level middleware
`app.use()` or `app.METHOD()`를 이용하여 Application-level middleware를 app object에 bind하여 사용한다.
여기서 METHOD는 미들웨어 함수가 처리하는 요청(get, put, post 등)의 소문자로 된 HTTP 메소드

#### 종류
* app.all(), app.get(), app.post(), app.put(), app.delete() 
* app.disable(), app.disabled(),app.enable(), app.enabled() 
* app.listen(): 특정 host와 port와의 연결을 listening한다.
* app.render()
* app.route(): HTTP를 사용하는 하나의 route instance를 리턴한다.
* app.set(name, value), app.get(value): name에 value를 set하거나 name의 value를 get해줌 
* app.use(path, callback): 특정 middleware나 function을 특정 path로 마운트 시킨다.

* 예) no mount, 요청이 들어올 때 마다 실행된다.
```js
const app = express()

app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

* 예) 모든 HTTP request던지 실행됨
```js
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```
* 예) 특정 HTTP Reeust에만 실행
```js
app.get('/user/:id', function (req, res, next) {
  res.send('USER')
})
```

### Router-level middleware
`express.Router()`에 바인딩 시켜 사용한다는 것 말고는 Application-level middleware와 거의 차이가 없다.

* 
```js
var app = express()
var router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// mount the router on the app
app.use('/', router)
```

* 나머지 router’s middleware functions을 스킵하고 싶다면 `next('router')`를 호출하면 된다.

### Error-handling middleware
Error-handling middleware이 다른 middleware랑 다른 점은 인자가 4개라는 점
```js
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

### Built-in middleware
Express 4.x부터는 `Connect(미들웨어 프레임워크)`와의 의존성이 사라졌다.  
기존에 Express에 포함되었던 `body-parser`,`compression`, `cookie-parser`등의 function들이 개별 npm module들로 분리되었다.
남은 function들은 3개이다.
* express.static: HTML, 이미지 같은 정적인 파일을 서비스 해준다.
* express.json:  parses incoming requests with JSON payloads. * NOTE: Express 4.16.0+ 부터 사용가능
* express.urlencoded:  parses incoming requests with URL-encoded payloads. * NOTE: Express 4.16.0+ 부터 사용가능

### Third-party middleware
[다양한 Third-party middleware](https://expressjs.com/en/resources/middleware.html)가 존재한다.

----------------------------
* app object
: Express 서버가 생성될 때 초기화되는 object. middleware를 세팅할 때 사용
app object는 Express application을 의미한다. app object를 create하기 전에 아래와 같이 항상 그 위 레벨에 express() function을 호출해야한다.
```js
const express = require('express')
const app = express()
app.listen(3000)
```
`app` object has methods for
* Routing HTTP requests ex) `app.METHOD and app.param`  
* Configuring middleware ex) `app.route`
* Rendering HTML views ex) `app.render`
* Registering a template engine. ex) `app.engine`

## Express Method
* app.use(middleware): 미들웨어를 쓰는 부분
```js
const app = express();
app.use();
```

* app.listen: 설정한 port로 요청을 받을 수 있게 대기하고 있는다.
```js
app.listen(port, () => {
  console.log('Express App on port '+ port); 
});
```

----------------------------
Q. app.use() 와 router.use()의 차이점은?
router.use()를 사용할 때 장점이 있다.
Router 객체를 생성하고 이 객체에 미들웨어나 라우팅 설정을 추가 할 수 있다.
이는 이전에 라우팅마다 일일히 처리하는 부분 없이 기능이나 로직별로 라우팅을 나눠서 관리할 수 있다는 의미이다.
그리고 Router를 app.use('/api', require('./router');처럼 연결할 수 있어서 Router 전체에 대한 URL 접미사를 한꺼번에 관리하는 것도 가능하다. 
아래의 예를 보면 이해하기 쉽다.
```js
const router = express.Router();
app.use('/first', router);  // Mount the router as middleware at path /first
router.get('/sub', require('./router/smaller')); // /first/sub로 요청이 들어오며 smaller 함수가 실행된다. /first/sub 요청에만 미들웨어를 추가 할수도 있다.
router.get('/user', require('./router/bigger'));
```
----------------------------
### app.router is deprecated
`app.use(app.router)`는 생략해도 express가 자동으로 추가하기 때문에 생략하면 설정 마지막에 자동으로 추가해준다.
```js
app.use(cookieParser());
app.use(bodyParser());
// app.use(app.router); 이 코드를 더이상 사용하지 않는다.

app.get('/' ...);
app.post(...);

// middleware(이 미들웨어는 라우팅 코드 이후에 호출 된다.)
app.use(function(req, res, next);
```
굳이 이 코드를 명시적으로 써주는 경우가 있다면, 라우팅 처리를 어느 단계에서 처리할 것인가를 지정하기 위해서이다.
라우팅 이후에 실행되어야 하는 미들웨어가 있다던가 정적 리소스 URL과 URL이 겹쳐서 명시적으로 우선순위를 주고 싶을 때 등등




