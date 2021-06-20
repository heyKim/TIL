* 
CICD란
-----
GitOps란
* ![GitOps Concept](/cicd/assets/gitops-concept.jpeg)
* IaC와 다른 점은? IaC가 더 상위 개념으로 모든 인프라를 관리하기 위한 패턴.


---
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
