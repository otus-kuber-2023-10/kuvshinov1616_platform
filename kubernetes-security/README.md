# Выполнено ДЗ № 5

 - [x] Основное ДЗ
 - [ ] Задание со *

## В процессе сделано:
task-01:
01-sa-bob-admin.yaml - создает service account bob, с ролью admin в рамках всего кластера
02-sa-dave.yaml - создает service account dave без доступа в кластер.

task-02:
01-prometheus-namespace.yaml - создает namespace prometheus
02-sa-carol.yaml - создает service account carol в namespace prometheus
03-pods-get-list-watch.yaml - дает права всем sa ns prometheus делать list,get,watch на pods

task-03:
01-dev-namespace.yaml - создает namespace dev
02-sa-jane.yaml - создает service account jane в namespace dev, дает jane роль admin в namespace dev
03-sa-ken.yaml - создает service account ken в namespace dev, дает ken роль view в namespace dev
