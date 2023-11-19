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