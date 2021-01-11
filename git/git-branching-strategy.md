# Git Braching Starategy(Git 브랜치 전략)
## 대표적인 브랜치 전략
* [Git-flow](#git-flow)
* [Github-flow](#github-flow)
* [Gitlab-flow](#gitlab-flow)
* [현재 내 프로젝트의 git branch 전략](#내-프로젝트-git-branching-strategy)

------
### Git Flow
![git flow](/images/git/git-flow.png)
* 브랜치 구분
    * Master: 제품으로 출시된 브랜치
    * Develope: 다음 출시 버전을 개발하는 브랜치
    * Feature: 기능을 개발하는 브랜치
    * Release: 이번 출시 버전을 준비하는 브랜치
    * Hotfix: 출시 버전에서 발생한 버그를 수정하는 브랜치
(Master/Develope은 Main, 나머지는 Sub)

* 처음에는 Develope, Master가 있음
* Master에서 Develope를 따낸다.
* 새로운 기능 개발시 Develope에서 Feature 브랜치는 딴다.
* 기능이 완료되면 Feature를 Develope로 Merge  


* QA를 위해 Develope에서 Release 브랜치 생성
* QA하면서 발생한 버그들은 Release에서 수정
* QA가 끝나면 Release 브랜치는 Master와 Develope으로 각각 Merge  

* Hotfix는 언제나 Master에서 시작
* 작업이 완료되면 Master와 Develope 브랜치로 각각 Merge

* 단점: 브랜치가 많아 복잡, 몇몇 branch는 포지션이 애매함
------
### Github flow
![github flow](/images/git/github-flow.png)
* master 브랜치는 항상 최신이며 Product에 배포되는 브랜치. 따라서 master 브랜치의 권한은 엄격하게 관리해야함.
* 새로운 기능을 만들기 위해 master에서 branch를 따면 어떤 기능인지 나타내기 위해 브랜치 이름을 명확하게 작성
* remote branch로 수시로 push
* 피드백이나 도움이 필요할 때, 그리고 merge준비가 완료되었을 때 `pull request`를 생성
* mater로 merge되면 즉시 배포되어야 한다.

* 장점: branch가 단순, 코드리뷰를 자연스럽게 하게됨
* 단점: 많은 것들이 올라오면 복잡해짐..
------
### Gitlab flow
github가 간단하지만 배포, 환경구성, 릴리즈, 통합에 대한 이슈가 있어 그것을 보안한 전략
![gitlab flow](/images/git/gitlab-flow.png)

* Production의 gitflow의 master와 비슷
* github flow의 단점인 안정성과 배포시기 조절에 대한 부분을 Production 브랜치를 두어 보장함
* 장점: production 브랜치에서 릴리즈된 코드가 항상 프로젝트의 최신 버전 상태를 유지해야 할 필요가 없음
------
### 내 프로젝트 git branching strategy
![my git branch strategy](/images/git/my-git-branch-strategy.png)
