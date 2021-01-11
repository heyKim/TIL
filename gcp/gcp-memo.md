  
## GCP
 
### Google Cloud Setup
* 참고: https://testdriven.io/blog/deploying-a-node-app-to-google-cloud-with-kubernetes/
* google-cloud-sdk 설치  
* google-cloud-sdk 설치 확인  
`$ gcloud --version`
* 내 GCP 초기 설정  
`gcloud init`
* 프로젝트 셋팅  
`$ gcloud config set project <PROJECT_ID>`
* kubectl 설치  
`$ gcloud components install kubectl`
* Cluster 생성(GUI로 해도됨)  
```
$ gcloud container clusters create node-kubernetes \
    --num-nodes=3 --zone us-central1-a --machine-type f1-micro
```

### Google Cloud SQL
#### Google Cloud SQL 연결승인 방법
저장소> SQL> SQL 인스턴스 선택> 연결 Tab > 공개IP에 허용할 IP를 추가
* 모든 트래픽 허용: 0.0.0.0/0 추가
* 특정 IP만 허용: [IP] 추가
* Google Kubernetes Engine에서 SQL 연결
 * 비공개 IP 주소를 사용한 연결: 연결 Tab 에서 비공개 IP생성해서 그 IP로 연결하면 된다.
 * 자세한건 여기: https://cloud.google.com/sql/docs/mysql/connect-kubernetes-engine  
 
#### Google Cloud SQL 연결 확인 방법
* 해당 Pod에 들어가서 mysql --host=[host_id] --user=root --password 날려서 
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 1492
Server version: 5.7.14-google-log (Google)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> ectl
```
이런식으로 나오면 됨

* mysql 설치 안되어 있으면 설치
`sudo apt-get update`
`sudo apt-get install mysql-client`



### 인그레스로 HTTP 부하 분산 설정
* 공홈: https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer?hl=ko
* 조대협 블로그: https://bcho.tistory.com/1263

#### 1단계: 웹 애플리케이션 배포
`kubectl run web --image=gcr.io/google-samples/hello-app:1.0 --port=8080`
#### 2단계: 내부적으로 배포를 서비스로 노출
* `kubectl expose deployment web --target-port=8080 --type=NodePort`
이렇게 서비스 리소스를 만들면 컨테이너 클러스터 내에서 web deployment에 접근할 수 있음

* 서비스 배포 확인
`kubectl get service web`
출력:
```
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
web       NodePort   10.35.245.219   <none>        8080:32640/TCP   5m
```
주의할 점은 GKE 노드는 기본적으로 외부에서 액세스할 수 없음. NodePort로 서비스를 만들어도 외부 인터넷에서 이 애플리케이션에 접근할 수 없음.
그래서 Ingress 리소스가 필요한 것!!!!

#### 3단계: 인그레스 리소스 만들기
* Ingress 리소스를 만들기위한 yaml작성
`basic-ingress.yaml` : 
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: basic-ingress
spec:
  backend:
    serviceName: web
    servicePort: 8080
```

* 인그레스 리소스 배포
`kubectl apply -f basic-ingress.yaml`
=> 모든 외부 HTTP 트래픽이 web NodePort 서비스로 라우팅됨

#### 4단계: 애플리케이션 방문
* Ingress의 IP 확인
`kubectl get ingress basic-ingress`
출력:
```
NAME            HOSTS     ADDRESS         PORTS     AGE
basic-ingress   *         203.0.113.12    80        2m
```
* 브라우저에서 접속 확인
브라우저 접속창에 `203.0.113.12`를 입력하여 연결되는지 확인 

#### 5단계: (선택사항) 고정 IP 주소 구성
* 지금 설정한 Ingress IP는 고정 IP가 아님.
* 주의: 인그레스 리소스에 대해 고정 IP를 구성하면 인그레스를 삭제해도 연결된 고정 IP 주소는 삭제되지 않는다!! 구성한 고정 IP 주소를 더 이상 사용하지 않으려면 반드시 삭제해야함.

