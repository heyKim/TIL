# Azure Logic Apps
## Basic
### 개념
* 자동화 워크플로 솔루션
  * 통합 솔루션을 빠르게 만들 수 있는
* JSON 기반 워크플로 정의
  * Azure Resource Manager 템플릿을 사용하여 배포 가능
### 구성
* 워크플로
  * 일련의 단계로 서술된 비즈니스 프로세스
* 트리거
  * 새 워크플로 인스턴스를 호출할 단계
* 작업
  * 워크플로의 개별 단계, 일반적으로 커넥터 또는 사용자 지정 API앱
* 커넥터
  * 특정 서비스 또는 데이터 원본과 통합할 준비가 되고 미리 빌드된 API앱의 특별한 사례
  * 예) Twitter의 글자를 읽어내는 커넥터, SQL Server Connector, 내부 ERP를 읽어보는 커넥터 등
  * [지원하는 커넥터 리스트](https://docs.microsoft.com/en-us/connectors/connector-reference/connector-reference-logicapps-connectors)

(로직앱 <-> Function)

* 워크플로 구성요소
  * 트리거 -> 작업 -> 작업 -> 커넥터 -> 작업 -> 커넥터 -> 작업
* 커넥터 구성요소
  * 작업: 사용자가 만든 변경사항
  * 트리거: 이벤트가 발생할 때 앱에 알림
    * 폴링 트리거
    * 푸시 트리거
    
### B2B 시나리오 및 엔터프라이즈 통합팩
다양한 형식을 변형하는 특수 팩

### Logic Apps Designer

## 실습
시나리오: OneDrive Target 폴더 아래 파일을 인식해서 번역. 번역한 정보는 Storage account에 담았다가 다시 OneDrive 폴더 아래 번역된 파일이 떨어진다.
1. Logic App 생성 
2. OneDrive에 Target 폴더 생성
3. Translator 생성
4. Logic App Desiner를 통해 Connector 생성
  * OneDrive Connetor 추가
  * Microsoft Translator V2 Connector 추가
  * Blob Conntector 추가
  * 번역된 파일을 저장할 OneDrive Connetor 추가
  ![logicapp](https://user-images.githubusercontent.com/35487539/103986218-b6166a80-51cd-11eb-8cbc-f77fef1d3cda.JPG)

