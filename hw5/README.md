Домашнее задание №5

Инструкция по запуску:
- istioctl install --set profile=demo -y
- istioctl manifest apply -f ./hw5/istio-values.yaml
- helm install gorelov-arch-hw5 ./hw5
  
Проверка портов и istio:
  - kubectl get virtualService
  - kubectl get svc -n istio-system
    (должен быть порт 30001)
    
---
- helm uninstall gorelov-arch-hw5 
- istioctl x uninstall --purge