# Axios

## Internet Explorer 에서 캐싱 제거
*  axios 날릴 때, header에 "Pragma" : 'no-cache' 옵션 추가
```jsx
 axios({
    url: `/user`, 
    method : 'get',
    headers: {  "Pragma" : 'no-cache', //Internet Explorer 에서 캐싱 제거
                "x-access-token": storage.getToken() //토큰추가
             }
}).then(
    (res)=>{
    }
).catch(
    (err) => {
    }
)
```

## Axios Interceptor
* Axios 보내고 혹은 받을 때 가로채는 놈놈놈!
* `then`이나 `catch`를 타기 전에 requests와 response를 가로챈다.
* [기본적인 사용방법](https://www.npmjs.com/package/axios)
```jsx
// Add a request interceptor
axios.interceptors.request.use(function (config) {
    // Do something before request is sent
    return config;
  }, function (error) {
    // Do something with request error
    return Promise.reject(error);
  });
 
// Add a response interceptor
axios.interceptors.response.use(function (response) {
    // Do something with response data
    return response;
  }, function (error) {
    // Do something with response error
    return Promise.reject(error);
  });
```

* 행성캠 사이트에서 Axios Interceptor 사용
  * why? backend와 token을 주고받고, error handling을 하는데 같은 코드가 계속 반복되었음!
  * How? 
  ```jsx
  /** Axios Interceptor 적용 */
  export const errorInterceptors = (store, history) => {
      axios.interceptors.response.use(function (response) {
          /** 정상동작시 do something */
          return response;
      }, function (err) {
          /** 에러발생시 do something */
          if(err.response.status==999){
              alert("로그인이 필요합니다. 로그인 페이지로 이동합니다.");
              history.push("/login");
          }else if(err.response.status==998){
              alert("로그인 시간이 만료되었습니다. 로그인 페이지로 이동합니다.");
              //sessionStorage에 userInfo key의 데이터 삭제
              storage.removeSessionObj();
              storage.removeSerialNumber();
              //store의 login데이터 reset
              store.dispatch(menuActions.setClickMenu('/login'));
              store.dispatch(authActions.logout());
              history.push("/login");
          }else if(err.response.status==401){
              let msg = "잘못된 로그인 정보입니다. 로그인 페이지로 이동합니다";

              if(err.response.data.error.indexOf("가입승인") > -1){
                  msg = "아직 가입승인 전입니다.\n귀사 대표관리자의 가입승인 후 로그인할 수 있습니다."
              }
              else if(err.response.data.error.indexOf("ID 혹은 패스워드") > -1){
                  msg = "ID 혹은 패스워드를 확인해주세요"
              }
              else {}
              alert(msg);
              //sessionStorage에 userInfo key의 데이터 삭제
              storage.removeSessionObj();
              storage.removeSerialNumber();
              //store의 login데이터 reset
              store.dispatch(menuActions.setClickMenu('/login'));
              store.dispatch(authActions.logout());
              history.push('/login');
          }else{
              alert("알수없는 에러가 발생했습니다. 시스템 담당자에게 문의하세요.")
              console.log( err.response);
          }

          return Promise.reject(err);
      });
   }
   ```
   * [Interceptor에서 store 접근](https://github.com/heyKim/myMemo/blob/master/react/react-redux.md#component-%EB%B0%96%EC%97%90%EC%84%9C-store%EC%A0%91%EA%B7%BC)
   * [Interceptor에서 history 객체 접근](https://github.com/heyKim/myMemo/blob/master/react/react-router.md#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%B0%96%EC%97%90%EC%84%9C-%ED%8E%98%EC%9D%B4%EC%A7%80%EC%9D%B4%EB%8F%99)
  
* Interceptor remove
```jsx
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

* :eyes:TO-DO: Add interceptors to a custom instance of axios  
custom Axios에 interceptor를 적용해서 각가 다른 인터셉터를 적용 
```jsx
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
  interceptors //
});
instance.interceptors.request.use(function () {/*...*/});
```
