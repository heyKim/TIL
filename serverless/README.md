# Serverless
## Glossary
* Serverless Architecture(Serverless) : 서버리스 개념으로 설계된 구조.
* Serverless Computing : Serverless Architecture를 구현하기 위해 필요한 자원, 클라우드 업체들을 통해 제공. (AWS Lambda, Azure Functions, IBM Cloud Functions 등)
* Serverless Application : Serverless Architecture로 구현된 애플리케이션.
* Serverless Framework : Serverless Application 을 보다 쉽게 구현하기 위해 사용되는 도구 (Serverless Framework, AWS SAM 등)
* FaaS : Function as a Service
-----
### Serverless Architecture
#### Benefits & Drawbacks
* Benefits
    * Cost Efficient: pay-as-you-go 서비스 모델이다 보니 인프라에 돈 쓸 필요가 없다.
    * Efficient Scaling: load 기반으로 자동 확장
    * 관리자의 Overhead 감소: 서버 관리가 불필요
    * Availability and Fault Tolerance
* Drawbacks
    * serverless 환경이 아닌 곳에서의 테스트가 어려움, 
    * vendor lock-in 경향이 있음: 특정 csp를 사용하다가 다른 vendor로 변경하려면 호환성 문제를 직면할 수도 있음.
    * Cold start issue: cloud provider는 serverless code를 사용하지 않을 때 container를 drop 해둠.(performance 떨어짐) 예를 들면 Lambda Container가 대기중인 상태가 아니다 보니 일정 시간이 경과된 후에 해당 페이지를 접속할 경우 로딩이 오래걸리는 issue들이 발생할 수 있다.

#### Improve startup latency
* cold start latency와 관련되 factor
    * Memory size: function에 할당된 메모리가 클수록 star-up이 빠르다.
    * Runtime: 보통 compiled runtime(Java, .NET,C#) 보다는 script 언어(Python, Ruby, Javascript)가 startup time이 덜 걸린다.
    * VPC: VPC 내부에 동작하는 function은 latency가 늘어난다. 왠만하면 VPC 밖에서 실행시키는 것이 낫다.
    * Code package size: package가 클수록 startup시간이 더 오래걸리는 건 당연함.

* container startup latency를 줄이는(mitigate) 방법
    * Monitor performance and log relevant indicators
        * log timestamps, duration outliers 체크
        * 
    * Increase memory allocation
    * Choose a faster runtime
    * Keep shared data in memory
    * Shrink package size
    * Keep a pool of pre-warmed functions
    * Use time-series forecasting
-----
## Serverless Computing
### Serverless Computing이란
서버가 없다는 것이 아니라 서버에 대해 관리할 필요가 없는 것
CPU, Memory, OS, Networking 등에 대해서 고민할 필요가 없다.

### Serverless Computing에 적합하지 않은 Case
* 주기적으로 높은 빈도의 서비스
* 작업시간이 오래 걸리는 서비스
* 높은 성능이 필요한 서비스
* 빠른 연산이 필요한 서비스

-----
## Serverless Framework
### 주요 Cloud Provider 의 서비스
* AWS Lambda (2014.11)
    * API Gateway, DynamoDB, CloudWatch, S3 등에서 event 받음
* Azure Functions
* Google Cloud Functions
### Serverless on Kubernetes
* OpenFaas
* Kubeless: kubernetes에서 FaaS 방식의 함수를 실행할 수 있게 해 주는 serverless framework
* Knative
* Fission
* OpenWhisk

-----
## Reference
* https://novemberde.github.io/aws/2018/02/02/Lambda_coldStart.html