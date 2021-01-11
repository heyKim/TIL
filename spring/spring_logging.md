# Spring Logging
- 왜 logging framework가 필요할까?
  - system.out.println 한줄 남기는데 io 리소스를 너무 많이 먹어서 시스템이 느려짐

- Logging Framework 종류
  - Logging Facade(추상체): apache commons logging(=Jakarta commons logging, JCL), slf4j
  - Logging Framework: log4j, logback, log4j2
 - spring은 기본적으로 commons-logging을 사용
 - spring boot는 기본적으로 Commons Logging -> SLF4j -> Logback의 흐름을 타고 결국 Logback에 의해서 찍힌다.
  
## Logging Facade
- logging facade는 실제 로깅을 하는 게 아니라 facade(추상체) 즉, 자바에서는 말하는 interface와 같은 역할을 해준다.
- 로깅 파사드의 장점은 로거들을 바꿔서 사용할 수 있다는 것
- 즉, log4j, logback, log4j2 등 어떤 로깅 프레임워크를 써도 실제 코드에 영향이 없다는 장점이 있다.
### apache commons logging
- 자카르타 재단에서 만든 로깅 유틸리티
- Jakarta commons logging, JCL이라고도 불림


### slf4j(Simple Logging Facade For Java)
- 로깅 프레임워크에 대한 추상화를 제공함
-

## Logging Framework
- log4j, logback, log4j2 순으로 등장
----------
### log4j
- 오래된 로깅 프레임워크로 2015년 부터 개발 중단됨. 사용안하는 게 좋음
#### Log4j 구성요소
|요소   | 설명                             |
|:-------:|:------|
|Logger | 출력할 메세지를 Appender에 전달함 |
|Apender| 전달된 로그를 어디에 출력할 지 결정|
|Layout | 로그를 어떤 형식으로 출력할 지 결정|

### Log Level
|로그레벨| 설명 |
|:---:  |:---  |
|FATAL  |아주 심각한 에러가 발생한 상태를 나타낸다.|
|ERROR  |어떠한 요청을 처리하는 중 문제가 발생한 상태를 나타낸다.|
|WARN   |프로그램 실행에는 문제가 없지만, 향후 시스템 에러의 원인이 될 수 있는 경고성 메세지를 나타낸다.|
|INFO   |어떠한 상태변경과 같은 정보성 메세지를 나타낸다. |
|DEBUG  |개발시 디버그 용도로 사용하는 메세지를 나타낸다.|
|TRACE  |디버그 레벨이 너무 광범위한 것을 해결하기 위해서 좀 더 상세한 이벤트를 나타낸다.|  

 `TRACE` < `DEBUG` < `INFO` < `WARN` < `ERROR` < `FATAL` 순이다.
 
------
### Logback
- log4j를 만든 개발자가 만듬
- log4j와 유사하지만 향상된 성능과 필터링 옵션, 자동 리로드 기능을 제공함
- log4j는 메모리 관리를 따로해서 io쪽에서 메모리를 많이 안먹고 빠르다.근데 막쓰면 system.out.println처럼 리소스 많이 먹음.
- Auto Reload란? 리눅스 서버 내에서 log4j를 사용할 시 log level을 변경 하게 되면, 서버를 재가동하여 반영사항을 적용해야한다.  
하지만 Logback은 logback.xml(혹은 logback-spring.xml)을 서버 재가동 없이 즉각 자동 리로드를 지원한다.

#### Logback module
- logback-core: 다른 두 모듈의 기반이 되는 핵심 코어 컴포넌트
- logback-classic: `slf4j`에서 사용이 가능하도록 만든 플러그인 컴포넌트. 기본적으로 SLF4J API를 구현함.
- logback-access: 사용하는 어플리케이션이 "웹 어플리케이션"일 경우 빛을 바라는 컴포넌트. Tomcat이나 Jetty같은 서블릿 컨테이너와 통합돼 HTTP-acess 로그 기능을 제공함.

#### Logback Class
logback은 세 가지 주요 클래스(Logger, Appender, Layout)을 기반으로 구현되어 있음  
Logger 클래스는 logback-classic 모듈의 일부인 반면에 Appender와 Layout 인터페이스는 logback-core의 일부이다.

#### Logback 설정
1. commons-logging을 사용하고 있다면 dependency에서 제거
2. logback 사용을 위한 dependency 추가(logback-classic만 있어도 기본적인 Log는 사용가능)
```xml
<dependencies>
  <dependency>
  <groupId>ch.qos.logback</groupId>
  <artifactId>logback-classic</artifactId>
  <version>1.1.2</version>
  </dependency>
</dependencies>
```
3. logback.xml(혹은 logback-spring.xml) 설정
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration> //logback이 구동될때 logback 상태를 확인할 수 있습니다.
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
  <layout class="ch.qos.logback.classic.PatternLayout">
  <Pattern>%d{HH:mm} %-5level %logger{36} - %msg%n</Pattern>
  </layout>
  </appender>

  <logger name="example.logback.level.grandparents" level="TRACE"/>
  //example.logback.level.grandparents 이하 모든 Logger들의 level은 TRACE라는 설정입니다.

  <logger name="example.logback.level.grandparents.parents.children" level="INFO"/>
  //example.logback.level.grandparents.parents.children 이하 모든 Logger들의 level은 INFO라는 설정입니다.

  <root level="DEBUG">
             <appender-ref ref="STDOUT" />
  </root>
  // 모든 대상에 STDOUT Appender를 적용하고 level이 DEBUG이하인것만 처리로 설정됩니다.
</configuration>
```
4. logback 사용
```java
import ch.qos.logback.classic.LoggerContext;
import ch.qos.logback.core.util.StatusPrinter;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Tutorial {
  // Logger은 꼭 private static이 붙어야 한다. getLogger(클래스명.clss) 적어라.
  private static final Logger logger = LoggerFactory.getLogger(Tutorial.class); 

  public static void main(String[] args) {
    logger.trace("trace");
    logger.debug("debug");
    logger.info("info");
    logger.warn("warn");
    logger.error("error");
  }
}

```
---------------
### log4j2
- 가장 최신 로깅 프레임워크


### 참고
- https://velog.io/@ym1085/log4j-vs-logBack
- logback: https://velog.io/@hanblueblue/%EB%B2%88%EC%97%AD-logback
- https://ram2ram2.tistory.com/7
