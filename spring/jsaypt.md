# Jsaypt(Java Simplified Encryption) 
* Spring Boot provides utilities for encrypting property sources 
 
## Spring Boot에 Jsaypt 적용 방법 
### Dependency 추가 
```xml 
<!--Mavne--> 
<!-- https://mvnrepository.com/artifact/com.github.ulisesbocchio/jasypt-spring-boot-starter --> 
<dependency> 
    <groupId>com.github.ulisesbocchio</groupId> 
    <artifactId>jasypt-spring-boot-starter</artifactId> 
    <version>3.0.3</version> 
</dependency> 
``` 
```yml 
# Gradle 
compile group: 'com.github.ulisesbocchio', name: 'jasypt-spring-boot-starter', version: '3.0.3' 
``` 
* (참고) jasypt-spring-boot-starter는 개인 프로젝트임. Spring에서 공식지원하는 라이브러리는 없음 
 
### Custom Jsaypt Encryptor 생성 
* jasypt Config 설정 
```java 
@Configuration 
public class JasyptConfig { 
 
@Bean("encryptorBean") // 바로 bean에 등록하여 사용 
    public StringEncryptor stringEncryptor() { 
        PooledPBEStringEncryptor encryptor = new PooledPBEStringEncryptor(); 
        SimpleStringPBEConfig config = new SimpleStringPBEConfig(); 
        config.setPassword("somePassword"); //암호화에 사용할 키 (중요함) 
        config.setAlgorithm("PBEWithMD5AndDES"); //사용할 알고리즘 
        config.setKeyObtentionIterations("1000"); 
        config.setPoolSize("1"); 
        config.setProviderName("SunJCE"); 
        config.setSaltGeneratorClassName("org.jasypt.salt.RandomSaltGenerator"); // Default salt는 RandomSaltGenerator 
        //고정된 Salt를 사용하려면 config.setSaltGenerator(new StringFixedSaltGenerator("thisFixedSalt")); 와 같이 정의 
        config.setStringOutputType("base64"); 
        encryptor.setConfig(config); 
        return encryptor; 
    } 
} 
``` 
 
### 간단한 암호화 실행 
* 아래와 같이 작성하여 암호화를 간단히 실행할 수 있다. 
```java 
@Override 
	public void encryptSimpleTest() { 
		StandardPBEStringEncryptor encryptor = new StandardPBEStringEncryptor(); 
		encry

ptor.setPassword("somePassword"); //암호화에 사용할 키 (중요함) 
		encryptor.setAlgorithm("PBEWithMD5AndDES"); 
		String str = "testString";  //암호화할 평문 
		String encStr = encryptor.encrypt(str); // 암호화 
		String decStr = encryptor.decrypt(encStr);  //복호화 
		System.out.println("encStr: " + encStr); //암호화 한 내용을 출력 
        System.out.println("decStr: " + decStr); //복호화 한 내용을 출력 
	} 
``` 
 
### Jsaypt 적용하여 Properties 암호화 
* 위의 실행 암호화를 했을 때 실행 결과로 나온 `ENC(Gxloau1Q4SIPesrGOMaUhQ==)`와 같은 값들을 properties 또는 yaml 정보들을 아래와 같이 변경해주면 됨. 
```yml 
spring.datasource.url=ENC(Gxloau1Q4SIPesrGOMaUhQ==) 
spring.datasource.username=ENC(Gxloau1Q4SIPesrGOMaUhQ==) 
spring.datasource.password=ENC(Gxloau1Q4SIPesrGOMaUhQ==) 
``` 
 
## Reference 
* [Spring Boot Configuration with Jasypt](https://www.baeldung.com/spring-boot-jasypt) 
* [Jasypt 암복호화 하기](https://luvstudy.tistory.com/67) 
* [Java Code Examples for PooledPBEStringEncryptor](https://www.programcreek.com/java-api-examples/?api=org.jasypt.encryption.pbe.PooledPBEStringEncryptor)