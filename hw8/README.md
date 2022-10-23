### Домашнее задание №8. Distributed transactions

Релизовать распределенную транзакцию в микросервисной архитектуре

---
![img.png](img.png)

#### Описание приложения:
Приложение сосотоит из:
- Интерцессор (Сервис Оркестратор) 

...todo

---

#### Инструкция по запуску:
- `minikube start`
- `kubectl create namespace arch-gur`
- Использовать nginx ingress controller установленный через хелм, а не встроенный в minikube:

  ```
  kubectl delete namespace ingress-nginx
  kubectl delete ingressClass nginx
  kubectl create namespace m && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace m -f nginx-ingress.yaml
  ```
  
- `helm install gorelov-kafka ./hw8/kafka/`
- `helm install gorelov-intercessor ./hw8/intercessor_deployment/`
  `kubectl get pods -n arch-gur`
- В случае ошибки при деплое приложения через helm

  Error: INSTALLATION FAILED: Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": Post "https://ingress-nginx-controller-admission.ingress-nginx.svc:4
  43/networking/v1/ingresses?timeout=10s": dial tcp 10.111.50.42:443: connect: connection refused

  необходимо выполнить:
    ```
    kubectl get ValidatingWebhookConfiguration
    kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
    ```  
- дождаться поднятия подов

---

#### Тесты:

- `newman run ./hw8/gorelov_hw_8.postman_collection.json --verbose`

#### Результаты тестов:

...todo

---

#### Проверка и отладка:
- Port-forward:
  `kubectl get pods -n arch-gur`
  `kubectl port-forward -n arch-gur arch-intercessor-deployment-76548647fd-bpxbj 8000:8000`
  `kubectl port-forward -n arch-gur arch-intercessor-postgresql-deployment-0 5432:5432`

#### Очистка пространства:

- `helm uninstall gorelov-intercessor`
- `helm uninstall gorelov-kafka`
- `helm uninstall nginx -n m`
- `kubectl delete namespace arch-gur`
- `kubectl delete namespace m`