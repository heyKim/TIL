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
