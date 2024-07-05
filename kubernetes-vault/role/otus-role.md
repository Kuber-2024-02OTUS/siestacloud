Для включания key/value хранилища нужно:
```
vault secrets enable -version=2 -path=secret kv
```

>Создаем сам секрет
```bash
vault kv put secret/apps/service username=otus
vault kv put secret/apps/service password=asajkjkahs’
```

> необходимо добавить новый метод аутентификации
```bash
vault auth enable kubernetes
```

```bash
vault write auth/kubernetes/config \

token_reviewer_jwt="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \

kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443" \

kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
``` 

> Далее создаём роль которая будет использовать этот метод аутентификации
```bash
vault write auth/kubernetes/role/otus \
bound_service_account_names=vault-auth \
bound_service_account_namespaces=vault \
policies=otus-policy \
ttl=72h
```