
# Выполнение ДЗ CRD

## Содержание

- [О проекте](#about)

## 🧐 О проекте <a name = "about"></a>

Описан CRD `mysqls.otus.homework`
Описан Deployment `mysql-operator-deployment` который поднимает приложение - оператор, которое следит за кастомными ресурсами с kind `MySQL`
Описаны sa cr crb с нимимально необходимыми привилегиями для поднятия ресурсов типа deployment svc pvc и pv в кластере
