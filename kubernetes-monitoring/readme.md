# Kubernetes monitoring

- [О проекте](#about)

## 🧐 О проекте <a name = "about"></a>

Использование prometheus operator, CRD ServiceMonitor и Nginx Exporter в роли дополнительного контейнера в рамках одного пода

Манифест ServiceMonitor лежит [ТУТ](./prometheus-operator/templates/svc-monitor.yaml)

> Команды для проверить helm чарта
```
helm dependency update ./kubernetes-monitoring/prometheus-operator
helm install ./kubernetes-monitoring/prometheus-operator --generate-name -n monitoring
```