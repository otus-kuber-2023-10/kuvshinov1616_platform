
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