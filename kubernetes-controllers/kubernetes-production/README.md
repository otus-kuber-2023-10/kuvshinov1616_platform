ДЗ. Подходы к развертыванию и обновлению production-grade кластера

Установка кластера с помощью kubeadm

создаем ноды в ycloud, производим настройки согласно интструкции


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.131.0.7:6443 --token 0zsxdd.d6y03tnf3gh4rjd1 \
        --discovery-token-ca-cert-hash sha256:152cebe967e95c13eb139a08be56db589c5550a392cb34bef5cccf223937b9b7




admin@master:~$ kubectl cluster-info
Kubernetes control plane is running at https://10.131.0.21:6443
CoreDNS is running at https://10.131.0.21:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy


admin@master:~$ kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
namespace/kube-flannel created
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.apps/kube-flannel-ds created



Добавляем ноды в кластер


 kubeadm join 10.131.0.7:6443 --token 0zsxdd.d6y03tnf3gh4rjd1 \
        --discovery-token-ca-cert-hash sha256:152cebe967e95c13eb139a08be56db589c5550a392cb34bef5cccf223937b9b7

admin@master:~$ kubectl get nodes
NAME      STATUS   ROLES           AGE   VERSION
master    Ready    control-plane   84m   v1.28.9
worker1   Ready    <none>          58m   v1.28.9
worker2   Ready    <none>          57m   v1.28.9
worker3   Ready    <none>          57m   v1.28.9

Запуск нагрузки

kubectl apply -f deploy.yaml

admin@master:~$ kubectl get pod
NAME                               READY   STATUS    RESTARTS      AGE
nginx-deployment-cd5968d5b-5hc9r   1/1     Running   0             52s
nginx-deployment-cd5968d5b-b7t5g   1/1     Running   0             52s
nginx-deployment-cd5968d5b-ck9tz   1/1     Running   1 (45s ago)   52s
nginx-deployment-cd5968d5b-fnp8d   1/1     Running   0             52s


Обновление кластера


[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.29.4". Enjoy!


NAME      STATUS   ROLES           AGE   VERSION
master1   Ready    control-plane   72m   v1.29.4
worker1   Ready    <none>          29m   v1.28.9
worker2   Ready    <none>          27m   v1.28.9
worker3   Ready    <none>          25m   v1.28.9


Вывод worker-нод из планирования

 kubectl drain worker1 --ignore-daemonsets
node/worker1 cordoned
Warning: ignoring DaemonSet-managed Pods: kube-flannel/kube-flannel-ds-wqg6p, kube-system/kube-proxy-vzc74
evicting pod default/nginx-deployment-cd5968d5b-pr622
evicting pod default/nginx-deployment-cd5968d5b-mv2z2
pod/nginx-deployment-cd5968d5b-pr622 evicted
pod/nginx-deployment-cd5968d5b-mv2z2 evicted
node/worker1 drained


Обновление worker-нод

 kubectl get node
NAME      STATUS                     ROLES           AGE   VERSION
master1   Ready                      control-plane   88m   v1.29.4
worker1   Ready,SchedulingDisabled   <none>          45m   v1.29.4
worker2   Ready                      <none>          42m   v1.28.9
worker3   Ready                      <none>          40m   v1.28.9


 kubectl get node
NAME      STATUS   ROLES           AGE   VERSION
master1   Ready    control-plane   88m   v1.29.4
worker1   Ready    <none>          45m   v1.29.4
worker2   Ready    <none>          43m   v1.29.4
worker3   Ready    <none>          41m   v1.29.4



Автоматическое развертывание кластеров

установка с помощью kubespray

заполняем inventory

```
# ## Configure 'ip' variable to bind kubernetes services on a
# ## different ip than the default iface
# ## We should set etcd_member_name for etcd cluster. The node that is not a etcd member do not need to set the value, or can set the empty string value.
[all]
node1 ansible_host=158.160.151.47     ip=10.131.0.24 etcd_member_name=etcd1
node2 ansible_host=158.160.159.34    ip=10.131.0.34
node3 ansible_host=158.160.162.132   ip=10.131.0.31
node4 ansible_host=158.160.165.171   ip=10.131.0.30

# ## configure a bastion host if your nodes are not directly reachable
# [bastion]
# bastion ansible_host=x.x.x.x ansible_user=some_user

[kube_control_plane]
node1

[etcd]
node1

[kube_node]
node2
node3
node4

[calico_rr]

[k8s_cluster:children]
kube_control_plane
kube_node
calico_rr
```

запускаем плейбук

 sudo ansible-playbook -i inventory/mycluster/inventory.ini --become --become-user=root --user=${SSH_USERNAME} --key-file=${SSH_PRIVATE_KEY} cluster.yml

kubectl get node
NAME    STATUS   ROLES           AGE   VERSION
node1   Ready    control-plane   20m   v1.29.3
node2   Ready    <none>          20m   v1.29.3
node3   Ready    <none>          20m   v1.29.3
node4   Ready    <none>          20m   v1.29.3
