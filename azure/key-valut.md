# Key Vault

## Key Vault 개념
* 클라우드 앱 및 서비스에서 사용하는 암호화 키 및 기타 비밀 보호
  * 키와 암호 관련 보안 및 제어 수준 개선
	* 애플리케이션이 키에 직접 액세스할 수 없음
	* FIPS 140-2 수준 2에서 유효성이 확인된 HSM(하드웨어 보안 모듈) 사용
	* 만들기 및 가져오기: 암호화 키,API키,비밀,암호,SSL/TLS 인증서
	
* Key Valut 개념
	* Valut: 비밀의 논리적 그룹
	* Valut 소유자: 자격 증명 모음을 완전히 제어할 수 있는 ID, 다른 ID소비자(범위) 액세스 권한을 자격 증명 모음에 부여할 수 있습니다.
	* Valut 소비자: (권한으로) 자격 증명 모음 내부에 자산에 대한 작업을 수행할 수 있는 ID
* Key Valut 인증 방법 2가지
  * 관리 ID: Azure 리소스용 할당 ID(recommended)
  * 서비스 주체: 인증서 또는 비밀을 제공(권장 ㄴㄴ)
* 키 자격 증명 모음 비밀 유형
  * 인증서<- 암호화 키, 비밀
		
### Azure CLI를 사용하여 Key Valut 비밀 만들기
```
# Azure CloudShell로 로그인
# PowerShell로 설정
# 변수를 먼저 설정합니다.
$myResourceGroup="hihikkh18rg"
$myKeyVault="hihikkh18kv"
$myLocation="eastus2"

# 리소스 그룹을 만듭니다.
az group create --name $myResourceGroup --location $myLocation

# Key vault를 만듭니다.
az keyvault create --name $myKeyVault --resource-group $myResourceGroup --location $myLocation

# Secret추가하고 검색합니다.
# Secret을 만들고 앱에서 사용할 수 있는 암호를 추가해봅니다.
az keyvault secret set --vault-name $myKeyVault --name "ExamplePass" --value "P@ssw0rd1234"
# 생성된 secret을 검색해봅니다.
az keyvault secret show --name "ExamplePass" --vault-name $myKeyVault
```

## Azure AD 관리 ID
자격 증명을 코드와 따로 둔다

관리 ID 구현(Azure VM에서)

#### 관리되는 ID 유형
* 시스템 할당 관리 ID
	* Azure 서비스 인스턴스 하나에 시스템 할당 관리 ID를 둔다. 인스턴스 삭제시 ID도 삭제
	* Azure에서는 이 구성을 recommended
	* 리소스 간 공유 불가

* 사용자 할당 관리 ID
	* 여러 Azure 리소스 간에 공유할 수 있다.
	
|---------|시스템 할당   | 사용자 할당|
|생성

#### 실습
* Azure CLI를 사용하여 Web Apps용 관리 ID구성
`az webapp identity --name <app-name> --resource-group <resource-group> #시스템 할당 관리 ID사용`

* Azure CLI를 사용하여 Azure Container Instance용 관리 ID 구성
```
# 시스템 할당된 관리 ID로 컨테이너 만들기
az container create --resource-group hihikkh19rg --name hahaysh19aci --image microsoft/azure-cli --assign-identity --command-line "tail -f /dev/null"
```

* Azure CLI를 사용하여 사용자 할당 관리 ID 만들기
```
# 사용자 할당 관리 ID 만들기
az identity create --resource-group hihikkh19rg --name hihikkh19id
resourceID=$(az identity show --resource-group hihikkh19rg --name hihikkh19id --query id --output tsv)
# 사용자 할당 관리 ID 를 이용해서 aci2 생성
az container create --resource-group hihikkh19rg --name hihikkh19aci2 --assign-identity $resourceID --image microsoft/azure-cli
```

tip: 사용자 ID있는데 System ID도 만들수 있음

key vault 접근 권한 부여
key vault > Access Policies

## 참고
https://docs.microsoft.com/ko-kr/azure/container-instances/container-instances-managed-identity
