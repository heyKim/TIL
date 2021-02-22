# Trouble shooting 
* 현상: Zuul로 동일한 API를 두 번 call하는 경우, 가끔 404가 떨어짐 
* 원인: 아직 원인 불명..Cloud 환경에 zuul을 올렸을 때 문제인지, Zuul 설정의 문제인지 파악 중 
클라우드 환경에 배포했을 때 문제인 경우 원인 파익이 더 어려움 
예를 들어 Zuul이 5개 Pod로 띄웠을 때, 이 Pod들이 각각 다른 물리서버에 뜨게 될 수도 있음 
물리 서버 중 일부에 방화벽 설정 또는 다른 기타 설정이 안되어 있을 수도 있음  
설정이 덜 된 물리서버 위에 올라간 Pod로 API Call이 가는 경우, 404 에러가 발생하게 됨 
 
* 현상: cifs mount 설정하여 배포했을 때, 130대역에 배포된 Pod는 잘 뜨고 120대역에 배포된 Pod는 permission 오류 발생 
    * Pod Fail Log 보면 "Failed to mount the network path: mount error(13): Permission denied 어쩌구 저쩌구" 라고 나옴 
    * 120 대역 내 새로 추가한 worker node(rhel7.9 사용)에서 수동으로 VM에 직접 마운트해도 permission denied 발생 
    * 120 대역 내 기존 VM은 mount 정상 동작 
        * CIFS 권한을 대역(xx.xxx.xxx.xx/24)으로 권한 받은게 아니라 IP(xx.xxx.xxx.xx)로 받은게 아닌지 의심 
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