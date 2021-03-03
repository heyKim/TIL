# Spring Cache 
* [Cache란](#cache란)
* [Cache를 왜 사용할까?](#cache를-왜-사용할까)
* [Long Tail 법칙](#long-tail-법칙)
* [Local Cache vs Global Cache](#local-cache-vs-global-cache)
* [Spring Boot Cache](#spring-boot-cache)
* [Spring Boot 공식지원 Third-Party Cache](#spring-boot-공식지원-third-party-cache)
* [Cache 사용법](#cache-사용법)
* Multiple Cache Managers
* [Cache 전략](#cache-전략)
 
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

## Cache 사용 시 주의사항

## Long Tail 법칙
Long Tail 법칙은 20%의 요구가 시스템 리소스의 대부분을 사용한다는 법칙이다. 그렇기 때문에 20%의 기능에 Cache를 이용함으로써 리소스 사용량은 대폭 줄이고, 성능은 대폭 향상시킬 수 있다.

## Local Cache vs Global Cache
* Local Cache
  * Local 장비 내에서만 사용 되는 캐시
  * Local 장비의 Resource를 이용한다 (Memory, Disk)
  * Local에서 작동 되기 때문에 속도가 빠르다.
  * Local에서만 작동되기 때문에 다른 서버와 데이터 공유가 어렵다
* Global Cache
  * 여러 서버에서 Cache Server에 접근하여 사용하는 캐시
  * 데이터를 분산하여 저장 할 수 있다.
    * Replication - 데이터를 복제
    * Sharding - 데이터를 분산하여 저장
  * Local Cache에 비해 상대적으로 느리다 (네트워크 트래픽)
  * 별도의 Cache Server를 이용하기 때문에 서버 간 데이터 공유가 쉽다.
  * 캐시에 저장된 데이터가 변경되는 경우:
    * 해당 서버를 제외한 모든 peer에 변경 사항 전달
    * All-to-All Replication
    * WAS 인스턴스가 늘어나고, 캐시 저장 데이터 크기가 커지면 성능이 저하되는 이유는 이 때문
 
## Spring Boot Cache 
* 스프링 3.1 이상부터는 캐시 추상화 기술을 제공한다. 
* Spring은 Third-Party Cache 구현체를 위한 Layer 만 제공한다. 개발자는 Third-Party Cache 구현체를 골라서 쓰기만 하면됨. 나중에 변경사항이 생겨서 바꾸기도 쉬움 
* 설정과 어노테이션 추가만으로 캐시를 쉽게 적용할 수 있음. 
 
## Spring Boot 공식지원 Third-Party Cache 
* Generic 
* JCache (JSR-107 aka javax.cache API) (JCache 구현체인 EhCache 3, Hazelcast, Infinispan, and * others) 
* EhCache 2.x 
* Hazelcast 
* Infinispan 
* Couchbase 
* Redis 
* Caffeine 
* Simple 
**Spring Boot는 자체적으로 cache manager로 ConcurrentHashMap 를 제공한다.** 
 
## Cache 사용법 
1. Enable Caching: Cache를 사용할 것을 Application에 알린다.(`@EableCaching`) 
2. Cache Configurations  
3. Caching declaration: caching을 하고 싶은 Method에 `@Cacheable` 어노테이션을 적용 

### Cache Annotation 
* 스프링의 Cache 서비스 추상화는 AOP를 이용한다. 
    * Advice: 스프링이 제공 
    * Bean과 Method선정: @Cacheable 어노테이션 사용 
 
### CacheManager 
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
## Multiple Cache Managers 
:exclamation: 추후작성 :exclamation:

## Cache 전략
* 장애대응 전략
  * 부하가 많이 걸린다면? 2차 캐시 구성을 고려하자.(Local Cache, Global Cache 구성)
  * Global Cache에서 장애가 발생할 경우를 대비, Circuit-Breaker pattern(Hystrix)를 고려
* Cache-aside pattern
  * 어플리케이션에서 데이터 read 요청이 오면, 먼저 cache에서 찾는다.
  * 캐시에 데이터가 있으면 return, 없으면 data store(DB)에서 찾아서 application에 돌려주고 cache를 업데이트 한다.
  * data store에 데이터 변경이 있으면, cache도 업데이트 해준다.
 
## Reference 
* [Multiple Cache Managers](https://www.baeldung.com/spring-multiple-cache-managers)
* [Local Cache vs Global Cache](https://jaehun2841.github.io/2018/11/07/2018-10-03-spring-ehcache/#long-tail-%EB%B2%95%EC%B9%99)
* [무신사 watcher 로컬 캐시 동기화 문제](https://github.com/JunHyeok96/Musinsa-Watcher/issues/103)
* [무신사 watcher 캐시 서버에 장애가 생긴다면?](https://jgrammer.tistory.com/entry/%EB%AC%B4%EC%8B%A0%EC%82%AC-watcher-%EC%BA%90%EC%8B%9C-%EC%84%9C%EB%B2%84%EC%97%90-%EC%9E%A5%EC%95%A0%EA%B0%80-%EC%83%9D%EA%B8%B4%EB%8B%A4%EB%A9%B4)
* [Spring Cache 장애 대응 방안](https://supawer0728.github.io/2018/04/18/spring-cache-fallback/)
* [Azure Caching guide](https://docs.microsoft.com/en-us/azure/architecture/best-practices/caching)
