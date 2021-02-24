# Spring Annotaion
* [@Bean](#bean) - Bean등록. 단, 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우 사용.
* [@Component](#component) - Bean등록("스프링아 난 너가 관리해야 하는 빈이야"라고 말하는 것). 단, 개발자가 직접 컨트롤이 가능한 Class인 경우에 사용.
* [@ComponentScan](#componentscan) - 빈 등록하기
* [@EnableAutoConfiguration](#enableautoconfiguration) - 설정 자동 등록하기
* [@PostContruct](#postcontruct) - Bean 객체 생성한 이후 초기화를 수행할 메소드 지정
-------------
#### Bean
- `Spring(IOC) Container`에 Bean을 등록하도록 하는 메타데이터를 기입하는 어노테이션
- 아래 예제와 같이 라이브러리 등을 Bean으로 등록하기 위해서 별도로 해당 라이브러리 객체를 반환하는 Method를 만들고 `@Bean` 어노테이션을 붙여주면 됨
```
@Configuration
public class DatabaseConfig {
	
	@Autowired
	PropertyConfig propertyConfig;
	
	@Bean
	public DataSource datasource() {
		System.out.println(propertyConfig.getUrl());
		return DataSourceBuilder.create().build();
	}
```
- Bean어노테이션에 아무런 값을 지정하지 않으면 Method 이름을 CamelCase로 변경한 것이 Bean id로 등록된다.(위 예제에서는 datasource)
- Bean id를 지정하고 싶으면 `@Bean(name=dataSource1)` 이렇게 지정해주면 된다.

#### Component
- `Spring(IOC) Container`에 Bean을 등록하도록 하는 메타데이터를 기입하는 어노테이션
- 단, 개발자가 사용하기 위해 직접 작성한 Class에 사용한다.
- `@Component` 역시 아무런 값을 지정하지 않으면 Class 이름을 CamelCase로 변경한 것이 Bean id로 사용된다.
- Bean id를 지정하고 싶으면 `@Component(value=dbconfig)` 이렇게 지정해주면 된다.
- `@Component`를 사용한 Bean의 의존성 주입은 `@AutoWired` 어노테이션을 이용하면된다. 
 - 위의 예제에서 처럼 DatabaseConfig가 PropertyConfig에 대한 의존성을 가지고 있는 경우 `@AutoWired`을 이용해 의존성을 자동으로 주입할 수 있음
  - `@AutoWired`의 경우 **형(타입)** 을 통해 해당 자리에 들어올 객체를 판별하여 주입한다. 따라서 해당 자리에 들어올 수 있는 객체가 여러개인 경우, 즉 다형성을 띄고있는 객체타입에 `@AutoWired`를 사용한 경우에는 `@Qualifier("bean이름")`을 이용하여 해당 자리에 주입될 Bean을 명시해주어야 한다.
  - 예를 들어 Multi DB Connection으로 Datasource가 2개이면 `@Qualifier`를 이용해서 어떤 것을 주입할지 지정해줘야함.
#### ComponentScan
- 스프링에서 관리하는 POJO(Plain Old Java Object)를 '빈(Bean)'이라고 한다.
- `@ComponentScan` 어노테이션은 현재 패키지 이하에서 `@Component` 어노테이션이 붙어 있는 클래스들을 찾아서 스프링 IOC Container에 빈으로 등록한다.
- `@Configuration`,`@Repository`,`@Controller`,`@Service`,`@RestController` 어노테이션이 붙은 클래스도 Bean으로 등록(해당 어노테이션이 내부에 `@Component` 어노테이션을 가지고 있음)


#### EnableAutoConfiguration
- `@EnableAutoConfiguration`은 미리 정의되어 있는 빈들을 가져와서 등록해준다.
- 미리 설정된 빈들은 어디 정의되어 있는 것인가?
  - 외부 라이브러리(External Libraries) 중 spring-boot-autoconfigure -> META-INF -> spring.factories 파일에 자동으로 가져올 Bean들이 정의되어 있다.
  - 이 위치에 `org.springframework.boot.autoconfigure.EnableAutoConfiguration`를 키값으로 하는 모든 패키지를 가져온다.
  - 각 클래스를 까보면 `@Configuration`으로 설정되어 있다. (@Configuration 은 SpringFramework에서 설정 파일로 쓴다는 것)
  - 여기서 모든 Configuration을 가져오는 것이 아니라, 각각의 클래스에 `@Conditional`이라는 조건이 달려있다. 이걸로 커스터마이징 가능.
  
#### PostContruct 
`@PostContruct`: 의존하는 객체를 설정한 이후에 초기화 작업을 수행할 메서드에 적용됨.
  - DataSourceConfig 클래스 이외에서 database.properties 프로퍼티 파일의 설정값을 가져다 쓰려고 할 때 Null Pointer Exception이 발생했었다.  
  - 이는 `@Autowired Environment env;` 가 DI되지 않아 Null Pointer Exception이 발생한 것이다. 만약 @Configuration 빈 설정 클래스가 너무 빠르게 초기화된다면, 오토와이어링이 동작하지 않을 수도 있다.
  - 이때 `@PostContruct` 어노테이션을 이용했었음
  ```java
  @Configuration
  @PropertySource
  public class DataSourceConfig {

     @Autowired
     private Environment env;

     private static Environment environment;

     @PostConstruct
     public void init(){
       environment = env;
       System.out.println(environment == env);
     }

     public static String getPopertyValue(String property){
        return environment.getProperty(property);
     }
  }
  ```
  - (참고) https://stackoverflow.com/questions/35394289/environement-is-null-when-i-try-to-autowire
