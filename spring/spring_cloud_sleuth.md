# Sleuth 
* Sleuth란? 
* Sleuth 특징 
* Sleuth의 ID 체계
* Sleuth 적용 테스트 
 
## Sleuth란? 
* Sleuth은 단어 뜻처럼 뒤를 쫓아준다.
* MSA 구조에서 클라이언트의 호출이 내부적으로 여러개의 서비스를 거쳐서 일어나서 전체 트랜잭션에 대한 로그 트레이싱이 어렵다. 
* 이를 쉽게 하기 위해 트랜잭션의 전 구간을 추적하기 위한 하나의 연관 ID가 필요하다. 
* Sleuth를 사용하면 이 연관 ID(traceID, spanID)를 자동으로 생성해 준다. 
* Sleuth는 지연 시간을 분석하기 위해 시간 정보를 기록하며, 이를 통해 사용자는 어플리케이션에서 시간이 지연되는 원인을 정확히 찾아낼 수 있다. 
 
## Sleuth 특징 
* Sleuth는 필요 이상으로 로그를 남기지 않으며 응용 프로그램이 충돌하지 않도록 설계되었다. 
* Slf4J MDC(Mapped Diagnostic Context)을 통해 Trace와 Span Ids를 로그에 추가할 수 있다. 이를 통해 로그로 부터 Trace와 Span 정보들을 로그 수집기에서 추출할 수 있다. 
    * 스프링 어플리케이션의 공통 진출입점에 적용 가능하다.   
    (servlet filter, rest template, scheduled actions, message channels, zuul filters, feign client) 
* Slf4J MDC에 trace Id와 span Id를 추가함으로써 집계기에 지정된 trace와 span으로부터 모든 로그를 추출할 수 있다. 
* MDC의 [appname,traceId,spanId,exportable] 항목 
    * appname: span이 기록된 어플리케이션 이름 
    * traceId: single request에 할당된 id. span이 포함된 지연시간 그래프의 아이디 
    * spanId: 발생한 특정 작업의 아이디 
    * exportable: 로그를 Zipkin으로 전송할지에 대한 여부. 일부 작업을 span에 래핑하고 로그로만 기록하려는 경우에 전송하지 않을 수 있다. 

## Sleuth의 ID 체계 
* Span ID 
    * 작업의 기본 단위이다. 각 서비스 호출시에 새로운 Span이 하나 생성
    * 유일한 64-bit ID로 구분   
    * Description, key-value annotation, process ID 등의 추가 정보를 가짐 
    * Trace에서 제일 처음 만들어지는 Span을 root span이라 함(trace id와 동일) 
* Trace ID 
    * 최초 호출시 인입 서비스에서 생성  
    * span들을 모아둔 것으로 tree와 비슷한 구조로 되어있음 
    * 유일한 64-bit ID로 구분 
    * 예를 들어 분산 데이터 서비스를 운영할때, 하나의 Put 리퀘스트에 의해 하나의 Trace가 생성 
 
 
## Sleuth 적용 테스트 
1. Sleuth dependency 추가 
```yml 
dependencies { 
    implementation 'org.springframework.cloud:spring-cloud-starter-sleuth' 
} 
```
2. sleuth configuration
```yml
# Enable default AsyncConfigurer
spring.sleuth.async.configurer.enabled=true
```