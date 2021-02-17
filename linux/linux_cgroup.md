# cgroup
* control group을 줄여서 cgroup이라는 약칭을 사용
* 단일 또는 태스크 단위의 프로세스 그룹에 대한 자원 할당을 제어하는 커널 모듈이다.
* (참고: 리눅스의 프로세스는 하나 이상의 스레드의 모음으로 동작한다.)
* 즉, cgroup을 사용하여 시스템 관리자는 시스템 자원 할당, 우선 순위 지정, 거부, 관리, 모니터링과 같은 세밀한 제어가 가능하다.
* cgroup으로 컨테이너 안의 프로세스에 대해 자원을 제한 함으로써 특정 컨테이너가 호스트OS의 자원을 모두 사용하는 일을 방지한다.


## cgroup 서브 시스템
서브시스템은 resource controller로서 cgroup당 자원을 관리하는 모듈이다.
### cgroup 서브 시스템 종류
* blkio: 블록 디바이스 입출력량 제어
    * 물리 드라이브 (예: 디스크, 솔리드 스테이트, USB 등)와 같은 블록 장치에 대한 입력/출력 액세스에 제한을 설정
* cpu: cpu 사용량 제한 
    * CPU에 cgroup 작업 액세스를 제공하기 위해 스케줄러를 사용
* cpuacct: CPU 사용량 통계
    * cgroup의 작업에 사용된 CPU 자원에 대한 보고서를 자동으로 생성
* cpuset: CPU나 메모리 배치를 제어
    * 개별 CPU (멀티코어 시스템에서) 및 메모리 노드를 cgroup의 작업에 할당
* devices: 디바이스 엑세스 허가/거부
    * cgroup의 작업 단위로 장치에 대한 액세스를 허용하거나 거부
* freezer: 그룹에 속한 프로세스 정지/재개
    * cgroup의 작업을 일시 중지하거나 다시 시작합니다.
* memory: 메모리 사용량 제한
    * cgroup의 작업에서 사용되는 메모리에 대한 제한을 설정하고 이러한 작업에서 사용되는 메모리 자원에 대한 보고서를 자동으로 생성
* net_cls: 네트워크 제어 태그 지정
    * 이 서브시스템은 Linux 트래픽 컨트롤러 (tc)가 특정 cgroup 작업에서 발생하는 패킷을 식별하게 하는 클래식 식별자 (classid)를 사용하여 네트워크 패킷에 태그를 지정
* ns: namespace 서브시스템

## cgroup 사용방법
cgroup은 두 가지 방법으로 사용할 수 있다.
1. cgroup 가상파일시스템을 마운트하여 파일 및 디렉토리를 조작하는 방법
2. 사용자 도구 사용 (Debian, ubuntu는 cgroup-bin / RHEL, CentOS는 libcgroup)

## Reference
https://torch.vision/2019/06/18/cgroups-1.html