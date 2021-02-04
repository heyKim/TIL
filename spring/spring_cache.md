# Spring Cache 
* Cache란 
* Cache를 왜 사용할까? 
* Spring Boot Cache 
* Spring Boot 공식지원 Third-Party Cache 
* Cache 사용법 
* Multiple Cache Managers 
 
## Cache란 
* 한번 읽어온 데이터를 임시로 저장하고 필요에 따라 전송,갱신, 삭제하는 기술이다. 
* 디스크에서 데이터를 가져오는 것보다 훨씬 빠르다. 
* 서버가 다운되면 사라지는 휘발성 데이터 
* 캐시는 여러 위치에 적용할 수 있다. 
    * DB 조회와 관련된 캐시면 데이터 액세스 기술에 적용 
    * JPA나 Hibernate 같은 ORM은 자체적으로 캐시 서비스 기능을 제공 
    * DB 내부적으로 제공하는 캐시 기능을 활용 
    * 웹 서버 또는 전용 캐시 장비가 제공하는 웹 캐시 기능을 사용해 HTML을 통째로 저장해서 사용할 수도 있음 
    * 빈의 메소드 결과를 캐시에 저장해두는 것이 적절한 경우 스프링이 제공하는 캐시 추상화 기능 이용 
 
## Cache를 왜 사용할까? 
* 불필요한 서버 트래픽을 줄일 수 있다. 
* REST API의 성능 개선 
 
## Spring Boot Cache 
* 스프링 3.1 이상부터는 캐시 추상화 기술을 제공한다. 
* Spring은 Third-Party Cache 구현체를 위한 Layer 만 제공한다. 개발자는 Third-Party Cache 구현체를 골라서 쓰기만 하면됨. 나중에 변경사항이 생겨서 바꾸기도 쉬움 
* 설정과 어노테이션 추가만으로 캐시를 쉽게 적용할 수 있음. 
 
## Spring Boot 공식지원 Third-Party Cache 
* Generic 
* JCache (JSR-107) (JCache 구현체인 EhCache 3, Hazelcast, Infinispan, and * others) 
* EhCache 2.x 
* Hazelcast 
* Infinispan 
* Couchbase 
* Redis 
* Caffeine 
* Simple 
**Spring Boot는 자체적으로 cache manager로 ConcurrentHashMap 를 제공한다.** 
 
## Cache Annotation 
* 스프링의 Cache 서비스 추상화는 AOP를 이용한다. 
    * Advice: 스프링이 제공 
    * Bean과 Method선정: @Cacheable 어노테이션 사용 
 
## CacheManager 
AOP를 이용해 캐시 부가기능을 적용할 수 있게 한다. 동시에 캐시 기술(프로바이더) 상관없이 추상화된 스프링 캐시 API를 이용할 수 잇게 서비스 추상화를 제공한다. 캐시 매니저는 CacheManager 인터페이스를 사용하며, CacheManager 구현 클래스를 제공한다. 
* ConcurrentMapCacheManager   
CocurrentMapCache 클래스를 캐시로 사용하는 캐시 매니저다. 
CocurrentMapCache는 ConcurrentHashMap을 이용하여 간단한 캐시 기능을 구현한 CacheManager이다. 해당 CahceManager의 경우, 캐시 용량 제한, 캐싱 알고리즘, TTL 등의 부가 기능은 없기 떄문에, 테스트 용도로만 사용을 권고한다. 
 
* SimpleCacheManager   
기본적으로 제공하는 캐시가 없다. 따라서 프로퍼티를 이용해서 사용할 캐시를 직접 등록해줘야 한다. 스프링 Cache 인터페이스를 직접 개발했을 때 테스트할 때 사용할 수 있다. 
 
* EhCacheCacheManager   
EhCahce는 캐시 용량 제한, 캐싱 알고리즘, TTL, TTI, 파일 캐싱, 분산 서버 캐싱 등의 고급 기능을 제공하는 Java에서 제일 인기 있는 캐시 프레임워크이다. 해당 설정들은 XML 또는 JavaConfig를 통해 설정할 수 있다. 
  
* CompositeCacheManager   
CompositeCacheManager는 하나 이상의 캐시 매니저를 사용하게 지원해주는 혼합 캐시 매니저이다.  여러 개의 캐시 기술이나 캐시 서비스를 동시에 사용해야 할 때 이용할 수 있다. 
 
* NoOpCacheManager   
캐시가 지원되지 않는 환경에서 동작할 때 캐시 관련 설정을 제거하지 않아도 에러가 나지 않게 해준다. 
 
## Cache 사용법 
1. Enable Caching: Cache를 사용할 것을 Application에 알린다.(`@EableCaching`) 
2. Cache Configurations  
3. Caching declaration: caching을 하고 싶은 Method에 `@Cacheable` 어노테이션을 적용 
 
## Multiple Cache Managers 
:exclamation: 추후작성 :exclamation:
 
## Reference 
* [Multiple Cache Managers](https://www.baeldung.com/spring-multiple-cache-managers)
