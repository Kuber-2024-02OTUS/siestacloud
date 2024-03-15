# Выполнено ДЗ №1

 - [ ] В данном домашнем задании вы научитесь формировать локальное окружение, запустят локальную версию kubernetes при помощи minikube, научатся использовать CLI утилиту kubectl для управления kubernetes.

## В процессе сделано:
 - Установлен Yandex k8s
 - Создал namespace.yaml
 - Создал pod.yaml
 - Приминил конфиг: kubectl apply -f namespace.yaml  -f pod.yaml  
 - Пробросил порт: kubectl port-forward pod/web -n homework  80:8000
 - Проверил что файл скачивается wget   127.0.0.1:80/index.html
 - Проверил что файл скачен: ls -la | grep index.html

## Как запустить проект:
 - Выполнить kubectl apply -f namespace.yaml -f pod.yaml  siestacloud\kubernetes-intro

## Как проверить работоспособность:
 - Пробросил порт: kubectl port-forward pod/web -n homework  80:8000
 - Проверил что файл скачивается wget   127.0.0.1:80/index.html
 - Проверил что файл скачен: ls -la | grep index.html
## PR checklist:
 - [] Выставлен label с темой домашнего задания


## Платформа выполнения:
OS: Ubuntu 22.04
Yandex K8s