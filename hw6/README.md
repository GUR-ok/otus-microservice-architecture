### Домашнее задание №6

- 'minikube start'
- 'kubectl create namespace arch-gur'  
- Использовать nginx ingress controller установленный через хелм, а не встроенный в minikube:
'kubectl delete namespace ingress-nginx'
'kubectl delete ingressClass nginx'
'kubectl create namespace ng && helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/ && helm repo update && helm install nginx ingress-nginx/ingress-nginx --namespace ng -f nginx-ingress.yaml'
- 'helm install gorelov-redis ./hw6/redis/'
- 'helm install gorelov-arch-order ./hw6/order-deployment/'

---

### Очистка пространства:

- 'helm uninstall gorelov-arch-order'
- 'helm uninstall gorelov-redis'
- 'helm uninstall nginx'
- 'kubectl delete namespace arch-gur'
- 'kubectl delete namespace ng'