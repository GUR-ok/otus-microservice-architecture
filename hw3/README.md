Домашнее задание №3

Инструкция по запуску и установке:

1) Установить в кластере prometheus:
   - kubectl create namespace prometheus
   - helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   - helm repo update 
   - helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus -f ./hw3/stack-values.yaml -f ./hw3/prometheus.yaml

2) Развернуть приложение в кластере:
   В корне проекта выполнить:
   - helm install gorelov-arch-hw3 ./hw3

3) Для очистки пространтсва выполнить:
   - helm uninstall gorelov-arch-hw3
   - helm uninstall prometheus -n prometheus

Для доступа к UI prometheus:
- kubectl port-forward -n m service/prometheus-operated 9090:9090

Для доступа к UI grafana:
- kubectl port-forward -n m service/prometheus-hw03-grafana 9000:80 login: admin password: prom-operator

Если нет метрик nginx:
- helm upgrade --install nginx ingress-nginx/ingress-nginx --namespace m --set controller.stats.enabled=true -f nginx-ingress.yaml 

Проверить конфиг nginx и prometheus:
- helm get values nginx -n m
- helm get values prometheus -n prometheus