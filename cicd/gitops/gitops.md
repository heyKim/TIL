# GitOps
## GitOps란
![GitOps Concept](/cicd/assets/gitops-concept.jpeg)
* GitOps란, 배포의 형상을 선언형으로 기술하여 Config 리포지토리에서 관리하고 이를  운영 환경 간의 상태 차이가 없도록 유지하는 방식을 말한다. 즉, 배포할 쿠버네티스 리소스를 Git 리포지토리에 올려놓으면 실제 운영 중인 클러스터와 차이가 발생하게 되고 이를 감지하고 클러스터에 동기화해주는 어플리케이션이 동기화(Sync)를 수행하여 지속적으로 배포가 이루어지도록 하는  방식이다.

* GitHub 등의 웹 UI에 들어가면 배포된 상태를 편하게 확인할 수 있고 단일 진실 원천(Single source of truth)으로 신뢰성도 담보된다. 또한, Git 커밋을 통해 관리되기 때문에 별도 설정이나 시스템 구축 없이도 배포 이력이나 감사 로그를 쌓을 수 있다. 롤백도 Git의 특정 리비전을 참고하여 동기화하는 것이라, 기존 배포 결과가 OK 상태였다고 하면 해당 리비전으로 롤백 했을 때 배포에 문제가 발생하지 않는다.
* 핵심은 git repo에 저장된 쿠버네티스 매니페스트 같은 파일을 이용해 배포를 선언적으로 하는 것!
* IaC와 다른 점은? IaC가 더 상위 개념으로 모든 인프라를 관리하기 위한 패턴이며, GitOps는 git이라는 특화된 소스 버전 관리 도구를 기반으로 kubernetes의 자원을 관리하는 목적으로 세분화된 기술임


|       | GitOps             | IaC                     |
|-------|--------------------|-------------------------|
| 관리대상| Kubernetes 리소스들   |  모든 인프라(local,Cloud,Kubernetes)    |
| 원천소스| git |  git 외에도 설정 저장가능 |
| 개념   | IaC 중 Kubernetes에 특화된 패턴     | 인프라를 관리하는 패턴    |
| 도구   | argocd, flux  |  terraform, ansible, chef 등    |

---
## GitOps 배포방식
* 기존의 배포방식(Push-based Deployment)
![기존의 배포방식](/cicd/assets/normal_cicd_flow.png)

* GitOps 배포방식(Pull-based Deployment)
![기존의 배포방식](/cicd/assets/gitops_cicd_flow.png)## Tools

## Tools
* [FluxCD, ArgoCD or Jenkins X: Which Is the Right GitOps Tool for You?](https://blog.container-solutions.com/fluxcd-argocd-jenkins-x-gitops-tools)
* [Enterprise grade CI/CD with GitOps](https://blog.container-solutions.com/gitops-case-study)

------


## GitOps WorkFlow
* Jenkins(push 방식)
* ArgoCD
![ArgoCD](/cicd/assets/gitops-argocd.png)
* FluxCD(with helm, kustomize)
![Flux with helm kustomize](/cicd/assets/gitops-flux-helm-kustomize.png)
* Auto-Image Update
![Auto-Image Update](/cicd/assets/gitops-auto-image-update.png)
* Automated Canary Deployment
![Automated Canary Deployment](/cicd/assets/gitops-automated-canary-deployment.png)
* CDN, DNS...
    * https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn/blob/master/README.md
* Terraform
* Kubernetes

- Tool 별 Pros and Cons
- GitOps의 한계

https://ruzickap.github.io/k8s-flux-istio-gitlab-harbor
https://github.com/ruzickap/k8s-flagger-istio-flux/blob/master/docs/part-05/tekton-dashboard.png
## Reference
* [GKE(Google Kubernetes Engine) GitOps release model](https://medium.com/contino-engineering/building-cloud-native-gitops-on-google-cloud-platform-21e022904e94)
    * pre-requisites: Google Kubernetes Engine, Cloud Build, and good ol’ GitHub

* [FINDA GitOps 적용](https://medium.com/finda-tech/finda-msa%EB%A5%BC-%EC%9C%84%ED%95%9C-kubernetes-%EC%84%B8%ED%8C%85%EA%B3%BC-ci-cd-pipeline-%EA%B5%AC%EC%84%B1-%EA%B7%B8%EB%A6%AC%EA%B3%A0-monitoring-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-2-ef29380ec474)

* [Admission Controller](https://faun.pub/workflow-for-kubernetes-devops-15f0dbb560ff)

* [CI/CD workflow using GitOps - Azure Arc enabled Kubernetes](https://docs.microsoft.com/en-us/azure/azure-arc/kubernetes/conceptual-gitops-ci-cd)
    * ![Azure ARC GitOps Arch](/cicd/assets/azure-arc-gitops-arch.png)


* GitOps at Adobe
    * https://www.youtube.com/watch?v=CU9AseP_DOI
    * https://www.cloudbees.com/videos/gitops-kubernetes-adobe
    * 장표: https://carlossg.github.io/presentations/2020-05_gitopsdays/#/

### 
* https://github.com/selkies-project/selkies

## 참고
* [Continuous GitOps, the way to do DevOps in Kubernetes](https://itnext.io/continuous-gitops-the-way-to-do-devops-in-kubernetes-896b0ea1d0fb)
* [Blue Green deployment with Flagger, Flux and Istio](https://github.com/ruzickap/k8s-flagger-istio-flux)
* [Infrastructure as Code Vs. GitOps](https://croz.net/news/infrastructure-as-code-vs-gitops/)
* https://dev.to/alcide/gitops-a-security-perspective-part-1-16ci
