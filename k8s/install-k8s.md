# Install K8S

## K8S

```bash
hostnamectl set-hostname k8sm
vi /etc/resolv.conf
vi /etc/hosts
```

### swap kernel

`sudo modprobe overlay sudo modprobe br_netfilter`

### intall packages:

`intsall containerd on both master and node` \
`yum install -y kubectl kubeadm kubelet`

### bootstrap Cluster

```bash
kubeadm init --control-plane-endpoint=192.168.1.130 or k8sm1.home.com
mkdir -p $HOME/.kube 
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
sudo chown $(id -u):$(id -g) $HOME/.kube/config 
kubectl get nodes
```

## Network configuration

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml kubectl apply -f calico.yaml
```

### Once all the above steps are ready:

````
ssh k8smaster:
kubectl get nodes
```sh
[root@k8sm1 wal]# kubectl get nodes
NAME    STATUS   ROLES           AGE    VERSION
k8sm1   Ready    control-plane   187d   v1.28.2
k8sn1   Ready    <none>          187d   v1.28.2
k8sn2   Ready    <none>          187d   v1.28.2
```
````

### From above we can say two nodes are ready for deployment:

```
There is 2 ways to deploy 
1. Imperrative way:
   1. It is a way to deploy using command line:
      1. kubectl create deployment nginx --image=nginx --replicas=3
      2. It will automatically deploy nginx and replication will be 3
2. Deceletative way:
   1. Using Yaml/Json manifest file
```

### Lets deploy a container using CMD

```bash
        kubectl get pod
        kubectl create deployment nginx --image=nginx --replicas=3
        [root@k8sm1 wal]# kubectl get pod
        NAME                     READY   STATUS    RESTARTS   AGE
        nginx-7854ff8877-djvzz   1/1     Running   0          14s
        nginx-7854ff8877-jbr5v   1/1     Running   0          14s
        nginx-7854ff8877-pnjvv   1/1     Running   0          14s
        [root@k8sm1 wal]# 
        kubectl get deployments.apps
        kubectl logs pods {pods_name}
        kubectl exec -it nginx-7854ff8877-pnjvv bash // if you need to go inside container
        kubectl delete deployments.apps nginx
```
