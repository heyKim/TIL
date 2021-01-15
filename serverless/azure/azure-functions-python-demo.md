# Azure Functions Python Demo

## VSCode 에서 Azure Functions 생성
### Prerequisites
#### Install software
* VS Code
* Python: 3.6버전 이상(내 로컬은 Python 3.7.5)
* VS Code Python extension  
(*[환경세팅 참고](https://code.visualstudio.com/docs/python/python-tutorial)*)
* VS Code Azure Functions extension

### Python 함수 생성
* (in VS Code) F1> Azure Functions: Create New Project.
* (in prompt)  

| Prompt                                                     | Value               | Description                     |
|------------------------------------------------------------|---------------------|---------------------------------|
| Select a folder that will container your functions project | Current open folder |                                 |
| Select a language                                          | Python              |                                 |
| Select Python interpreter to create a virtual environment  | (Default)           |                                 |
| Select a template for your project's first function        | HTTP trigger        |                                 |
| Provide a function name                                    | HTTPTrigger1        |                                 |
| Authorization level                                        | Anonymous           | anyone can access this function |

### 로컬에서 디버그
* 디버깅(F5)
![Debugging](/images/azure/azure-function-debug-log.png)
* restAPI 테스트
![Http Test](/images/azure/azure-function-apitest.png)
### Azure에 배포
* Upload settings  
![Upload settings](/images/azure/azure-function-deploy.png)
* View output
```
오후 2:45:31 func-kikihihi-prod-01: HTTP Trigger Urls:
  HttpTrigger1: https://func-kikihihi-prod-01.azurewebsites.net/api/HttpTrigger1
```
* 결과 확인
![Azure Functions in Azure Portal](/images/azure/azure-function-deploy-list.png)
리소스 그룹 아래 storage account, Hosting Plan, Function App, Application Insights(모니터링 도구)가 생성된 걸 확인 할 수 있다.  
Azure Functions는 default로 Consumtion plan을 사용함(Consumtion plan을 사용해도 App Service plan이 생성됨)
* 배포된 Azure Function Test
 - url: `https://func-kikihihi-prod-01.azurewebsites.net/api/HttpTrigger1?name=kikihihi`
 - body: `Hello, kikihihi. This HTTP triggered function executed successfully.`

#### Useful command
Open the Command Palette
* macOS: `Command+Shift+P`
* Windows and Linux: `Ctrl+Shift+P` or `F1`
-----
## 로컬 Azure Functions 프로젝트 생성
`func init func-kikihihi-01`
```
Select a worker runtime:
dotnet
node
python 
powershell
```
* 출력
```
Use the up/down arrow keys to select a worker runtime:python
Found Python version 3.7.5 (py).
Writing requirements.txt                                                       ensions.json
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\heyKim\azure-study-code\azure-functions\func-kikihihi-01\.vscode\extensions.json
```


## Reference
* https://docs.microsoft.com/ko-kr/azure/developer/python/tutorial-vs-code-serverless-python-01
