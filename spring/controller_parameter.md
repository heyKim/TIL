# Controller Parameter 정리 
* HttpServletRequest request 
* @RequestParam 
* @ModelAttribute 
* @RequestBody 
* Map, Model, ModelMap 
* Model 클래스를 통한 직접 매칭 
* @PathVariable 
* RequestEntity<?> 
* 기타 
------ 
## HttpServletRequest request 
```java 
@RequestMapping("/") 
public String test(HttpServletRequest request) { 
    String id = request.getParameter("id"); 
    return id; 
} 
``` 
 
## @RequestParam 
```java 
@RequestMapping("/") 
public String test(@RequestParam(value="id", defaultValue="heykim", required= true) String id) { 
    return id; 
} 
``` 
```java 
// Parameter가 여러 개인 경우에 Map으로도 받을 수 있음 
@RequestMapping("/") 
public String test(@RequestParam Map<String, String> parameters) { 
 
} 
``` 
* `required`를 `true`로 지정하고 전달 안하면 400 에러(MissingServletRequestParameterException) 발생하니 주의할 것 
* 위에 어노테이션 모두 생략 가능. 그러나 혼동을 피하기 위해 써주는 것이.. 
```java 
// 어노테이션 생략가능. 그러나 default로 required false임 
@RequestMapping("/") 
public String test(String id)) { 
} 
``` 
 
## @ModelAttribute 
* `@RequestParam`는 1:1로 파라미터를 받는 반면, ModelAttribute는 도메인 모델이나 DTO 같은 모델을 받는 타입이다. 
* 즉, 여러 파라미터들을 1:1로 객체에 바인딩해줌(바로 자바빈 객체로 바인딩) 
```java 
@GetMapping("/") 
public String test(@ModelAttribute Account account) 
``` 
 
## @RequestBody 
* `@RequestBody`를 사용할 경우 반드시 **POST**형식으로 응답을 받아야함. 
* JSON이나 XML같은 데이터를 Jackson과 같은 messageConverter를 사용하여 변환하여 사용 
 
## Map, Model, ModelMap 
```java 
@RequestMapping("/") 
public String test(Map<String, String> param) { 
    return param.getId().toString(); 
} 
``` 
 
## Model 클래스를 통한 직접 매칭 
```java 
@RequestMapping("/") 
public String test(CustomVo vo) { 
    return vo.getId(); 
} 
``` 
 
## @PathVariable 
```java 
@RequestMapping("/delete/{idx}") 
public String test(@PathVariable("idx") int idx) { 
    return testService.deleteId(idx); 
} 
``` 
* { }를 이용해 매핑되는 url 경로에 작성해주면 해당 @PathVariable 어노테이션이 작성된 파라미터에 자동으로 매핑된다. 
 
## RequestEntity<?> 
* RequestEntity: HttpEntity 상속 받음으로서 HTTP response 전체를 의미함(status code, headers, body) 
* parameter Value만 받는 것이 아니라 상태코드, 응답 메세지 같은 것들을 다같이 받고 싶을 때 활용 하면 좋음 
```java 
@PostMapping("/") 
public String test(RequestEntity<Account> account) { 
    return "hello"l 
} 
``` 
 
## 기타 
* @CookieValue: 저장된 쿠키 정보 가져올 때 사용 
* @RequestHeader: 헤더정보 가져올 때 사용 
 
 
## Reference 
* https://velog.io/@ye050425/spring-controller-parameter-%EC%A0%95%EB%A6%AC