# Continuous Integration (CI) and Continuous Delivery (CD)
code를 지속적으로(?) 배포하고 버그를 미리 detect하고 소스통합 시 문제점을 빠르게 파악할 수 있게 도와줌
* Jenkins
* Travis CI
* Buddy
* CircleCI
* CodeShip
* Codefresh
* Skycap
* AppVeyor
* Percy
* TeamCity

-----
## Jenkins
* Java-based cross platform, Open-Source, enterprise-level급
* delivery pipeline을 code로 작성 가능
* Plugin ecosystem이 엄청 많음(1500개 이상?)
* Windows, Linux, macOS 지원
* AWS, Google Cloud, Azure 등 cloud platform과 연동됨
* [Jenkins Tutorials](https://www.lambdatest.com/blog/jenkins-pipeline-tutorial/)

## Travis CI
* Ruby로 만들어져있음, Open-Source or 기업용
* 지원하는 Programming Language가 30개 이상
* CI/CD Pipeline은 YAML로 작성함(travis.yml)

## CircleCI
* CircleCI Server는 on-premise solution, CircleCI Cloud는 cloud-based solution
* CircleCI Cloud는 Docker, Linux, macOS, Android, Windows 등 지원
* CircleCI Server는 Docker, Linux, Android, Windows 등 지원
* 설치가 쉽고 Github,Bitbucket 과 같은 version-controlling system이랑 연동하기 쉬움(*To-do: Jenkins도 쉬운데..?비교 해봐야할듯*)
* AWS, Google Cloud, Azure 등 cloud platform과 연동됨
* CI/CD Pipeline은 YAML로 작성함(config.yml)


