# Azure App Configuration
* 응용 프로그램 설정 및 기능 플래그를 중아에서 관리하는 서비스
* 기능 플래그 관리를 위한 전용 UI
* Azure 관리 ID를 통한 보안 강화
* 유휴 또는 전송중인 완벽한 데이터 암호화
* 즉, 응용프로그램에서 사용하는 config를 모아둔 것+보안 적용해서(config를 담을 수 있는 서비스)

## Azure App Configuration 구성
* Keys and values
  * Azure App Configuration은 구성 데이터를 키-값 쌍으로 저장한다.
* Labels
  * 동일한 키로 키 값을 구별하는 데 사용(ex. 디버그 모드용인지 운영용인지..버전 별로)
  ```
  Key = AppName:DbEndpoint & Label = Test
  Key = AppName:DbEndpoint & Label = Staging
  Key = AppName:DbEndpoint & Label = Production
  ```

#### 실습
1. App Configuration 생성
  * Create Resource > App Configuration(config를 담을 수 있는 서비스) 생성
  * App Configuration> Access Keys> Read-only keys 메모 해두기
    * endpoint => https://hihikkh20config.azconfig.io
    * Connection string => Endpoint=https://hihikkh20config.azconfig.io;Id=tFEG-l6-s0:OG0NiVyGXfwNaenbV4rX;Secret=8oMTngvwdSFqJ+gAAJ+UaIk2Jxcaw5ocAowmSybSa8A=
  
  * App Configuration> Configuration explorer> Create> key-value
  ```
  Key: Broodwar:Zerg:Zealot # 이런 namespace 형식으로 하는 것 추천
  Value: ATTACK
  ```
2. 프로젝트 생성
* ConfigDemo 폴더 생성
`mkdir ConfigDemo`
* dotnet console 어플리케이션 생성
`dotnet new console -f netcoreapp3.1`
* 패키지 설치
`dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration`
* Program.cd 소스 수정
```net
using System;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Configuration.AzureAppConfiguration;
namespace ConfigDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder();
            builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
            var config = builder.Build();
            Console.WriteLine(config["Broodwar:Zerg:Zealot"] ?? "Hello world!");
        }
    }
}
```
* 실행을 위한 변수 설정
```
$Env:ConnectionString = "Endpoint=https://hihikkh20config.azconfig.io;Id=tFEG-l6-s0:OG0NiVyGXfwNaenbV4rX;Secret=8oMTngvwdSFqJ+gAAJ+UaIk2Jxcaw5ocAowmSybSa8A="
```
* 실행
```
dotnet build
dotnet run
```

