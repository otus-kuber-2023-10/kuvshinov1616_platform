# kuvshinov1616_platform
kuvshinov1616 Platform repository

 # Выполнено ДЗ № 1

все pod в namespace kube-system восстановились, т.к. etcd, kube-apiserver, kube-controller-manager,
kube-scheduler являются static подами и управляются непосредственно кубелетом, при удалении он их снова запускает
coredns описан в deployment, в котором указано количество реплик, которое должно быть запущенно
kube-proxy описан в daemonset, т.е. должен быть запущен на каждой ноде кластера

frontend находился в статусе error, т.к. не были заданы переменные окружения
`panic: environment variable "PRODUCT_CATALOG_SERVICE_ADDR" not set`

В процессе выполнения работы сделано: 
 настроено рабочее окружение (kubectl, minikube)
 Написан Dockerfile и собран контейнер с веб серевером
 Созданы манифесты подов
 Запущены контейнеры в minikube
 Работа с утилитой kubectl

 # Выполнено ДЗ № 2

 - [x] Основное ДЗ
 - [x] Задание со *
 - [x] Задание со **

## В процессе сделано:
 - Запущен кластер kind
 - Создан манифест ReplicaSet для микросервиса frontend
 - Создан манифест Deployment для микросервиса paymentservice
 - Создан манифест Deployment paymentservice-deployment-bg.yaml со стратегией развертывания аналогичной  blue-green
 - Создан манифест Deployment paymentservice-deployment-reverse.yaml c Reverse Rolling Update
 - Создан манифест Deployment frontend-deployment.yaml для микросервиса frontend, содержащий readinessProbe
 - Создан манифест Daemonset nodeexporter-daemonset.yaml для развертывания Node Exporter
 - Скорректирован Daemonset, позволяющий запускаться Node Exporter как на worker, так и на master нодах

## PR checklist:
 - [x] Выставлен label с темой домашнего задания