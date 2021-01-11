# Cluster Access를 위한 kubeconfig file 설정
* kubeconfig file은 clusters, users, namespaces와 같은 cluster connection information과 authentication 매커니즘 내용으로 구성된다.
* `kubectl` cli은 kubeconfig file을 통해 cluster를 선택하고 cluster의 API Server와 communicate한다.
* kubeconfig file를 이용하면 multiple cluster와 users에 authentication switching이 쉽다.
```yaml
preferences: null
apiVersion: v1
kind: Config
current-context: zcp-context
contexts: 
  - name: zcp-context
    context: 
      cluster: zcp-cluster
      user: mgmtsv-admin@sk.com
      namespace: submodule-template
clusters: 
  - name: zcp-cluster
    cluster: 
      server: https://c4.seo01.containers.cloud.ibm.com:23451
      certificate-authority-data: [DATA]
  - cluster:
    insecure-skip-tls-verify: true
    server: https://pig.org:443
  name: pig-cluster
users: 
  - name: mgmtsv-admin@sk.com
    user: 
      token: [TOKEN]
```

## context
* context는 cluster connection information이다.
* authentication info와 namespace를 사용하는 특정 cluster에 request를 보낼 때 사용하는 정보
* 3가지 parameter를 가짐
  * (optional)cluster
  * (optional)namespace
  * (optional)user
* default로 현재 communicate하는 cluster의 current context를 paramter로 사용함.
* To choose the current context:
  `kubectl config use-context`

## cluster
* cluster는 kubernetes cluster의 endpoint data를 가지고 있다.
* 구성
  * server: kubernetes apiserver에 접근하기 위한 qualified url
  * cluster's certificate authority 또는 `insecure-skip-tls-verify: true`
  접근 certificate가 필요한 경우 certificate-authority을 설정하고 필요없으면 insecure-skip-tls-verify에 true를 준다.
  * name: nickname, kubeconfig file안에서 cluster 구별을 위한 dictionary key(nickname)
* cluster를 추가 또는 수정하고 싶으면 `kubectl config set-cluster`

## users 
* user는 kubernetes cluster의 authenticating을 위한 client credentials 를 정의한다.
* 구성
  * name: kubeconfig가 load되거나 merge될 때 user list 안에서 key로서 사용하는 부분(일종의 nickname)
  * Available credentials: `client-certificate`, `client-key`, `token`, `username/password`
  * `username/password`와 `key`는 같이 사용 못함
  * `client certs`와 `key`는 함께 combined 가능
* user를 추가 또는 수정하려면 `kubectl config set-credentials`


-----------
## 참고 사이트
[KUBECONFIG and Context](https://theithollow.com/2019/02/11/kubernetes-kubeconfig-and-context/)
[KUBECONFIG 잘 정리된 github](https://github.com/zecke/Kubernetes/blob/master/docs/user-guide/kubeconfig-file.md)
