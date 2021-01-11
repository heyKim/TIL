# HTTP Requests in Node.js
1. HTTP- Standard HTTP Library
* http 와 https 모두 기본 library라 npm으로 설치할 필요없다.
* 가장 쉽게 사용가능하지만 low-level module이고 다른 package에 비해 user-friendly 하지 않다.
* 사용법
```js
const https = require('https');

https.get('https://jsonplaceholder.typicode.com/todos/1', (response) => {
  let todo = '';

  // called when a data chunk is received.
  response.on('data', (chunk) => {
    todo += chunk;
  });

  // called when the complete response is received.
  response.on('end', () => {
    console.log(JSON.parse(todo).title);
  });

}).on("error", (error) => {
  console.log("Error: " + error.message);
});
```

2. Request
* 단순한 HTTP client이다. npm으로 설치가 필요하다.
```js
const request = require('request');

request('https://jsonplaceholder.typicode.com/todos/1', { json: true }, (err, res, body) => {
  if (err) { 
      return console.log(err); 
  }
  console.log(body.id);
  console.log(body.title);
});
```

3. Needle
* streamable HTTP client로 2.0.x이상부터 Promises(event chain에 유용함)도 supported함
* proxy, iconv, cookie, deflate, multi-part requests 지원함
```js
const needle = require('needle');

needle('get', 'https://jsonplaceholder.typicode.com/todos/1', { json: true })
    .then(res => {
        let todo = res.body;
        console.log(todo.id);
        console.log(todo.title);
    }).catch(err => {
        console.log(err);
    });
```

4. Axios 
* Browser나 Node.js를 위한 Promise 기반의  HTTP client
* Axios는 response data를 자동으로 JSON object로 변환 시켜준다. 
```js
const axios = require('axios');

axios.get('https://jsonplaceholder.typicode.com/todos/1')
  .then(res => {
    console.log(res.data.id);
    console.log(res.data.title);
  })
  .catch(err => {
    console.log(err);
  });
```
* Axios의 가장 큰 장점은 `axios.all`을 이용해 multiple concurrent request를 한다는 것이다.
```js
const axios = require('axios');

axios.all([
      axios.get('https://jsonplaceholder.typicode.com/todos/1'),
      axios.get('https://jsonplaceholder.typicode.com/todos/2')
    ]).then(axios.spread((res1, res2) => {
      console.log(res1.data.title);
      console.log(res2.data.title);
    })).catch(err => {
      console.log(err);
    });
```

5. SuperAgent
* 이것도 Axios와 비슷하게 reponse data를 JSON으로 변환시켜준다.
```
const superagent = require('superagent');

superagent.get('https://jsonplaceholder.typicode.com/todos/1')
.end((err, res) => {
  if (err) { 
      return console.log(err); 
  }
  console.log(res.body.id);
  console.log(res.body.title);
});
```

6. Got
* Node.js를 위한 user-friendly하고 lightweigh한 HTTP request library이다.
* Axios나 Needle처럼 Promises 를 support한다.
```
const got = require('got');

got('https://jsonplaceholder.typicode.com/todos/1', { json: true })
    .then(res => {
      console.log(res.body.id);
      console.log(res.body.title);
    }).catch(err => {
      console.log(err.response.body);
    });
```

7. Node-fetch
* light-weight한 HTTP request library
* Axios처럼 최근에 Promises를 지원한다.
```
const fetch = require('node-fetch');

fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(res => res.json()) // expecting a json response
    .then(json => {
        console.log(json.id);
        console.log(json.title);
    })
    .catch(err => {
        console.log(err);
    });
```

더 많은 HTTP client를 보려면 [여기](https://github.com/zeke/npm-collection-http-clients)
