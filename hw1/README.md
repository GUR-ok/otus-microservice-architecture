### Домашнее задание №1. HealthCheck

В кластере разворачивается nginx и простое приложение с healthcheck.

#### Инструкция по запуску:

Использовать nginx ingress controller установленный через хелм, а не встроенный в minikube:
- `kubectl delete namespace ingress-nginx`
- `kubectl delete ingressClass nginx`
- `kubectl create namespace m && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace m -f nginx-ingress.yaml`
Убедиться, что в etc/hosts прописан адрес из команды minikube ip
  192.168.59.100 arch.homework
  
Вариант 1 (используя Helm)
1) В корне проекта выполнить: `helm install gorelov-arch-hw ./hw1`
2) Подождать поднятие подов и вызвать: `curl http://arch.homework/health && curl http://arch.homework/otusapp/yk_gorelov/health`
3) Для очистки пространтсва выполнить: 
   - `helm uninstall gorelov-arch-hw`
   - `kubectl delete namespace m`
   - `helm uninstall nginx -n m`
  
Вариант 2
1) Выполнить в директории /hw1/templates: `kubectl apply -f deployment.yaml -f service.yaml -f ingress.yaml`
2) Подождать поднятие подов и вызвать: `curl http://arch.homework/health && curl http://arch.homework/otusapp/yk_gorelov/health`
3) Для очистки пространтсва выполнить: 
   - `kubectl delete -f deployment.yaml -f service.yaml -f ingress.yaml`
   - `kubectl delete namespace m`
   - `helm uninstall nginx -n m`

- В случае ошибки при делое приложения через helm

  Error: INSTALLATION FAILED: Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": Post "https://ingress-nginx-controller-admission.ingress-nginx.svc:4
  43/networking/v1/ingresses?timeout=10s": dial tcp 10.111.50.42:443: connect: connection refused

  необходимо выполнить:
    ```
    get ValidatingWebhookConfiguration
    kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
    ```
