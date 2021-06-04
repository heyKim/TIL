# Cloud Modernization

## Application Modernization이란
기존의 Application을 모던한/최신의 Technology로 새로운 Application으로 바꾸는 작업

### Application Modernization이 필요한 이유
오늘날 빠른 시장 속도와 고객 변화에 응대하기 위해서 빠르고 빈번하게 어플리케이션을 개발하고 배포해야한다.
오늘날의 Application은 수백만명의 사용자의 요청에 신속하게(밀리초 단위) response를 줘야하고, 어플리케이션이 빠르게 확장되고 가용성을 보장하며, 페타바이트급 데이터를 관리해야한다.
이런 요구사항을 만족하기 위해서 새로운 아키텍처, 조직문화의 변화, 새로운 프로세스 수립을 통해 어플리케이션을 Modernization 해야한다.
#### Modernization Triggering
Modernization Triggering하는 Business Requirement들이 있다.
예) Datacenter contracts expiring, Deliver applications and feature faster, Urgent capacitiy needs, software or hardware refresh, enable new biz opportunity, software EOS
#### 주요 Requirement에  대한 해결책
* 각종 이벤트 시에 사용자 증가에 따른 부하 분산 필요 => 클라우드, 컨테이너 도입
* 하드웨어 및 소프트웨어 EOS, 노후화되어 안정성뿐 아니라 신기술 적용 어려움 => 최신 아키텍처 도입 (MSA, Scalable Database : Master-Replica 구조 등), 오픈 소스 소프트웨어 도입
* 빠른 Delivery => CI/CD 도입

### Application Modernization 3가지 디자인 패턴
* 마이크로서비스 아키텍처 : 서비스들이 네트워크를 통해 서로 API로 통신 / 서비스는 독자적으로 업데이트하며, 서로 영향을 주지 않음 / 다른서비스의 내부 구조를 알지 못해도 내 서비스 코드를 업데이트 할 수 있음. 
* 서버리스 기반 운영 모델 : 확장성 가용성 제공, 장애 복원력, 실행 시간만 과금, 관리/운영 불필요, 비즈니스 로직만 집중, 클라우드 네이티브 서비스 활용
* 데브옵스 자동화 배포 방식 : 소스 코드 저장소부터 패키지 빌드, 테스트, 배포 및 모니터링까지 전 과정에 대해 자동화! 