# API Management
* API 종합 선물세트..?ㅎㅎ
* 용어
  * Backend API: 비즈니스 논리로 구현하는 HTTP 서비스
  * Frontend API: 백엔드 API를 난독 처리하기 위해 API Management가 호스팅하는 HTTP 서비스 외관(백엔드 API를 바로 노출하지 않음)
  * Product: 하나 이상의 API와 사용 할당량 및 사용 약관
  * Operation: 백엔드 API의 특정 요청/응답과 상관 관계를 지정하는 프런트엔드 API의 특정 작업
  * Version
    * 프런트 엔드 API에 대해 호환성이 손상되는 변경
    * 프런트 엔드 API를 업데이트하거나 변경할 때 기존 어플리케이션의 코드를 변경할 필요가 없다
  * Revision
  * 개발자 포털: API 개요 페이지
* Product
  * 패키지에 하나 이상의 API가 포함되어 있음
  * 제품을 OPEN하거나 Protected 할 수 있음
  * 개발자가 API에 접근하는 단위

## 실습
### API Management 생성
* Portal에서 만들기
 * Add resource > API Management
  ```
  Resource group: hihikkh21rg
  Region: East US 2
  Resource name: hihikkh21apim
  Organization name: hihikkh
  Administrator email: jkk3366@hotmail.com
  Pricing tier: Developer (no SLA) # 꼭 Developer로 해야 개발자 센터 구성됨
  ```
* Command에서 만들기
```
$myLocation = "eastus2"
$myEmail = "jkk3366@hotmail.com"
$myAPIMname="hihikkh21apim2"
az apim create -n $myAPIMname -l $myLocation `
    --publisher-email $myEmail  `
    -g "hihikkh21rg" `
    --publisher-name waglnet `
    --sku-name Basic
```

### Add API
* API Management> APIs > Add API > OpenAPI
 * OpenAPI URL: `https://conferenceapi.azurewebsites.net/?format=json`
 
### Add product
API Management > Products > Add Product
![add product](https://user-images.githubusercontent.com/35487539/103979320-e9063180-51c0-11eb-8b1e-bbd7f24a7edc.JPG)

## API Management 인스턴스 만들기
* API 생성 및 관리
* 각 API에는 하나 이상의 작업 집합이 포함되어있음

관리
Git를 사용하여 관리

## API 정책
* API 요청 또는 응답 시 순착적으로 실행되는 명령문의 컬렉션
* 실제 백엔드 API애플리케이션에 대한 코드 변경 없이 API의 동작을 빠르게 변경할 수 있는 방법
* 포괄적인 정책 옵션 목록은 다음에서 찾을 수 있음
### API 정책 구성
* inbound, backend, outbound, on-error로 나뉨
```xml
<policies>
  <inbound>
    <!-- 응답에 적용될 문의 위치는 여기입니다 -->
  </inbound>
  <backend>
    <!-- 요청이 백 엔드 서비스로 전달되기 전에 적용할 문 
         위치는 여기입니다 -->
  </backend>
  <outbound>
    <!-- 응답에 적용될 문의 위치는 여기입니다 -->
  </outbound>
  <on-error>
    <!-- 오류 상황 발생 시 적용할 문의 위치는 여기입니다 -->
  </on-error>
</policies>
```
* 예)
```
<outbound>
    <set-header name="X-Powered-By" exists-action="delete" />
    <set-header name="X-AspNet-Version" exists-action="delete" />
    <base />
</outbound>
```
* 고급 정책 시나리오
  * mock response policy
    * DB성능빼고 부하테스트를 하고 싶은경우 mock response 사용하면 좋을 듯
    * `<mock-response status-code="code" content-type="media type"/>`
  * retry
   ```
    <retry
     condition="boolean expression or literal"
     interval="retry interval in seconds"
     max-interval="maximum retry interval in seconds"
     deltal="retry interval delta in seconds"
     count="number of retry attempts">
     <!-- 1개 이상의 자식 정책, No restrictions -->
    </retry>
   ```
  * return response
  ```
  <return-response response-variable-name="existing context variable">
    <set-header/><set-body/><set-status/>
  </return-response>
  ```
