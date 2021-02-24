# application.yaml vs application.properties
Spring에서 configuration data을 properties file에 둔다.
Spring Boot에서는 properties file 대신 YAML파일을 사용할 수 있다.
* application.properties 예시
```properties
spring.datasource.url=jdbc:h2:dev
spring.datasource.username=SA
spring.datasource.password=password
```
* application.yaml 예시
```yaml
spring:
    datasource:
        password: password
        url: jdbc:h2:dev
        username: SA
```
그렇다면 application.yaml과 application.properties 차이는 뭐지?  
*(참고)properties는 key-value 쌍의 정보고, 그 properties가 모여있는 하나의 파일을 profile이라 한다.*

## Properties Configuration
* Placeholders in Properties
`${}` syntax를 쓰면 Properties file 내 다른 key,ystem properties 또는 environment variables을 불러 사용할 수 있다.
```
app.name=MyApp
app.description=${app.name} is a Spring Boot application
```
* List Structure 사용
```properties
application.servers[0].ip=127.0.0.1
application.servers[0].path=/path1
application.servers[1].ip=127.0.0.2
application.servers[1].path=/path2
application.servers[2].ip=127.0.0.3
application.servers[2].path=/path3
```
* Multiple Profiles 설정
    * Spring Boot 2.4.0 이후 변경 사항
        * application.properties도 Multiple Profiles 설정이 가능.
        * `#---` notation을 사용해 document를 구분한다.
        * Properties file에 profile 설정
            * `spring.profiles` => `spring.config.activate.on-profile`로 변경
        ```properties
        # 공통 profile
        logging.file.name=myapplication.log
        bael.property=defaultValue
        #---
        # dev profile
        spring.config.activate.on-profile=dev
        spring.datasource.password=password
        spring.datasource.url=jdbc:h2:dev
        spring.datasource.username=SA
        bael.property=devValue # 공통 profile에 bael.property는 override 됨
        #---
        # prod profile
        spring.config.activate.on-profile=prod
        spring.datasource.password=password
        spring.datasource.url=jdbc:h2:prod
        spring.datasource.username=prodUser
        bael.property=prodValue
        ```
* Profiles Across Multiple Files
    * Spring Boot 2.4.0 이전에는 application.properties를 1개만 설정할 수 있었다. 이 이후로는 application-dev.properties,application-prd.properties로 여러 properties file을 둘 수 있다.
    
## YAML Configuration
prefixes를 반복적으로 사용하지 않고 hierarchy 구조로 properties file보다는 가독성이 높다.  
YAML은 Maps, lists, and scalar types을 지원한다.
* Placeholders in Properties
* List Structure 사용
```yaml
application:
    servers:
    -   ip: '127.0.0.1'
        path: '/path1'
    -   ip: '127.0.0.2'
        path: '/path2'
    -   ip: '127.0.0.3'
        path: '/path3'
```
* Multiple Profiles 설정
yaml 파일은 three dashes(`---`)로 새로운 document 시작을 나타낸다.
```yaml
logging:
  file:
    name: myapplication.log
---
spring:
  config:
    activate:
      on-profile: staging
  datasource:
    password: 'password'
    url: jdbc:h2:staging
    username: SA
bael:
  property: stagingValue
```
* Profiles Across Multiple Files
    * application.yml도 application-dev.yml, application-prd.yml 등으로 profile에 따라 여러 파일을 둘 수 있다.

## Conclusion
* yaml 파일이 좀더 가독성 높다.
* Spring Boot 2.4.0 이전에는 properties 파일에는 제약들이 있었는데 이제는 다 해결됨
* 다만 yaml은 python, Ruby, Kubernetes 등에서 많이 쓰니 Java에서도 yaml쓰면 호환도 되고.. 좋을 듯 하다.(개인적 의견)

## Reference
* [spring-boot-yaml-vs-properties](https://www.baeldung.com/spring-boot-yaml-vs-properties)
* [Spring Boot에서 yaml과 properties 차이](https://www.geeksforgeeks.org/difference-between-yaml-yml-and-properties-file-in-java-springboot/)
* [Spring Boot 2.4부터 변경되는 Config File 설정](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.4-Release-Notes)
* [Yaml Specification](https://yaml.org/spec/1.2/spec.html)