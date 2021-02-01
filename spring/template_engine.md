# Template Engine 
- 템플릿 엔진이란 
- 템플릿 엔진 종류 
- 레이아웃 템플릿 엔진 VS 텍스트 템플릿 엔진 
- Spring Boot 템플릿 엔진 
 
## 템플릿 엔진이란? 
* 템플릿 양식과 특정 데이터 모델에 따른 입력자료를 합성하여 결과 문서를 출력하는 소프트웨어 
* 즉, Template + data-model = output 
## 템플릿 종류 
### 레이아웃 템플릿 엔진 
* 중복되는 `include`코드를 사용하지 않고도 지정된 페이지 레이아웃에 따라 페이지 타일을 조합하여 완전한 페이지로 만들어준다. 
* 종류:  Apache Tiles, Sitemesh 
### 텍스트 템플릿 엔진 
* 템플릿 양식에 적절한 특정 데이터를 넣어 결과 문서를 출력한다. 
* Freemarker, Thymeleaf, Groovy Markup Templates, Jade4j, Mustache 
 
## Spring Boot Template Engine 
* Spring Boot 공식 지원 템플릿 
    * [Freemarker](https://freemarker.apache.org/docs/) 
    * [Groovy](https://docs.groovy-lang.org/docs/next/html/documentation/template-engines.html#_the_markuptemplateengine) 
    * [Thymeleaf](https://www.thymeleaf.org/) 
    * [Mustache](https://mustache.github.io/) 
* 보통 템플릿 파일위치는 `src/main/resources/templates`에 둔다. 
* [Spring Boot Template Engines Comparison](https://springhow.com/spring-boot-template-engines-comparison/) 
 
## Reference 
* [Spring Boot Template Engine](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-spring-mvc-template-engines) 
* [Spring Boot Template Engines Comparison](https://springhow.com/spring-boot-template-engines-comparison/)