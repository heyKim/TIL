# Volume 이란? Pod에 종속되는 디스크 
Container에 저장된 데이터는 휘발성! Container가 죽으면 그 데이터도 날아감. 
그래서 Container에 저장된 데이터를 영구적으로 저장하거나 다른 여러 Container와 데이터를 공유하기 위해서 Volume이라는 디스크 서비스를 사용한다! 
 
 
 
## Volume 종류 
* emptyDir (Pod 내 공유) 
  * Pod가 생성될 때 생성되고, Pod가 삭제 될 때 같이 삭제되는 임시 볼륨이다. 
* hostPath (Pod 외부 공유) 
  * 노드의 로컬 디스크의 경로를 Pod에서 마운트해서 사용한다. 
  * 같은 Node의 Pod 간에만 공유 가능 
  * Pod가 각각 다른 노드에 올라오면 데이터 공유 불가 
  * nodeSelector 등을 통해 Pod가 배포될 Node를 지정해주면 항상 같은 Node에 배포되게 가능 
  * DC/OS로 배포 되는 서비스는 hostPath Volume 타입만 제공 
* CIFS

## PV(Persistent Volumes)과 PVC(Persistent Volume Claims) 
위에 Volume을 개발자가 사용하려면 Cluster에서 접근 가능한 Network Storage Infra에 대한 사전 지식이 요구되고 Server IP나 공유 폴더, Volume ID, name등이 노출되는 약점이 있음. 
그래서 k8s는 인프라에 대한 복잡성을 추상화 시키고 개발자들이 손쉽게 인프라 자원을 사용할 수 있도록 PV와 PVC라는 Object Resource를 사용! 
 
클라우드 관리자는 실제 스토리지를 생성하고, 이 디스크의 일부 용량을 할당한 PV를 Kubernetes에 등록 
개발자는 Pod를 생성할 때, Volume을 정의하고 이 부분에 PVC를 지정하여 시스템 관리자가 생성한 PV를 연결 
 
 
### PV Provisioning 
1) 정적 프로비저닝 
정적 프로비저닝은 매니페스트 파일 등을 통해 특정 용량을 가진 PV를 미리 생성해두고, 요청이 있을 시 미리 생성한 PV을 할당하여 사용 
2) 동적 프로비저닝 
정적과는 다르게 사용자가 요청할 때 PV를 생성하여 할당하고, 사용자는 원하는 만큼의 용량을 생성해서 자유롭게 사용.
기존에는 관리자가 PV를 개발자로부터 요청을 받을 때마다 생성해주었음. 그런데 k8s에서는 이 작업을 StorageClass를 통해 자동으로 수행
즉, 누군가(Provisioner)가 알아서(Dynamic) 볼륨을 만들어서 붙여주는(Provisioning) 하는 방법 
클라우드 관리자는 PV를 여러 개 만들어 두는 대신에 Profile(또는 MetaClass)과도 같은 StorageClass를 정의하여 사용자가 이 StorageClass 사용하게 한다.
개발자가 PVC를 생성할 때 PV name이 아닌 관리자가 만든 storageClassName을 지정하면 PVC에 연결이 되고, 스토리지 클래스에 정해진 스펙에 따라서 물리 디스크와 PV를 생성하게 된다. 
 

### StorageClass Reclaiming Policy 
PV는 기존에 사용했던 PVC가 아니더라도 다른 PVC로 재활용이 가능함. 사용이 종료된 PVC를 삭제할 때, 사용했던 PV의 데이터를 어떻게 처리할 지에 대한 설정을 지정해주어야 함. 
Retain : PV의 데이터를 그대로 보존 
Recycle : 재사용하게 될 경우 기존의 PV 데이터들을 모두 삭제 후 재사용 가능 
Delete : 사용이 종료되면 해당 볼륨을 삭제(Default) 

### PVC생성
```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce # 하나의 Pod에만 마운트되고 하나의 Pod에서만 읽고 쓰기가 가능
  resources:
    requests:
      storage: 3Gi # 최소 3기가바이트의 볼륨이 필요해요!
```
* Access Mode 
  * ReadWriteOnce(RWO): 하나의 Pod에만 마운트되고 하나의 Pod에서만 읽고 쓰기가 가능
  * ReadOnlyMany(ROX): 여러 개의 Pod에 마운트하며, 여러 개의 Pod에서 동시에 읽기가 가능. 쓰기는 불가능
  * ReadWriteMany(RWX): 여러 개의 Pod에 마운트가 가능하고, 동시에 여러 개의 Pod에서 읽기/쓰기 가능

#### PV 정적 프로비저닝 예제
