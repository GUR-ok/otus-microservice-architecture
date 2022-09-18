### Домашнее задание №5. Authorization

Реализована авторизации по jwt через Istio.

### Описание приложения:

![img.png](img.png)
![sequence_diag.png](sequence_diag.png)
1) Компоненты приложения:

- Сервис Аутентификации (СА) https://github.com/GUR-ok/arch-auth https://hub.docker.com/repository/docker/gurok/arch_auth 
- Сервис Управления Профилем (СУП) https://github.com/GUR-ok/arch-profiles https://hub.docker.com/repository/docker/gurok/arch_profiles
- настроенный Istio API-gateway с проверкой авторизации по jwt.
- БД: каждый микросервис подключен к своей БД:
  СА хранит данные юзеров (userId, login, password, profileId), СУП хранит данные профиля (profileId, fullName, age,
  ...).
2) Запросы на /auth/ не требуют авторизации (перенаправляются на СА), остальные запросы требуют передачи валидного jwt (
   перенаправляются на СУП).
3) Пользователю доступны API /auth/register, /auth/login, а также API управления Профилем с доступом по токену
   авторизации.
4) При первичной регистрации пользователя на /auth/register СА обращается к СУП для создания профиля. Полученный
   profileId привязывается к userId.
5) После регистрации клиент может залогиниться на /auth/login, в ответ получит подписанный сервисом аутентификации jwt.
   Jwt содержит id профиля profileId (из сервиса управления Профилем), созданного при регистрации.
6) Сервис аутентификации СА имеет пару ключей (хранятся в jks): приватным ключом подписывается jwt; публичный ключ
   открытый, предоставляется по адресу /auth/.well-known/jwks.json
7) Istio ingressgateway настроен на проверку валидности подписи jwt при помощи публичного ключа. 
   При валидном jwt полезные данные из jwt направляются header'ом "x-jwt-token" в микросервис.
8) При запросе изменения профиля в СУП проверяется profileId из jwt на совпадение с id запрашиваемого профиля. В случае
   попытки запроса чужого профиля запрос считается неавторизованным. Верификация и проверка подписи jwt в СУП не
   производится, за верификацию отвечает Istio.

#### Инструкция по запуску:

- `minikube start`
- `kubectl delete namespace ingress-nginx`
- `kubectl delete ingressClass nginx`  
- `kubectl create namespace arch-gur`
- `istioctl install --set profile=demo -y`
- `istioctl manifest apply -f ./hw5/istio/istio-values.yaml`
- `helm install gorelov-arch-hw5 ./hw5/istio/`
- `helm install gorelov-arch-auth ./hw5/auth_deployment/`
- `helm install gorelov-arch-profiles ./hw5/profiles_deployment/`

#### Тесты:

- `newman run ./hw5/gorelov_hw_5.postman_collection.json --verbose`

#### Проверка портов и istio:

- `kubectl get virtualService`
- `kubectl get svc -n istio-system`
  (должен быть порт 30001)

---
#### Очистка пространства:

- `helm uninstall gorelov-arch-hw5`
- `helm uninstall gorelov-arch-auth`
- `helm uninstall gorelov-arch-profiles`
- `istioctl x uninstall --purge`
- `kubectl delete namespace arch-gur`
- `kubectl delete namespace istio-system` 

#### Результаты тестов:
![nm_1.png](nm_1.png)
![nm_2.png](nm_2.png)
![nm_3.png](nm_3.png)
![nm_4.png](nm_4.png)
![nm_5.png](nm_5.png)
![nm_6.png](nm_6.png)
