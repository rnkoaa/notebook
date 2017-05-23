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
    kubeadm init --apiserver-advertise-address $MASTER_IP
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
    kubeadm join --token f47b0a.9c17a6bd21153974 192.168.33.10:6443
```

if you run into the following issue:
```sh 
kubeadm init stuck on "First node has registered, but is not ready yet"
```

Follow the following steps to fix it.
first take a backup of `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`
using the following command
```sh
sudo cp /etc/systemd/system/kubelet.service.d/10-kubeadm.conf /etc/systemd/system/kubelet.service.d/10-kubeadm.conf.bak
```

Update `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf` by removing `$KUBELET_NETWORK_ARGS`. After this step, the file should look like this
```sh
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--kubeconfig=/etc/kubernetes/kubelet.conf --require-kubeconfig=true"
Environment="KUBELET_SYSTEM_PODS_ARGS=--pod-manifest-path=/etc/kubernetes/manifests --allow-privileged=true"
Environment="KUBELET_NETWORK_ARGS=--network-plugin=cni --cni-conf-dir=/etc/cni/net.d --cni-bin-dir=/opt/cni/bin"
Environment="KUBELET_DNS_ARGS=--cluster-dns=10.96.0.10 --cluster-domain=cluster.local"
Environment="KUBELET_AUTHZ_ARGS=--authorization-mode=Webhook --client-ca-file=/etc/kubernetes/pki/ca.crt"
ExecStart=
ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_SYSTEM_PODS_ARGS $KUBELET_DNS_ARGS $KUBELET_AUTHZ_ARGS $KUBELET_EXTRA_ARGS
```

Then reset `kubeadmn` with the following command

```sh
sudo kubeadm reset
```

Reload the system properties and restart
```sh
sudo systemctl daemon-reload
```

Then reinitialize kubeadm as before 
```sh
kubeadm init --pod-network-cidr 10.244.0.0/16 --apiserver-advertise-address $MASTER_IP
```