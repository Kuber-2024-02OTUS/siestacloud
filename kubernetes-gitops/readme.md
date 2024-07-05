
# Выполнение ДЗ Kubernetes-gitops

## Содержание

- [О проекте](#about)

## 🧐 О проекте <a name = "about"></a>


Выставить taint и label на инфра-ноду kubectl label nodes minikube-m02 infraNode=true kubectl taint nodes minikube-m02 node-role=infra:NoSchedule
Выставить label на какую-либо ноду для запуска подов из kubernetes-networks kubectl label nodes minikube-m03 homework=true


Описан Helm чарт gitops системы argocd
```
helm repo add argo https://argoproj.github.io/argo-helm
helm dependency update argocd/
helm install -n argocd argocd argocd/
```


