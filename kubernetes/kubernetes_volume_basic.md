Volume 이란? Pod에 종속되는 디스크 
 
Container에 저장된 데이터는 휘발성! Container가 죽으면 그 데이터도 날아감. 
 
그래서 Container에 저장된 데이터를 영구적으로 저장하거나 다른 여러 Container와 데이터를 공유하기 위해서 Volume이라는 디스크 서비스를 사용한다! 
 
 
 
Volume 종류 
 
emptyDir (Pod 내 공유) 
Pod가 생성될 때 생성되고, Pod가 삭제 될 때 같이 삭제되는 임시 볼륨이다. 
hostPath (Pod 외부 공유) 
노드의 로컬 디스크의 경로를 Pod에서 마운트해서 사용한다. 
같은 Node의 Pod 간에만 공유 가능 
Pod가 각각 다른 노드에 올라오면 데이터 공유 불가 
nodeSelector 등을 통해 Pod가 배포될 Node를 지정해주면 항상 같은 Node에 배포되게 가능 
DC/OS로 배포 되는 서비스는 hostPath Volume 타입만 제공 
 
CIFS 
PV(Persistent Volumes)과 PVC(Persistent Volume Claims) 
 
위에 Volume을 개발자가 사용하려면 Cluster에서 접근 가능한 Network Storage Infra에 대한 사전 지식이 요구되고 Server IP나 공유 폴더, Volume ID, name등이 노출되는 약점이 있음. 
 
 
 
그래서 k8s는 인프라에 대한 복잡성을 추상화 시키고 개발자들이 손쉽게 인프라 자원을 사용할 수 있도록 PV와 PVC라는 Object Resource를 사용! 
 
 
 
클라우드 관리자는 실제 스토리지를 생성하고, 이 디스크의 일부 용량을 할당한 PV를 Kubernetes에 등록 
 
개발자는 Pod를 생성할 때, Volume을 정의하고 이 부분에 PVC를 지정하여 시스템 관리자가 생성한 PV를 연결 
 
 
 
PV Provisioning 
 
1) 정적 프로비저닝 
 
정적 프로비저닝은 매니페스트 파일 등을 통해 특정 용량을 가진 PV를 미리 생성해두고, 요청이 있을 시 미리 생성한 PV을 할당하여 사용 
 
2) 동적 프로비저닝 
 
정적과는 다르게 사용자가 요청할 때 PV를 생성하여 할당하고, 사용자는 원하는 만큼의 용량을 생성해서 자유롭게 사용. 
 
즉, 누군가(Provisioner)가 알아서(Dynamic) 볼륨을 만들어서 붙여주는(Provisioning) 하는 방법 
 
이때 사용 하는 것이 StorageClass 라는 API Object! 
 
 
 
클라우드 관리자가 StorageClass를 만들어 두고 
 
개발자가 PVC를 생성할 때 PV name이 아닌 관리자가 만든 storageClassName을 지정하면 PVC에 연결이 되고, 스토리지 클래스에 정해진 스펙에 따라서 물리 디스크와 PV를 생성하게 된다. 
 
 
 
PV Reclaiming Policy 
 
PV는 기존에 사용했던 PVC가 아니더라도 다른 PVC로 재활용이 가능함. 사용이 종료된 PVC를 삭제할 때, 사용했던 PV의 데이터를 어떻게 처리할 지에 대한 설정을 지정해주어야 함. 
 
Retain : PV의 데이터를 그대로 보존 
Recycle : 재사용하게 될 경우 기존의 PV 데이터들을 모두 삭제 후 재사용 가능 
Delete : 사용이 종료되면 해당 볼륨을 삭제(Default) 
 
 
​PVC생성 
 
​Access Mode 
​ReadWriteOnce(RWO): 
ReadOnlyMany(ROX) 
ReadWriteMany(RWX) 

https://nirsa.tistory.com/157
# Volume
Container내 디스크에 있는 파일은 임시적이다. 컨테이너가 죽으면 해당 파일들도 다 날라간다.
그래서 볼륨이라는 개념이 나왔다.
Volume은 Pod에 종속되는 디스크이다. Pod 단위 이기 때문에 같은 Pod안에 속한 컨테이너들은 공유해서 사용가능하다.

## Volume 종류
* emptyDir
* hostPath
* azureDisk, azureFile
* NFS
* PVC

### emptyDir
* Pod 과 함께 생성되고, 삭제되는 임시 Volume 
```yml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  # redis 컨테이너의 /appdata/log 디렉토리에 appdata-volume를 마운트
  - image: redis
    name: redis
    volumeMounts:
    - mountPath: /appdata/log
      name: appdata-volume
  # nginx 컨테이너의 /appdata/log 디렉토리에 appdata-volume를 마운트
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /appdata/log
      name: appdata-volume
  # appdata-volume이라는 이름으로 emptyDir 기반의 볼륨생성(in Pod)
  volumes:
    - name: appdata-volume
      emptyDir: {}
```

* (참고) mount 개념
    * 기차는 서울역에서 정차한다. (=디스크를 디렉토리에 마운팅)
    * 홍길동은 서울역에서 기차를 탄다. (=데이터를 마운트 디렉토리에 저장)
    * 기차는 홍길동을 태우고 목적지까지 간다. (=실제 물리적으로 디스크에 저장됨)

### hostPath
* 노드의 로컬 디스크의 경로를 Pod에 마운트 한다.
* 같은 노드의 Pod들은 hostPath 타입 볼륨을 공유할 수 있다.
* Docker 에서 `-v`옵션과 동일

### azureDisk, azureFile
