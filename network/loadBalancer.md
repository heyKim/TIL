# LoadBalancer
* 로드밸런서의 종류
-----
## 로드밸런서의 종류
로드밸런서는 OSI 7계층을 기준으로 어떻게 부하를 분산하는지에 따라 종류가 나뉜다.
상위 계층으로 갈수록(L2->L7) 섬세한 부하 분산이 가능하지만 가격이 비싸짐
| LB | 특징                                               |  Protocal      |
|----|---------------------------------------------------|----------------|
| L2 | Data link 계층을 사용, Mac주소 기반 부하 분산     |                |
| L3 |     Network 계층을 사용, IP주소 기반 부하 분산    |                |
| L4 | Transport 계층을 사용, Port 기반 부하 분산        | TCP, UDP       |
| L7 | Application 계층을 사용, 요청(URL) 기반 부하 분산 | HTTP, HTTPS 등 |


## Reference
* [로드밸런서의 종류와 동작방식](!https://deveric.tistory.com/91)