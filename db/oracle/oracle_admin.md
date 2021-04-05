# Oracle Admin 
## Oracle DB 계정 생성 및 사용자 확인 
* 현재 생성된 계정 확인 
```sql
SELECT * FROM DBA_USERS; 
SELECT * FROM ALL_USERS; 
``` 
* 사용자에게 부여된 시스템 권한 확인 
```sql
SELECT * FROM DBA_SYS_PRIVS 
WHERE GRANTEE='사용자명' 
``` 
* 사용자에게 부여된 롤 확인  
```sql
SELECT * FROM DBA_ROLE_PRIVS WHERE GRANTEE='사용자명';
``` 
 
* 계정 생성    
```sql
CREATE USER 계정명 IDNETIFIED BY 비밀번호;
``` 
 
* 계정 비밀번호 변경
    * 비밀번호에 특수문자가 들어갈 경우 "" 따옴표로 묶어서 입력한다 
    * 변경할 비밀번호는 문자로 시작해야함 
```sql
ALTER USER 계정명 IDENTIFIED BY 변경할 비밀번호;
```   
    
* 비밀번호 만료일 조회 
```sql
SELECT * FROM DBA_PROFILES WHERE PROFILE='DEFAULT';
``` 
 
* 비밀번호 만료일 변경 
```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME [만료일]; 
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED; 
``` 
 
* 계정 락 해제 
```sql
ALTER USER [계정명] ACCOUNT UNLOCK;
``` 
 
* LOCK 계정 조회하기 
```sql
SELECT USERNAME, ACCOUNT_STATUS, LOCK_DATE FROM DBA_USERS;
```