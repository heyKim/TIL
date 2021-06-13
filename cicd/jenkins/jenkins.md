# Jenkins
* Jenkins 설치(in AWS EKS)
* Pipeline
  * Pipeline 생성 방법
  * Pipeline 문법
    * Declarative pipelines
    * Scripted pipelines
  * Shared Libraries
* Plugin 및 설정(In-progress)
  * Github, Bitbucket 연동 - Credential 설정
  * Pull Request Trigger - Github Pull Request Builder/ Bitbucket Push and Pull Request
  * Jenkins Pipeline으로 원격지 배포 - Publish Over SSH Plugin
  * Jenkins Configuration을 Code로 더 쉽게 관리 - Configuration as Code Plugin
  * 원격으로 명령어를 전달하기 위한 설정 - Execute shell script on remote host using ssh Plugin
  * 작업 설정 이력 관리 - JobConfigHistory Plugin
  * 상위 프로젝트 커밋터에게 빌드 실패를 통보 기능 - Blame Upstream Committers Plugin
  * 젠킨스가 사용하고 있는 디스크의 용량을 그래프와 수치로 확인 - Disk Usage Plugin
* 백업
  * 젠킨스가 제공하는 백업 플러그인을 사용
  * rsync 를 이용


## Jenkins란
* 소프트웨어 개발 시 지속적으로 통합 서비스를 제공하는 툴이다.
* Jenkins는 자바로 개발되었으며 서블릿 컨테이너 위에서 구동된다. 