##### 옵션 1: 기존 임시 IP 주소를 고정 IP 주소로 변환
*  GCP Console> VPC 네트워크> 외부 IP 주소 섹션 들어가서 리스트 중에 ingress 찾아서 임시IP를 고정 IP로 변경
##### 옵션 2: 새 고정 IP 주소 예약
* web-static-ip라는 고정 외부 IP 주소를 예약(생성)
`gcloud compute addresses create web-static-ip --global`
* `ingress.yaml` 매니패스트 내용을 변경
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: basic-ingress
  annotations:
    kubernetes.io/ingress.global-static-ip-name: "web-static-ip"
spec:
  backend:
    serviceName: web
    servicePort: 8080
```
위 소스는 `web-static-ip`라는 고정 IP 리소스를 사용하도록 인그레스에 대한 주석을 추가하는 것임.
* 변경한 인그레스에 적용
`kubectl apply -f ingress.yaml`

#### 그러나 오류남
* 새 고정 IP 주소 예약
```
$ gcloud compute addresses create web-static-ip --global
ERROR: (gcloud.compute.addresses.create) Could not fetch resource:
 - Quota 'STATIC_ADDRESSES' exceeded. Limit: 1.0 globally.
```
=> Limit을 넘었다는...  
* 프로젝트 리소스에 사용할 수 있는 할당량 확인
`$ gcloud compute project-info describe --project key-shine-243509`
=> 출력
(생략)
- limit: 1.0
  metric: STATIC_ADDRESSES
  usage: 1.0
(생략)
=> 결론: STATIC_ADDRESSES 한개 뿐임;
=> 콘솔> IAM 및 관리자> 할당량> 측정항목 중에서 Static IP Addresses 선택 > 계정 업그레이드 하면되는데  
But 무료 평가판 계정을 업그레이드 해야되서 포기;;;;


### Google Cloud Build
* Cloud 빌더는 공용 언어 및 도구가 설치된 컨테이너
* [공홈](https://cloud.google.com/cloud-build/docs/cloud-builders?hl=ko)

### 비공개 GitHub 저장소에 액세스
* https://cloud.google.com/cloud-build/docs/access-private-github-repos?hl=ko
* 키 생성하고 Github에 셋팅 하고 Cloud Build에서 복호화해서 배포....긴 여정...

----------------------
## 배포
### gcloud 에 로그인
`gcloud auth login`
=> OAuth 로 연결되서 Google 로그인 하면됨

### config 설정(프로젝트 ID 설정)
`gcloud config set project secret-bloom-244108`

### 환경변수 설정
`export PROJECT_ID="$(gcloud config get-value project -q)"`

### 이미지 만들기
`docker build -t gcr.io/${PROJECT_ID}/skcc-2016-concert:v1 .`

### 이미지 확인
`docker images`

### 이미지 push
`docker push gcr.io/${PROJECT_ID}/skcc-node-concert:v1`

### Deplyment 배포
* 방법1
`kubectl run concert-deployment --image=gcr.io/${PROJECT_ID}/skcc-node-user:v1 --port 3006`

* 방법2: yaml을 이용한 배포
 * `kubectl apply -f concert-deployment.yaml`
 * concert-deployment.yaml: 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: concert-deployment
spec:
  selector:
    matchLabels:
      app: metrics
      department: skcc
  replicas: 2
  template:
    metadata:
      labels:
        app: metrics
        department: skcc
    spec:
      containers:
      - name: concert
        image: "asia.gcr.io/key-shine-243509/skcc-2016-concert:v1"
        env:
        - name: "PORT"
          value: "3006"
        - name: "database"
          value: "2016"
        - name: "dbusername"
          value: "root"
        - name: "dbhost"
          value: "34.67.8.226"
        - name: "dbpassword"
          value: "skcc"
 ```
 * deployment 배포 확인: `kubectl get pods`  
 
