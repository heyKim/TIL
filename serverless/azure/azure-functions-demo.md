# Azure Functions Demo
## Portal에서 간단한 Function만들기
1. Function App 생성(Function App은 function을 담는 상자일뿐)
2. Functions생성
    * HTTP 트리거를 이용한 Function
    * Blob 트리거를 이용한 Function: Blob에 데이터 들어오면 function실행
        - trigger path: samples-workitems/{name}
        - output path: outcontainer/{rand-guid}

## VS Code에서 간단한 Function만들기
### prerequisite
* VS Code 에 Azure Functions Extension 설치
* function emulator 설치(function은 클라우드 기반이기 때문에 로컬에서 돌리기 위해서)
    - https://github.com/Azure/azure-functions-core-tools  
    (* 참고: Visual Studio 에서는 Azure Core Tool이 깔려있음)

## Reference
* [Azure Functions Lab](https://github.com/MicrosoftLearning/AZ-204-DevelopingSolutionsforMicrosoftAzure/blob/master/Instructions/Labs/AZ-204_02_lab_ak.md)