## Jenkins 설치(in AWS EKS)
### Install Jenkins with YAML files
#### Deploy Jenkins
* deployment.yaml 작성
```yaml {.line-numbers}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
      - name: jenkins
        image: jenkins/jenkins:alpine
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: jenkins-home
          mountPath: /var/jenkins_home
      volumes:
        - name: jenkins-home
          emptyDir: {}
```
* create the deployment  
```kubectl create -f jenkins-deployment.yaml -n jenkins```
#### Grant access to Jenkins service
```yaml {.line-numbers}
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
spec:
  selector:
    app: jenkins
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8080
  type: LoadBalancer
```
* (참고)[How do I expose the Kubernetes services running on my Amazon EKS cluster?](https://aws.amazon.com/ko/premiumsupport/knowledge-center/eks-kubernetes-services-cluster/)
#### 초기 설정
* 초기 credentials 확인
    * pod name 확인  
    `$ kubectl get pods -n jenkins`
    * log에서 initial password 확인  
    `$ kubectl logs <pod_name> -n jenkins`
    ![jenkin-initial-password](../assets/jenkins-initial-password.png)
    * initial password 입력
    ![unblock-jenkins](../assets/unblock-jenkins.png)
    * 이후 초기 설정 진행
    ![customize-jenkins](../assets/customize-jenkins.png)

## Pipeline
Jenkins Pipeline은 CD(Continuous Delevery)를 지원하는 플러그인의 집합이다.
### Pipeline 생성 방법
*  Blue Ocean: Blue Ocean 에서 Pipeline project를 셋팅하고, UI 상에서 Pipeline을 구성하면 Pipeline의 Jenkinsfile을 생성해준다.
* Jenkins UI: Jenkins UI상에서 바로 작성 가능하다. 이렇게 작성된 Jenkinsfile은 Jenkins 서버 home directory 위치에 저장된다. 더 많은 control을 하기 위해서 Blue Ocean이나 SCM 상에서 정의된 Jenkinsfile을 사용하는게 좋다.
* Jenkinsfile in source control: SCM 상에 Jenkinsfile을 정의한다.

### Pipeline 문법
* 파이프라인이 정의된 코드가 Jenkinsfile 파일이다.
* Pipeline 문법에는 Declarative Pipeline과 Scripted Pipeline 두가지가 있다.
Jenkins Pipeline은 Groovy 언어를 사용하는데 문법은 크게 Declarative pipelines과 Scripted pipelines 2가지로 나뉜다.
  * Declarative Pipeline : 보다 쉽게 작성 할 수 있게, 커스텀 되어 있음. Groovy-syntax기반 - Groovy 문법을 잘 몰라도 작성 가능
  * Scripted Pipeline : Groovy기반, Declarative보다 효과적으로 많은 기능을 포함하여 작성 가능. 하지만 작성 난이도가 높음(Groovy 문법을 잘 알아야 함) 복잡한 요구사항을 가진 사용자에게 이상적이다.
2가지 문법을 번갈아 사용 가능 - 동시에는 X
* Jenkins Pipeline Concepts

| Term     | Description                                                                                                                                                                                                                |
|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pipeline | The pipeline is a set of instructions given in the form of code for continuous delivery and consists of instructions needed for the entire build process. With pipeline, you can build, test, and deliver the application. |
| Node     | The machine on which Jenkins runs is called a node. A node block is mainly used in scripted pipeline syntax.                                                                                                               |
| Stage    | A stage block contains a series of steps in a pipeline. That is, the build, test, and deploy processes all come together in a stage. Generally, a stage block is used to visualize the Jenkins pipeline process.           |
| Step     | A step is nothing but a single task that executes a specific process at a defined time. A pipeline involves a series of steps.                                                                                             |

#### Declarative pipelines
문법 생성기 위치 : `<Your_Jenkins_Url>/directive-generator/`
```groovy
pipeline {
    agent any // 실행할 agent 선언
    stages {
        stage('Git Pull') {
            steps {
                git (
                    branch: "${YOUR_GIT_BRANCH}",
                    credentialsId: "${YOUR_CREDENTIAL_ID}",
                    url: "${YOUR_GIT_URL}"
                )
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        }
        stage('Test') {
            steps {
                script { // if문과 같은 scirpt를 쓰기 위해 선언
                    if (!params.TEST_SKIP) {
                        echo "start test"
                    }
                }
            }
        }
    }
    // 빌드 결과에 따른 동작 정의
    post {
        always {
            echo "Build Finish"
        }
        success {
            echo "Build Success"
        }
        failure {
            echo "Build Failure"
        }
    }
    // 환경변수 정의
    environment {
        DIR="repo"
    }
    // 파라미터 정의
    parameters {
        booleanParam(name: 'TEST_SKIP', defaultValue: true, description: '테스트 스킵 여부')
    }
    // 옵션 정의
    options {pipeline {
    agent any // 실행할 agent 선언
    stages {
        stage('Git Pull') {
            steps {
                git (
                    branch: "${YOUR_GIT_BRANCH}",
                    credentialsId: "${YOUR_CREDENTIAL_ID}",
                    url: "${YOUR_GIT_URL}"
                )
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        }
        stage('Test') {
            steps {
                script { // if문과 같은 scirpt를 쓰기 위해 선언
                    if (!params.TEST_SKIP) {
                        echo "start test"
                    }
                }
            }
        }
    }
    // 빌드 결과에 따른 동작 정의
    post {
        always {
            echo "Build Finish"
        }
        success {
            echo "Build Success"
        }
        failure {
            echo "Build Failure"
        }
    }
    // 환경변수 정의
    environment {
        DIR="repo"
    }
    // 파라미터 정의
    parameters {
        booleanParam(name: 'TEST_SKIP', defaultValue: true, description: '테스트 스킵 여부')
    }
    // 옵션 정의
    options {
        timestamps() //timestamp 기능
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '5') // 오래된 빌드 삭제
    }
    // 빌드 도구 정의
    tools {
        jdk 'JDK14' // 빌드시 사용 할 JDK
        maven 'M3' // 빌드시 사용 할 maven
    }
}
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '5') // 오래된 빌드 삭제
    }
    // 빌드 도구 정의
    tools {
        jdk 'JDK14' // 빌드시 사용 할 JDK
        maven 'M3' // 빌드시 사용 할 maven
    }
}
```

#### Scripted pipelines
문법 생성기 위치 : `<Your_Jenkins_Url>/pipeline-syntax/`
```groovy
node {
    // 환경변수 정의
    def DIR="repo"

    // 빌드 도구 정의    
    def jdk = tool name: 'JDK14', type: 'jdk'
    def maven = tool name: 'maven3.5', type: 'maven'
    env.JAVA_HOME = "${jdk}"
    env.MAVEN_HOME = "${maven}"
    
    //timestamp 기능
    timestamps {
        // 빌드 결과에 따른 동작 정의
        try {
            stage('Git Pull') {
                git (
                    branch: "${YOUR_GIT_BRANCH}",
                    credentialsId: "${YOUR_CREDENTIAL_ID}",
                    url: "${YOUR_GIT_URL}"
                )
            }
            stage('Build') {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
            stage('Test') {
                if (!params.TEST_SKIP) {
                    echo "start test"
                }
            }
            echo "Build Success"
        } catch (e) {
            echo "Build Failure"
            throw e

        } finally  {
            echo "Build Finish"
        }
    }
    properties([
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '5')), // 오래된 빌드 삭제
        parameters([booleanParam(name: 'TEST_SKIP', defaultValue: true, description: '테스트 스킵 여부')]) // 파라미터 정의
    ])
}
```
## 참고
* [Jenkins-Pipeline](https://velog.io/@dogy/Jenkins-Pipeline)
* [Pipeline 문법](https://waspro.tistory.com/554)
* [Jenkins 백업](https://www.lesstif.com/continuous-integration/%EC%A0%A0%ED%82%A8%EC%8A%A4-%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%93%9C%EC%99%80-%EB%B0%B1%EC%97%85-31851139.html)