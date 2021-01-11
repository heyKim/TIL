# Kubernetes Object
Object는 k8s의 상태를 나타내는 엔티티
- Pod, Service, Volume, Namespace 등이 있음
- 각각의 Object는 Spec과 Status라는 필드를 갖게 됨
- Spec은 사용자가 기대하는 상태(Desired State)
- Status는 기대되는 값에 대비한 현재의 상태

## Pod
k8s가 상태유지를 위해 사용하는 가장 작은 배포단위

### Pod 특징
- 1개의 Pod에 여러 개의 Container 가능
- 1개의 Pod는 여러 개의 물리서버에 나눠지는 것이 아니고 1개의 물리서버(Node)위에서 실행됨
- 동일 작업을 수행하는 Pod는 ReplicaSet이라는 컨트롤러에 의해 복제생성 된다.
- Pod는 클러스터 내에서 사용할 수 있는 유니크한 네트워크 ip를 할당 받지만 이 ip는 서비스에서 사용하지 않음
- Pod에 속한 컨테이너는 스토리지와 네트워크를 공유하고 서로 localhost로 접근할 수 있다.
- Pod는 언제가는 반드시 죽는(mortal) 오브젝트 이다.

## ReplicaSet


# Kubernetes Controller(Workloads)
Object의 spec에 정의된 상태로 지속적으로 변화시키는 주체
- ReplicaSet, Deployment, StatefulSets, DaemonSet, CronJob 등이 있음
