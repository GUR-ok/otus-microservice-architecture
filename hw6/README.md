### Домашнее задание №6. Idempotency

Релизовать идемпотентный метод создания заказа.

#### Описание приложения:
Приложение сосотоит из:
- Сервис Управления Заказом.
- Реляционная БД PostgreSQL
- Redis для хранения ключей идемпотентности

#### Инструкция по запуску:
- `minikube start`
- `kubectl create namespace arch-gur`  
- Использовать nginx ingress controller установленный через хелм, а не встроенный в minikube:

```
kubectl delete namespace ingress-nginx
kubectl delete ingressClass nginx
kubectl create namespace m && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace m -f nginx-ingress.yaml
```

- `helm install gorelov-redis ./hw6/redis/`
- `helm install gorelov-arch-order ./hw6/order_deployment/`

---

#### Проверка и отладка:
- Доступ к редис:
`kubectl port-forward -n arch-gur redis-order-statefulset-0 6379:6379`

- В случае ошибки при делое приложения через helm

    Error: INSTALLATION FAILED: Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": Post "https://ingress-nginx-controller-admission.ingress-nginx.svc:4
    43/networking/v1/ingresses?timeout=10s": dial tcp 10.111.50.42:443: connect: connection refused
    
    необходимо выполнить:
    ```
    get ValidatingWebhookConfiguration
    kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
    ```

#### Очистка пространства:

- `helm uninstall gorelov-arch-order`
- `helm uninstall gorelov-redis`
- `helm uninstall nginx -n m`
- `kubectl delete namespace arch-gur`
- `kubectl delete namespace m`