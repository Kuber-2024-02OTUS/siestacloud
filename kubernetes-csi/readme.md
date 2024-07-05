# Конфигурация системы Vault

## Содержание

- [О проекте](#ee)
- [полезные команды](#p)




## 🧐 О проекте <a name = "ee"></a>

- ./csi-s3
    - - содержит helm чарт для деплоя CSI в k8s 

- ./pod
    - содержит определение манифеста pod который монтирует volume? Данных фактически лежат на S3

- ./pvc
    - содержит определение манифеста pvc который использует storage class CSI S3 провайдера
    

## 🧐 Полезные команды <a name = "p"></a>

```
export HELM_EXPERIMENTAL_OCI=1 && \
helm pull oci://cr.yandex/yc-marketplace/yandex-cloud/csi-s3/csi-s3 \
  --version 0.35.5 \
  --untar && \
helm install \
  --namespace kube-system \
  --set secret.accessKey=<идентификатор_ключа> \
  --set secret.secretKey=<секретный_ключ> \
  csi-s3 .
```