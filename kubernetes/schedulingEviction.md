# Scheduling and Eviction

## 특정 Node에 Pod 배포
ssd가 설치된 Node에 Pod를 배포하거나 GPU가 탑재된 Node에 Pod를 배포하는 등 특정 Node를 지정해야하는 경우들이 있다.
Pod를 특정 Node에 배포 하는 방법에는 여러가지가 있다.
* nodeSelector
* affinity

### nodeSelector
1. Node에 Label 할당
```bash
# Node에 Label 붙이기
# 문법: kubectl label nodes <node-name> <label-key>=<label-value>`
kubectl label nodes mynode disktype=ssd

# 라벨 확인
kubectl get nodes --show-labels
```
2. Pod에 라벨 지정
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```

### Affinity 
* Node Affinity: pod가 특정 node에 배포되는 것을 보장해줌
* Pod Affinity: pod가 동일한 node에 공존하는 것을 보장해줌

#### Node Affinity/Anti-Affinity 
* node affinity 타입
    * `requiredDuringSchedulingIgnoredDuringExecution`: 무조건 조건에 맞는 노드에 배포(Hard)
    * `preferredDuringSchedulingIgnoredDuringExecution`: 가능한(따라해봐요) 조건에 맞는 노드에 배포(Soft)
* supported operators: `In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt`
* `nodeSelectorTerms`를 여러 개 지정하는 경우, `nodeSelectorTerms` 중 하나라도 만족시키는 노드에 배포됨
* `matchExpressions`를 여러 개 지정하면, `matchExpressions` 모두를 만족하는 노드에 배포됨
* 스케줄된 노드의 label을 지우거나 변경해도 파드는 제거되지 않는다. 다시 말해서 어피니티 선택은 파드를 스케줄링 하는 시점에만 작동한다.
* `weight` 필드를 통해서 선호도를 지정 가능(1~100)
* 예시
```yaml
# 가능하면 특정 zone에 배포
apiVersion: v1
kind: Pod
metadata:
  name: with-node-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: "failure-domain.beta.kubernetes.io/zone"
              operator: In
              values: ["asia-south1-a"]
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value
```

