Домашнее задание №3

Инструкция по запуску и установке:
1) Установить в кластере prometheus:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus-hw03 prometheus-community/kube-prometheus-stack -f .hw3/stack-values.yaml

2) Развернуть приложение в кластере:
В корне проекта выполнить: helm install gorelov-arch-hw3 ./hw3

3) Для очистки пространтсва выполнить:
helm uninstall gorelov-arch-hw3
helm uninstall prometheus-hw03

Для доступа к UI prometheus:
kubectl port-forward service/prometheus-operated 9090:9090