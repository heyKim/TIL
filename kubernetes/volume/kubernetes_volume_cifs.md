# Kubernetes CIFS Volume Mount 
 
## CIFS Volume Mount 방법 
### Secret 생성 
* Secret을 생성하기 위해서 CIFS username과 password 를 base64로 encoding 해야한다. 
```cmd 
echo -n username | base64 
echo -n password | base64 
``` 
* 인코딩한 username과 password를 가지고 `secret.yml` 작성 
```yaml 
apiVersion: v1 
kind: Secret 
metadata: 
  name: cifs-secret 
  namespace: default 
type: fstab/cifs 
data: 
  username: 'ZXhhbXBsZQ==' 
  password: 'bXktc2VjcmV0LXBhc3N3b3Jk' 
``` 
* Secret 생성 
`kubectl apply -f secret.yml` 
* 생성한 Secret 확인 
`kubectl describe secret cifs-secret` 
 
### CIFS를 이용해 Volume Mount  
* `pod.yml` 작성 
```yaml 
apiVersion: v1 
kind: Pod 
metadata: 
  name: busybox 
  namespace: default 
spec: 
  containers: 
  - name: busybox 
    image: busybox 
    command: 
      - sleep 
      - "3600" 
    imagePullPolicy: IfNotPresent 
    volumeMounts: 
    - name: busybox-cifs-0 
      mountPath: /data 
  volumes: 
  - name: busybox-cifs-0 
    flexVolume: 
      driver: fstab/cifs 
      fsType: cifs 
      secretRef: 
        name: cifs-secret 
      options: 
        networkPath: //server/share 
        mountOptions: dir_mode=0755,file_mode=0644,noperm 
``` 
* Pod 생성 
`kubectl apply -f pod.yml` 
 
### Testing  
* 컨테이너 안에 들어가서 /data로 마운트 되어 있는지 확인  
`kubectl exec -ti busybox /bin/sh` 
------
### CIFS Trouble Shooting
* 현상: cifs mount 설정하여 배포했을 때, 130대역에 배포된 Pod는 잘 뜨고 120대역에 배포된 Pod는 permission 오류 발생 
    * Pod Fail Log 보면 "Failed to mount the network path: mount error(13): Permission denied 어쩌구 저쩌구" 라고 나옴 
    * 120 대역 내 새로 추가한 worker node(rhel7.9 사용)에서 수동으로 VM에 직접 마운트해도 permission denied 발생 
    * 120 대역 내 기존 VM은 mount 정상 동작 
        * CIFS 권한을 대역(xx.xxx.xxx.xx/24)으로 권한 받은게 아니라 IP(xx.xxx.xxx.xx)로 받은게 아닌지 의심(결국 아니었음) 
* 원인: 새로 추가된 Node만 OS Kernel을 rhel7.9로 했더니 kernel version 이슈 발생 
    * rhel7.9에서는 cifs username에 domain이 포함되면 domain옵션을 따로 주어야함. 
    * 즉, username=user01@yourcompany.com 이면 username=user01, option은 domain=yourcompany.com 으로 주어야함. 
    ```yaml 
    # secret.yml 
    apiVersion: v1 
    kind: Secret 
    metadata: 
    name: cifs-secret 
    namespace: default 
    type: fstab/cifs 
    data: 
    username: 'ZXhhbXBsZQ==' # 도메인을 뺴고 user01만 
    password: 'bXktc2VjcmV0LXBhc3N3b3Jk' 
    ``` 
    ```yaml 
    # deployment.yml 
    volumes: 
        - 
          name: timezone-config 
          hostPath: 
            path: /usr/share/zoneinfo/Asia/Seoul 
        - name: busybox-cifs-0 
          flexVolume: 
            driver: fstab/cifs 
            fsType: cifs 
            options: 
              mountOptions: dir_mode=0755,file_mode=0644,noperm,domain=yourcompany.com # domain을 mountOptions으로 줌 
              networkPath: //server/share 
            secretRef: 
              name: cifs-secret 
    ```


## Reference 
* [CIFS Flexvolume Plugin for Kubernetes](https://github.com/fstab/cifs)