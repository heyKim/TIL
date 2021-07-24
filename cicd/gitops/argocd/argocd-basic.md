# ArgoCD

## ArgoCD란?

## ArgoCD 실습
![ArgoCD GitOps CD](/cicd/assets/argocd-flow.png)

### ArgoCD 설치
* helm repo 추가
`helm repo add argo https://argoproj.github.io/argo-helm`
* config 수정을 위해서 source 다운로드 및 소스 수정 후 argocd 설치
```bash
# source 다운로드
helm fetch argo/argo-cd

# source 수정
# argocd 서버를 외부에서 접속하기 위해서 Service Type만 ClusterIP에서 LoadBalancer로 변경해줌

# helm install 하여 argocd 설치
helm install "argo cd이름 지정" argo/argo-cd

# 설치 시 수정한 values.yaml을 적용하고 싶을 때
helm install "argo cd이름 지정" argo/argo-cd -f ./values.yaml
```
* 설치 후 패스워드 확인
```bash
# secret 에서 패스워드 확인(id는 admin이 default)
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# `argocd-initial-admin-secret`는 password 변경 후에 namespace를 지워야한다고 함.(공홈 피셜)
```

### ArgoCD 설정
#### 둘러보기
![ArgoCD 메인화면](/cicd/assets/argocd-main.png)
ArgoCD는 `Application`라는 CRD(Custom Resource Definition)를 제공한다. Application은 Kubernetes resources 묶음으로 배포를 책임진다.
#### Application 생성
![ArgoCD App생성1](/cicd/assets/argocd-application-create-general.png)
![ArgoCD App생성2](/cicd/assets/argocd-application-create-source-destination.png)
![ArgoCD App생성3](/cicd/assets/argocd-application-create-kustomize.png)
* Application Name: App의 이름을 적는다.
* Project: 프로젝트를 선택하는 필드. 쿠버네티스의 namespace와 비슷한 개념으로 여러 App을 논리적인 project로 구분하여 관리할 수 있다.
* Sync Policy: Git 저장소의 변경 사항을 어떻게 sync할지 결정. Auto는 자동으로 Git 저장소의 변경사항을 운영에 반영하고 Manual은 사용자가 버튼 클릭 혹은 API를 통해 직접 운영 반영을 해야함.
* Repository URL: ArgoCD가 바라볼 Git 저장소를 의미
* Revision: Git의 어떤 revision (HEAD, master branch 등)을 바라 볼지 결정
* Path: Git 저장소에서 어떤 디렉토리를 바라 볼지 결정함. (dot(.)인 경우 root path를, 디렉토리 이름을 적으면 해당 디렉토리의 배포 정의서만 tracking 함)
* Cluster: 쿠버네티스의 어느 클러스터에 배포할지를 결정
* Namespace: 쿠버네티스 클러스터의 어느 네임스페이스에 배포할지를 결정
* Kubernetes manifests Tool 선택: Kubernetes manifest를 정의할 tool 을 선택하고 설정 정보를 입력한다.

![ArgoCD App생성4](/cicd/assets/argocd-application-after-creat.png)
ArgoCD Application 생성 후, 동기화 이전이라 `OutOfSync`상태
![ArgoCD App생성5](/cicd/assets/argocd-application-deploy-change.png)
config repo에서 Deployment의 namespace을 변경하니 이를 감지하여 동기화가 필요한 상태임을 표시해줌
#### ArgoCD Image Updater
* Argo CD로 관리되는 k8s workload의 container image를 자동으로 업데이트 해주는 기능. 
* resouce yaml에 image 버전 등을 일일이 업데이트 할 필요없이 새로운 이미지의 업데이트를 자동으로 감지하여 k8s 워크로드에 적용해준다.
* Argo CD가 관리하는 Application 리소스에 annotation을 부여하여 설정한다.
> **_NOTE:_**  아직 개발 중인 기능이다.
##### ArgoCD Image Updater 설정
1. argocd-image-updater 설치
```bash
# argocd 가 설치된 k8s workload에 설치
git clone https://github.com/argoproj-labs/argocd-image-updater/blob/master/manifests/install.yaml

# Apply the installation manifests¶
kubectl apply -n argocd -f manifests/install.yaml
```
2. Docker Registry에 대한 Access 권한 부여
3. ArgoCD Application에 annotation 추가
Auto Image Updater 설정을 하기 위해서는 annoatation을 추가 해주어야한다.
아래 command 로 annoation 추가해도 되고
```bash
kubectl annotate app argocd-updater-test -n argocd\
    argocd-image-updater.argoproj.io/image-list=registry-1.docker.io/jkk3366/jenkins \
    argocd-image-updater.argoproj.io/todo.force-update=true \
    argocd-image-updater.argoproj.io/write-back-method=git
```
ArgoCD UI에서 아래 annoation을 추가해주어도 된다.
```
argocd-image-updater.argoproj.io/image-list: jkk3366/jenkins
argocd-image-updater.argoproj.io/todo.force-update: true
argocd-image-updater.argoproj.io/write-back-method: git
```

## 참고
* [Argo CD Install Official](https://argoproj.github.io/argo-cd/getting_started/#1-install-argo-cd)
* [Argo CD Image Updater Official](https://argocd-image-updater.readthedocs.io/en/stable/)
* [Argo CD Image Updater 예제](https://zenn.dev/nekoshita/articles/02c1e59a487fb4)
