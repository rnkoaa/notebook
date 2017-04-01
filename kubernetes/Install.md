Kubernetes setup

Install docker
kubelet
kubectl
kubeadm

master:
```sh
kubeadm init --pod-network-cidr=10.244.0.0/16 --api-advertise-addresses $MASTER_IP
```

nodes
kudeadmin join

configure networking

install addons


On Master:
kubectl get nodes
watch kubectl get nodes (for continuous refresh)

On Node 
kubeadmn join --token=484ae0.4025e99b793cb412 $MASTER_IP


Go to /etc/kubernetes
there's a file called kubelet.conf

cp ~/.kube/config ~/.kube/config.bak
scp user@master_ip:/etc/kubernetes/admin.conf ~/.kube/config

master.sh

```sh
    #!/bin/bash
    export MASTER_IP=192.168.33.10
    apt-get update && apt-get upgrade -y && apt-get install -y apt-transport-https

    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

   cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

    apt-get update -y && apt-get install -y docker.io kubelet kubeadm kubectl kubernetes-cni

    kubeadm init --pod-network-cidr 10.244.0.0/16 --apiserver-advertise-address $MASTER_IP
    # after this command, we will get a token for clients to join such as
    # kubeadmn join --token=484ae0.4025e99b793cb412 $MASTER_IP

    curl -sSL "https://github.com/coreos/flannel/blob/master/Documentation/kube-flannel.yml?raw=true" | kubectl --namespace=kube-system create -f -

    kubectl create -f https://rawgit.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard.yaml --namespace=kube-system
```

```sh
    node.sh

    #!/bin/bash
    export MASTER_IP=192.168.33.10
    apt-get update && apt-get upgrade -y && apt-get install -y apt-transport-https

    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

   cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

    apt-get update -y && apt-get install -y docker.io kubelet kubeadm kubectl kubernetes-cni

    #kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address $MASTER_IP
    # after this command, we will get a token for clients to join such as
    # kubeadmn join --token=484ae0.4025e99b793cb412 $MASTER_IP

    # curl -sSL "https://github.com/coreos/flannel/blob/master/Documentation/kube-flannel.yml

    # kubectl create -f https://rawgit.com/kubernetes/dashboard/master/src/deploy/kube

    kubeadmn join --token=484ae0.4025e99b793cb412 $MASTER_IP
    kubeadm join --token 985ad6.1f7b9fab1255fda4 192.168.33.10:6443
```