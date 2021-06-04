# ArgoCD

## ArgoCD 설치
```bash
# kubectl get po
NAME                                  READY   STATUS    RESTARTS   AGE
argocd-application-controller-0       1/1     Running   0          55m
argocd-dex-server-9dc558f5-nbkxv      1/1     Running   0          55m
argocd-redis-759b6bc7f4-gxwzj         1/1     Running   0          55m
argocd-repo-server-5fbf484547-lkmsx   1/1     Running   0          55m
argocd-server-6d4678f7f6-l97zs        1/1     Running   0          55m
guestbook-ui-85985d774c-xp8q2         1/1     Running   0          23m
```
```bash
# kubectl get svc
NAME                    TYPE           CLUSTER-IP       EXTERNAL-IP                                                                    PORT(S)                      AGE
argocd-dex-server       ClusterIP      10.100.31.122    <none>                                                                         5556/TCP,5557/TCP,5558/TCP   56m
argocd-metrics          ClusterIP      10.100.170.9     <none>                                                                         8082/TCP                     56m
argocd-redis            ClusterIP      10.100.183.130   <none>                                                                         6379/TCP                     56m
argocd-repo-server      ClusterIP      10.100.127.28    <none>                                                                         8081/TCP,8084/TCP            56m
argocd-server           LoadBalancer   10.100.172.1     a2dcee8a4ed354e8491cdcda46a9e08e-1826405691.ap-northeast-2.elb.amazonaws.com   80:32191/TCP,443:31538/TCP   56m
argocd-server-metrics   ClusterIP      10.100.7.66      <none>                                                                         8083/TCP                     56m
guestbook-ui            ClusterIP      10.100.42.81     <none>                                                                         80/TCP                       23m
```

```bash
# kubectl get serviceaccounts
NAME                            SECRETS   AGE
argocd-application-controller   1         53m
argocd-dex-server               1         53m
argocd-redis                    1         53m
argocd-server                   1         53m
default                         1         56m
```
```bash
# kubectl get secret
NAME                                                                                       TYPE                                  DATA   AGE
argocd-application-controller-token-8prx9                                                  kubernetes.io/service-account-token   3      54m
argocd-dex-server-token-dhgf6                                                              kubernetes.io/service-account-token   3      54m
argocd-initial-admin-secret                                                                Opaque                                1      53m
argocd-redis-token-75z7l                                                                   kubernetes.io/service-account-token   3      54m
argocd-secret                                                                              Opaque                                5      54m
argocd-server-token-j6zd5                                                                  kubernetes.io/service-account-token   3      54m
cluster-693dd95ee85ca8ef4a2b5452df30b219.sk1.ap-northeast-2.eks.amazonaws.com-1969336033   Opaque                                3      25m
default-token-rm5hn                                                                        kubernetes.io/service-account-token   3      57m
```

```bash
# argocd cluster add CONTEXTNAME
# kubectl get secret -n kube-system
NAME                                                       TYPE                                  DATA   AGE
secret/argocd-manager-token-gfv28                          kubernetes.io/service-account-token   3      21m
```

```bash
# kubectl get cm
NAME                        DATA   AGE
argocd-cm                   0      61m
argocd-gpg-keys-cm          0      61m
argocd-rbac-cm              0      61m
argocd-ssh-known-hosts-cm   1      61m
argocd-tls-certs-cm         0      61m
```