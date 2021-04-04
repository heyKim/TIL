# Kubernetes Object
Kubernetes에서 가장 중요한 부분이 Object
Object에는 basic object와 controller가 있다.

## Basic Object
Object는 k8s의 상태를 나타내는 엔티티
- Pod, Service, Volume, Namespace 등이 있음
- 각각의 Object는 Spec과 Status라는 필드를 갖게 됨
- Spec은 사용자가 기대하는 상태(Desired State)
- Status는 기대되는 값에 대비한 현재의 상태

### Pod
컨테이너를 포함하고 있고, k8s가 상태유지를 위해 사용하는 가장 작은 배포단위
#### Pod 특징
- 1개의 Pod에 여러 개의 Container 가능
- 1개의 Pod는 여러 개의 물리서버에 나눠지는 것이 아니고 1개의 물리서버(Node)위에서 실행됨
- 동일 작업을 수행하는 Pod는 ReplicaSet이라는 컨트롤러에 의해 복제생성 된다.
- Pod는 클러스터 내에서 사용할 수 있는 유니크한 네트워크 ip를 할당 받지만 이 ip는 서비스에서 사용하지 않음
- Pod에 속한 컨테이너는 스토리지와 네트워크를 공유하고 서로 localhost로 접근할 수 있다.
- Pod는 언제가는 반드시 죽는(mortal) 오브젝트 이다.

### Service
* ReplicaSet의 LoadBalancer
* Pod의 IP는 Pod가 삭제되고 새로 생성될 때마다 바뀜. Pod의 엔드포인트를 더 잘 관리하기 위해 Pod 앞단에 Service를 둠
* 앞단의 Service 타입에 따라 Pod와 클러스터 외부를 어떤 네트워크 서비스로 연결할지 결정함
    * Service 타입
        * ClusterIp: 클러스터 내에서는 접근이 가능하지만, 클러스터 외부에서는 접근이 불가능하다.
        * NodePort
            * 클러스터 내에서 <내부IP>:<포트>으로도 접속 가능하고, 외부에서 `<NodeIP>:<NodePort>` 로도 접근 가능하다.
            * 모든 Node의 포트가 열리고, 해당 서비스로 연결된다. 30000 ~ 32767 까지 사용 가능
            * 노드의 IP가 바뀔 수 있는 부분을 처리해줘야함. 보통 ingress랑 같이 씀
        * LoadBalancer
            * 보통 클라우드 서비스에서만 설정 가능한 방식으로, 외부 IP를 가지고 있는 로드밸런서를 할당한다
            * 외부 IP를 가지고 있기 때문에 외부에서 접근이 가능하다
            * 클라우드 서비스 내에서 외부 IP가 할당된 로드밸런서를 생성하고 서비스에 연결시키는 구조
* 그리고 보통 Pod를 여러 개 띄우는데 이 Pod들을 묶어 LB역할을 해줌
* Pod를 외부로부터 어떻게 네트워크 서비스를 연결할지 결정함
### Volume
k8s는 기본적으로 Pod를 Stateless(상태가 없는)로 유지하고 싶어한다.
하지만 서비스를 하다보면 Pod안의 데이터를 영구적으로 저장할 필요가 있는데 이때 사용하는 것이 Volume
### Namespace
* 쿠버네티스 클러스터내의 논리적인 분리단위이다
* 네임스페이스별로 리소스들을 나눠서 관리할 수 있고, 접근권한, 리소스 할당량 등을 설정할 수 있다

## Kubernetes Controller(Workloads)
* Object의 spec에 정의된 상태로 지속적으로 변화시키는 주체
* 간단히 말해서 Object를 더 잘 쓰기 위해서 Object를 생성하고 관리함
* ReplicaSet, Deployment, StatefulSets, DaemonSet, CronJob 등이 있음

### ReplicaSet
* 지정한 숫자로 Pod의 개수를 관리하는 Controller

### Deployment
* ReplicaSet 보다 상위에 있는 개념으로 ReplicaSet 배포의 기본 단위가 되는 리소스
* 쿠버네티스는 이 Deployment를 단위로 애플리케이션을 배포한다
* ReplicaSet을 직접 다루기보다는 Deployment를 통해 배포하는 경우가 대부분이다.
* Deployment를 통해 Pod를 더 세밀하게 배포
    * 배포하다가 롤백 가능(Deployment는 리비전 관리하기 때문에)
    * 블루-그린 배포도 가능