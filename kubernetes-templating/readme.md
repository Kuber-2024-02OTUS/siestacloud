# Kubernetes templating

- [О проекте](#about)

## 🧐 О проекте <a name = "about"></a>

Шаблонизация манифестов приложения, использование Helm, helmfile. Установка community Helm charts

Основные переменные заданы в файле values.yaml

> Команды для проверить helm чарта
```
helm dependency update ./kubernetes-templating
helm install ./kubernetes-templating --dry-run --generate-name -n infra-dev
```
> В директории kafka лежат файлы по заданию №2