# Swagger 
* Swagger는 OpenAPI 에서 생성된 api-docs 를 기반으로 api를 HTML 문서화 해줍니다. 
* OpenAPI 3.0은 swagger 3.0의 다른 이름이다. 
* swagger2(springfox-swagger2) 말고 swagger3(springdoc-openapi-ui) 사용할 것 
 
## Swagger 사용법 
1. 의존성 추가 
```yaml 
# (in build.gradle) 
dependencies{ 
	implementation 'org.springdoc:springdoc-openapi-ui:1.2.32' 
} 
``` 
2. Swagger 설정 
* `OpenAPI` type Bean을 추가하여 Doc을 만든다. 
```java 
@Configuration 
public class SwaggerConfig { 
	@Bean 
    public OpenAPI customOpenAPI() { 
        return new OpenAPI() 
                .info(new Info().title("Hello API") 
	                .description("hey! sample application") 
	                .version("v1.0.0")); 
    } 
} 
``` 
3. Swagger 
* `/swagger-ui.html` 로 접속하여 API가 보이는지 확인 
 
## Swagger 추가 설정 
### Swagger Properties 
```yml 
springdoc: 
  version: '@project.version@' 
  api-docs: 
    path: /api-docs 
  default-consumes-media-type: application/json 
  default-produces-media-type: application/json 
  swagger-ui: 
    operations-sorter: alpha 
    tags-sorter: alpha 
    path: /swagger 
    disable-swagger-default-url: true 
    display-query-params-without-oauth2: true 
  paths-to-match: 
  - /api/v1/** 
  - /test/** 
``` 
* `springdoc.swagger-ui.path: /swagger`: Swagger HTML 문서 경로(default: `/swagger-ui.html`). path를 새로 설정해도 `swagger-ui.html`로 redirection 되긴 함 
## Reference 
* [Swagger3](https://blog.jiniworld.me/83#a01-1)
