Домашнее задание №6

<u>Описание приложения:</u>

![img.png](img.png)
![sequence_diag.png](sequence_diag.png)
1) Компоненты приложения:

- Сервис Аутентификации (СА) https://github.com/GUR-ok/arch-auth https://hub.docker.com/repository/docker/gurok/arch_auth 
- Сервис Управления Профилем (СУП) https://github.com/GUR-ok/arch-profiles https://hub.docker.com/repository/docker/gurok/arch_profiles_2
- настроенный Istio API-gateway с проверкой авторизации по jwt.
- БД: каждый микросервис подключен к своей БД:
  СА хранит данные юзеров (userId, login, password, profileId), СУП хранит данные профиля (profileId, fullName, age,
  ...).
- Брокер сообщений Kafka

<u>Инструкция по запуску:</u>

- minikube start
- kubectl delete namespace nginx-ingress
- kubectl create namespace arch-gur
- helm install gorelov-kafka ./hw6/kafka/
- istioctl install --set profile=demo -y
- istioctl manifest apply -f ./hw6/istio/istio-values.yaml
- helm install gorelov-arch-hw6 ./hw6/istio/
- helm install gorelov-arch-auth ./hw6/auth_deployment/
- helm install gorelov-arch-profiles ./hw6/profiles_deployment/

<u>Диагностика, проверка портов и istio:</u>

- kubectl get virtualService
- kubectl get svc -n istio-system
  (должен быть порт 30001)
- kubectl get svc -n arch-gur
  kafka-manager                 NodePort    10.101.112.105   <none>        9000:30170/TCP
  Для входа в kafka-manager http://arch.homework:30170/
- kubectl port-forward -n arch-gur arch-profiles-deployment-67d58c5b57-x25q4 8080:8000
- http://localhost:8080/swagger-ui/index.html#/kafka-controller/sendString
- kubectl logs -f -n arch-gur arch-profiles-deployment-67d58c5b57-x25q4  
---
<u>Очистка пространства:</u>

- helm uninstall gorelov-arch-hw6
- helm uninstall gorelov-arch-auth
- helm uninstall gorelov-arch-profiles
- istioctl x uninstall --purge
- kubectl delete namespace arch-gur
- helm uninstall gorelov-kafka

