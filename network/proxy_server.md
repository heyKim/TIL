# Proxy Server

* Proxy Server란?
* Proxy Server를 왜 쓸까?
* Proxy Server 종류
    * Forward Proxy
    * Reverse Proxy 
* Proxy 서버의 동작 방식
* Squid Proxy Server
-----
## Proxy Server란?
* proxy는 사전적으로 '대리'라는 뜻을 지님
* 클라이언트를 대신해서 간접적으로 인터넷에 접속할 수 있는 중계기 역할을 하는 서버이다.

## Proxy Server를 왜 쓸까?
* 일부 네트워크를 막기 위해서
* 캐시를 사용해 리소스 접근을 빠르게 하기 위해서
* 사용 기록 확인, 네트워크 서비스나 콘텐츠로의 접근 정책을 적용하기 위해. (이를테면 원치 않는 사이트를 차단),  사용률을 기록하고 검사하기 위해(보안, ACL(Access Control List), Log / Audit 등을 위해 사용)
* IP 추적을 안당하려고(내 PC에서 proxy 서버 거쳐서 요청이 가면 서버 입장에서 요청자 IP는 proxy server IP가 됨, 그러나 고급기술(?) 쓰면 IP 역추적 가능하긴 함)
* 밖으로 나가거나, 안으로 들어오는 컨텐츠를 검사하기 위해서
* 지역 우회(proxy 서버를 외국에 두고 접속하면 접속 지역을 외국으로 인식함)

## Proxy Server 종류
* Forward Proxy: 클라이언트의 IP 주소를 숨김. Client에서 Proxy 서버에 Request 요청
    * 주로 캐싱을 위해 사용
* Reverse Proxy: 서버의 IP 주소를 숨김. 서버의 Response를 Proxy 서버를 통해서 전달
    * 주로 보안때문에 사용

## Proxy 서버의 동작 방식
* Transparent Proxy
    * "나 Proxy Server고, 너한테 요청하는 애 IP도 줄게"
    * HTTP 헤더의 VIA, X_FORWARD_FOR 영역에 원래 IP를 전달, 즉 받은 요청 그대로 전달함.
    * 사용예) 스타벅스 Wifi 접근할 때, 별도의 페이지가 뜨는데 이건 접속 요청을 특정 페이지로 보내고 사용하도록 만든 것
* Simple Annonymous Proxy
    * "나 Proxy Server예요~내 IP 받아요~"라는 것
    * HTTP 헤더의 HTTP_VIA, HTTP_X_FORWARD_FOR 영역에 자신의 IP를 박아서 자신이 프록시임을 알린다. 원래 IP는 제공되지 않는다.
* Distorting Proxy
    * HTTP 헤더의 HTTP_VIA 영역에는 자신의 IP, HTTP_X_FORWARD_FOR 영역에는 랜덤으로 생성된 값을 박아서 자신이 프록시임을 알린다. 원래 IP는 제공되지 않는다.
* High Annonymity Proxy(또는 Elite)
    * "아무 단서도 남기지 않는다..." -> 역추적 불가능
    * 프록시를 사용하지 않는 것과 동일하게 동작한다. 이건 아예 프록시인지 조차도 알 수 없다.  

*(여담)public cloud에 Legacy 시스템을 올리는 프로젝트에서 한 시스템이 사용자 IP를 판단하여 서비스하는 로직을 가지고 있었다. 근데 Cloud에 올리면서 네트워크 아키텍처가 바껴서 HTTP_X_FORWARD_FOR에 사용자 IP가 아닌 다른 IP(LB였나)가 찍히는 문제가 있었음.*

## Squid Proxy Server
* 프로시 서버이자 웹캐시
* 폐쇄망 안의 서버를 업데이트 하거나 파일을 다운로드 하기 위해서(내가 이걸 정리하는 이유)
    * 폐쇄망 안의 서버에 npm package를 다운받기 위해서

### Squid 설치
* linux: `yum -y install squid`
* mac: `brew install squid`

### 스퀴드(Squid) 구성
* squid/bin : squid 실행과 관련된 스크립트 파일이 있는 디렉토리
* squid/etc : squid 관련 환경 설정 파일인 squid.conf 가 있는 디렉토리
* squid/libexec : 서버운영과 관련된 스크립트 파일이 있는 디렉토리
* squid/lib : 프로그램 개발에 필요한 라이브러리 정보들이 있는 디렉토리
* squid/man : man 명령 관련 정보들이 있는 디렉토리
* squid/sbin : squid 서버를 실행하고 관리하는 squid 라는 명령이 위치하는 디렉토리
* squid/share : squid 를 사용하면서 발생하는 에러 메시지에 대한 정보들이 있는 디렉토리
* squid/var : 로그 파일에 대한 정보들이 있는 디렉토리

## Reference 
* [스퀴드(Squid)로 프록시 서버 구축하기](!https://github.com/Yongdae-Kim/HowToUseSquid)
* [GCP에서 Squid를 이용한 Proxy 서버 설정 방법](!https://kibua20.tistory.com/129)