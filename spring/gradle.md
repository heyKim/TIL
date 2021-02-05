# Gradle 
 
------- 
## Gradle Wrapper 
프로젝트를 새로운 환경에 설치할때 별도의 설치나 설정과정없이 곧 바로 빌드할 수 있게 하기 위함 
Java나 Gradle도 설치할 필요가 없다. 또한 로컬에 설치된 Gradle 또는 Java의 버전도 신경쓸 필요가 없다. 따라서 항상 Wrapper를 사용할 것을 권장한다. 
* gradle wrapper 생성시 아래와 같은 파일이 생성된다 
``` 
├── build.gradle 
├── settings.gradle 
├── gradle 
│   └── wrapper 
│       ├── gradle-wrapper.jar 
│       └── gradle-wrapper.properties 
├── gradlew 
└── gradlew.bat 
``` 
* `gradle-wrapper.jar`: Gradle distribution 다운로드를 위한 코드를 포함한 Wrapper JAR file 
* `gradle-wrapper.properties`: Wrapper runtime 설정파일. 이 파일의 wrapper 버전 등을 변경하면 task 실행시, 자동으로 새로운 Wrapper 파일을 로컬 캐시에 다운로드 받는다. 
* `gradlew`: wrapper 빌드를 위한 리눅스용 shell script 
* `gradlew.bat`: wrapper 빌드를 위한 윈도우용 batch script 
* `build.gradle`: 의존성이나 플러그인 설정 등을 위한 스크립트 파일 
* `settings.gradle`: 파일은 프로젝트의 구성 정보를 기록하는 파일이다. 어떤 하위프로젝트들이 어떤 관계로 구성되어 있는지를 기술한다. Gradle은 이 파일에 기술된대로 프로젝트를 구성한다. 
 
------- 
## Spring Boot Gradle Plugin 
Spring Boot의 의존성을 관리하고 어플리케이션 패키징을 도와주는 도구 
실행가능한 jar, war 파일들로 패키징을 해주기도 하고, `spring-boot-dependencies`를 통해서 의존성 관리 기능을 제공한다. 
Spring Boot Gradle Plugin은 Gradle 4.4이상의 버전을 필요로 합니다. 
 
### Spring Boot Gradle Plugin 적용방법 
build.gradle내 plugins 블록을 이용하여 원하는 plugin을 추가하여 적용 
```groovy 
plugins { 
	id 'org.springfr amework.boot' version '2.4.2' // org.springframework.boot plugin 적용 
	id 'io.spring.dependency-management' version '1.0.11.RELEASE' // io.spring.dependency-management plugin 적용 
	id 'java' // java plugin 적용 
} 
``` 
```yml 
// legacy method 니깐 이제는 이런 식으로 사용하지 말 것 
apply plugin: 'anypluginanme' 
apply plugin: 'maven' 
``` 
 
#### Maven Plugin를 이용한 Deploying to a Maven repository 
```groovy 
    // Add Maven plugin 
    plugins { 
        id 'maven' 
    } 
    // Upload of file to remote Maven repository 
    uploadArchives { 
        repositories { 
            mavenDeployer { 
                repository(url: "file://localhost/tmp/myRepo/") 
                // snapshotRepository(url: 'http://repo.yourcompany/maven-snapshots/') { 
                //     authentication(userName: 'username', password: 'password') 
                // } 
            } 
             
        } 
    } 
``` 
* 더 자세한 내용은 [Maven Plugin](https://docs.gradle.org/current/userguide/maven_plugin.html#sec:maven_convention_properties) 참고 

------- 
### Managing Dependencies 
build.gradle 내 plugin 블록에 `io.spring.dependency-management`를 추가하면 Spring Boot의 Plugin이 자동적으로 사용중인 Spring Boot 버전에 맞게 `import the spring-boot-dependencies bom`를 import해서 의존성 관리를 해준다. 
 
#### Managing Dependencies 적용 
1. io.spring.dependency-management 추가 
```yml 
plugins { 
	id 'io.spring.dependency-management' version '1.0.11.RELEASE' 
} 
``` 
2. Repository 설정 
`repositories` block에 의존성을 가져올 주소를 설정한다. 
* 기본 Maven Repository 사용 시 
    * mavenCentral은 Maven 저장소을 호스팅하는 데 더 널리 사용되는 Nexus 소프트웨어의 배후 회사인 Sonatype 에서 운영 
```groovy 
  repositories { 
    maven { 
        // 기본 Maven Repository 사용 
        mavenCentral() 
    } 
  } 
``` 
* 개별 Maven Repository 사용 시 
```groovy 
  repositories { 
    maven { 
        // 레포지토리 접속에 필요한 계정정보 추가(필요없으면 빼면 됨) 
        credentials { 
            username 'user' 
            password 'password' 
        } 
        url "http://repo.yourcompany.com/maven2" 
    } 
  } 
``` 
* jcenter 사용 시 
    * jcenter는 안드로이드 개발자 분들이 많이 쓰는 Repository 
    * jcenter은 Bintray와 Artifactory를 만든 회사 인 JFlog에서 운영 
```groovy 
    repositories { 
        jcenter() 
    } 
``` 
 
2. Dependency 설정 
build.script의 `dependencies` 블록에 여러 가지 다양한 종속성 구성(implementation, compileOnly, runtimeOnly, annotationProcessor 등)을 사용하여 라이브러리 종속성을 선언할 수 있다. 
```groovy 
  dependencies { 
  	// 모니터링 
	implementation 'org.springframework.boot:spring-boot-starter-actuator' 
	compileOnly 'org.projectlombok:lombok' 
	developmentOnly 'org.springframework.boot:spring-boot-devtools' 
	annotationProcessor 'org.projectlombok:lombok' 
  } 
``` 
* `dependencies` block: 설정된 Repository에서 가져올 아티팩트를 설정 
    * implementation 
    * compile: 컴파일 타임에 의존성을 받아옴
        * deprecated 됨, implementation으로 대체하여 사용 
        * compile을 사용하게 되면 연결된 모든 모듈의 API가 exposed(노출)된다고 한다. 
    * runtime: 실행할때 의존성을 받아옴(기본적으로 컴파일을 모두 포함) 
    * testCompile: 프로젝트의 테스트를 컴파일할 때 필요한 라이브러리인 경우 (deprecated 됨, testimplementation으로 대체하여 사용) 
    * testRuntime: 프로젝트의 테스트를 실행할 때 필요한 라이브러리인 경우 
        * testRuntime는 기본적으로 compile, runtime, testCompile 의존성을 포함함 
    * compileOnly: 컴파일할때는 사용하고, 아티팩트(빌드 결과물)를 만들때는 포함하지 않음. runtime 시 필요없는 라이브러리인 경우에 사용 
    * runtimeOnly: runtime 시에만 필요한 라이브러리인 경우 
    * developmentOnly 
    * annotationProcessor: annotation processor 명시 
 
* (참고) [implementation vs compile](https://hack-jam.tistory.com/13)
