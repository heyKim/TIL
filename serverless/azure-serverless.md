# Azure Serverless
## Azure Function이란?
* 2016년 3월에 발표
* AWS Lambda, Google Cloud Functions, IBM OpenWhisk 와 비슷한 서비스

### Glossary
* Functions App: Azure Function들을 담는 그룹. Functions App을 생성해야 그 아래 Azure Function을 생성할 수 있다.
* Azure Function: 클라우드 환경에서 작은 Function or Code를 실행하기 위한 솔루션
* Trigger: Azure Function을 실행시키는 Event

### 함수 통합
Notification Hubs, Event Grid, Event Hubs, Cosmos DB, Twilio, 스토리지 등에서 Functions을 실행 시킬 수 있음

-----
### Function App
* pricing model
    * Consumption Plan: 함수가 실행 중인 경우에만 비용 지불. 들어온 이벤트 수만큼 인스턴스 동적으로 증가
    * App Service Plan: Web Apps, API Apps, Mobile Apps와 같은 App Service App과 동일한 전용 VM에서 실행
    * Premium Plan: pre-warm up이 되어있는 인스턴스(response빠름), 무제한의 실행시간 지원. (단, 윈도우 기반만 지원)
### Azure Functions
* 지원하는 Language: C#, Java, PHP, Python, PowerShell, JavaScript
### Trigger
* Azure 서비스 기반 트리거
    * Cosmos DB
    * Blob Storage
    * A Service Bus Queue trigger (e.g. a workitem from another Function)
    * An IoT/Event Hub message (e.g., a message from a device or service)
* 일반적인 시나리오을 기반으로 하는 트리거
    * HTTP Trigger (e.g., for a REST API)
    * A scheduled timer (e.g., run every 5 minutes)
* 외부 서비스와 연계된 트리거
    * A Webhook fires (e.g., Github project update)
    * Twilio(SMS message)


-----
## Durable Functions
상태 저장 함수를 쓰게 해주는 Azure Functions의 확장

