# Конфигурация системы Vault

## Содержание

- [О проекте](#ee)
- [полезные команды](#p)




## 🧐 О проекте <a name = "ee"></a>

- ./consul
    - содержит директиву файла values.yaml для установления количенства реплик и дебаг режима

- ./external-secret
    - содержит определение манифеста crd объекта ExternalSecret c параметрами из задания

- ./policy
    - содержит описание политики которая используется ролью `auth/kubernetes/role/otus`
    
- ./role
    - содержит описание роли `auth/kubernetes/role/otus` и порядок её создания 

- ./sa
    - содержит определение манифестов ServiceAccount, Secret Auth Token и ClusterRoleBinding

- ./sa
    - содержит определение манифеста SecretStore c параметрами из задания
  
- ./vault
    - содержит helm чарт для деплоя vault в k8s 


## 🧐 Полезные команды <a name = "p"></a>

```
vault write auth/kubernetes/config \
  token_reviewer_jwt="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \
  kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443" \
  kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
```