### Service 배포 : 애플리케이션을 인터넷에 노출
* 방법1
 * LoadBalancer로 서비스 배포하는 경우
 `kubectl expose deployment concert-deployment --type=LoadBalancer --port 3005 --target-port 3005`
 * NodePort로 서비스 배포하는 경우
 `kubectl expose deployment concert-deployment --type=NodePort --protocol=TCP --port 3005 --target-port 3005`
 (* kubectl expose : 서비스를 만들어줌)
* 방법2: yaml을 이용한 배포
 * `kubectl apply -f concert-service.yaml`
 * concert-service.yaml:
 ```
 apiVersion: v1
 kind: Service
 metadata:
   name: concert-service
 spec:
   type: NodePort
   selector:
     app: metrics
     department: skcc
   ports:
   - protocol: TCP
     port: 3006
     targetPort: 3006
 ```
 * service 배포 확인: `kubectl get svc`

### (중요!!!) NodePort 로 Service를 배포한 경우 방화벽 규칙 등록해줘야함
* 방화벽 규칙 등록 명령어
 * `gcloud compute firewall-rules create [방화벽 name 지정] --allow tcp:[NodePort로 서비스 만들때 자동 할당된 NodePort]`
 * ex. `gcloud compute firewall-rules create concert-port --allow tcp:31358`
* 방화벽 규칙 삭제 명령어 : `gcloud compute firewall-rules delete concert-port`
* 방화벽 규칙 조회 명령어: `gcloud compute firewall-rules list`

### NodePort로 배포한 서비스 배포 확인
* 클러스터의 노드에 외부IP 주소가 있는 노드 중 하나의 외부 IP주소를 찾는다
`kubectl get nodes --output wide`
* 브라우저 창에 `[VM IP]:[NodePort]` 치고 제대로 동작하는지 확인 

### Ingress 배포
* ingress 배포: kubectl apply -f xxx-ingress.yaml
* ingress 배포 확인: kubectl get ing
* Deployment, Service 모두 잘 배포 되었는데 Ingress가 제대로 동작하지 않은 경우가 있다. GCP Ingress가 그닥 좋지 않은 상태임..

----------------------
##  끝없는 에러.. 참고
#### service 배포시 난 ERROR
`kubectl expose deployment 2016-back-node --type=LoadBalancer --port 3006 --target-port 3006`
=> (출력)
```
ERROR: The Service "2016-back-node" is invalid: metadata.name: Invalid value: "2016-back-node": a DNS-1035 label must consist of lower case alphanumeric characters or '-', start with an alphabetic character, and end with an alphanumeric character (e.g. 'my-name',  or 'abc-123', regex used for validation is '[a-z]([-a-z0-9]*[a-z0-9])?')
```
=> 원인: deployment 이름이 숫자로 시작하면 안됨
=> 해결: deployment 이름이 문자시작으로 변경

#### 고정 IP 예약 ERROR
`$ gcloud compute addresses create web-static-ip --global`
=> (출력)
```
ERROR: (gcloud.compute.addresses.create) Could not fetch resource:
 - Quota 'STATIC_ADDRESSES' exceeded. Limit: 1.0 globally.
```
=> 원인: 고정IP Address 할당을 위한 Resource가 부족
=> 해결  
 * 프로젝트 리소스에 사용할 수 있는 할당량 확인
 `$ gcloud compute project-info describe --project key-shine-243509`
 => 출력
 (생략)
 - limit: 1.0
   metric: STATIC_ADDRESSES
   usage: 1.0
 (생략)  
=> 결론: STATIC_ADDRESSES 한개 뿐임  
=> 콘솔> IAM 및 관리자> 할당량> 측정항목 중에서 Static IP Addresses 선택 > 계정 업그레이드
But 무료 평가판 계정을 업그레이드 해야되서 포기ㅎ
