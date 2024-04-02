# Предоставление пользовательского доступа для взаимодействия с кластером

## Содержание


- [Важно знать](#dforget)
- [Доступ K8s Api снаружи кластера (обычный пользователь)](#df)
- [Доступ K8s Api изнутри кластера (приложения)](#d)

## 🧐 Важно знать <a name = "dforget"></a>

Всё взаимодействие с K8s сводится к запросам на API. Эти запросы могут инициироваться как обычными пользователями (`kubectl`) так и приложениями.
K8s Api предоставляет доступ к тем или иным ресурсам на основе:
- **Клиентского сертификата** в полях которого должны быть указаны `CN=<пользователь> O=<группа>/`. Сертификат должен быть подписан корневым сертификатом K8s
- **JWT токена** который подписан корневым сертификатом кластера, в нём так-же указан название `service account` (имя пользователя) и `namespace` где лежит этот `sa`


**Service Account** 
- Просто описывает конкретного пользователя. Был создан в первую очередь для ограничения прав. Взаимодействие с K8s происходит посредством токенов или клиентских сертификатов, в которых указывается имя конкретного `service account`. Дело в том, что Kubernetes ничего не знает о пользователях в том виде, в котором мы привыкли их видеть в других системах ограничения доступа, где у пользователей есть логин, пароль, группы и т.д. То есть внутри своих объектов Kubernetes никаких списков логинов, групп и паролей не хранит.

**Роли**
- `Role` 

  Это YAML-манифест, который описывает некий набор прав на объекты кластера. Role ничего и никому не разрешает. Это просто список.
  Описывает права в ns, сущность Role неймспейс-зависимая, и мы можем создавать роли с одинаковым именем в разных неймспейсах.
  Назначением роли занимается другой ресурс,  `RoleBinding` 

- `ClusterRole`

  Это кластерный объект, эта сущность описывает права на объекты во всём кластере.

**Биндинги**

- `RoleBinding`

  Назначает роль конкретным sa в рамках ns т.е. даёт доступ только к тем сущностям, которые находятся в том же ns

- `ClusterRoleBinding`

  Назначает роль конкретным sa во всех неймспесах кластера сразу.



## 🧐 Доступ K8s Api снаружи кластера (обычный пользователь) <a name = "df"></a>

Доступ к кластеру снаружи обычно предоставляется на основе файла `~/.kube/kubeconfig`  в котором указываются
- перечень кластеров
- перечень аккунтов (с указанием токено или сертификатов)
- контексты подключения
В зависимости от выбранного контекста, клиент подключается к конкретному кластеру под конкретным аккаунтом. За проверку возможности выполнить то или иной действие отвечает модуль авторизации `RBAC`, который аутентифицирует аккаунт и авторизирует его посредством роли, которая связана с этим аккаунтом. 
Если в роли указаны разрешение на выполнения действия пользователя, то тогда авторизация считается успешно выполненой.

Пользователи могут подключаться к API кластера на основе:
- **Клиентского сертификата и ключа** которые фактически указываются в файле `~/.kube/kubeconfig` (так-же можно указать просто пути до них) 
- **Клиентского токена**, который генерируется автоматически в момент создания секрета с типом `kubernetes.io/service-account-token`. Такой тип Секретов ссылается на имя опеределённого `service account`. Этот `service account` идентифицирует конкретного пользователя и биндится с заранее настроенной ролью. Далее этот токен может использоваться как обычным пользователем в файле `~/.kube/kubeconfig`

В контексте нашего кластера, мы используем стандартную кластерную роль edit и назначаем её юзерам, тем самым раздаём им доступы в нужные неймспейсы. Достигаем мы этого путём привязки ClusterRole к RoleBinding в конкретном неймспейсе 


```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: user-app
  namespace: app

---
apiVersion: v1
kind: Secret
metadata:
  name: usr-app-token
  namespace: app
  annotations:
    kubernetes.io/service-account.name: user-app
type: kubernetes.io/service-account-token

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: user-app-rb
subjects:
- kind: ServiceAccount
  name: user-app
  namespace: app
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: edit
```




## 🧐 Доступ K8s Api изнутри кластера (приложения) <a name = "d"></a>

> Secret с этим токеном монтируется внутрь контейнера, где работает приложение, и если приложение знает и умеет в Kubernetes, оно берёт токен из файла `/var/run/secrets/kubernetes.io/serviceaccount/token`, смонтированного из secret, и с ним делает запросы в API K8s. 



## Создание kubeconfig
```
SERVICE_ACCOUNT_NAME=cd
SECRET_NAME=${SERVICE_ACCOUNT_NAME}-token
CONTEXT=$(kubectl config current-context)
NAMESPACE=users

NEW_CONTEXT=${SERVICE_ACCOUNT_NAME}-context
KUBECONFIG_FILE="kubeconfig-${SERVICE_ACCOUNT_NAME}"

TOKEN=`kubectl -n ${NAMESPACE} get secret ${SECRET_NAME} -o jsonpath='{.data.token}' | base64 --decode`

kubectl config view --raw > ${KUBECONFIG_FILE}.full.tmp
kubectl --kubeconfig ${KUBECONFIG_FILE}.full.tmp config use-context ${CONTEXT}
kubectl --kubeconfig ${KUBECONFIG_FILE}.full.tmp config view --flatten --minify > ${KUBECONFIG_FILE}.tmp
kubectl config --kubeconfig ${KUBECONFIG_FILE}.tmp rename-context ${CONTEXT} ${NEW_CONTEXT}
kubectl config --kubeconfig ${KUBECONFIG_FILE}.tmp set-credentials ${SERVICE_ACCOUNT_NAME} --token ${TOKEN}
kubectl config --kubeconfig ${KUBECONFIG_FILE}.tmp set-context ${NEW_CONTEXT} --user ${SERVICE_ACCOUNT_NAME}
kubectl config --kubeconfig ${KUBECONFIG_FILE}.tmp view --flatten --minify > ${KUBECONFIG_FILE}
rm ${KUBECONFIG_FILE}.full.tmp
rm ${KUBECONFIG_FILE}.tmp
```