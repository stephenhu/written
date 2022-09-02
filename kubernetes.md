# kubernetes

1. `curl -s https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -`
1. `sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main"`
1. `sudo apt-get update`
1. `sudo apt-get install kubelet kubeadm kubectl`
1. `sudo apt-mark hold kubelet kubeadm kubectl` # prevents these from being automatically upgraded or deleted

### pod commands
1. `kubectl create -f kubernetes.yaml`
1. `kubectl get pods`
1. `kubectl describe pod zp`

#### useful commands
1. `minikube ssh`

#### k8 init

There are different choices for container runtimes, the default after installation should be cgroupfs,
but on Ubuntu systemd is preferred over cgroupfs.

add this file to /etc/docker/daemon.json:

```
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
```

1. `sudo mkdir -p /etc/systemd/system/docker.service.d`
1. `systemctl daemon-reload`
1. `systemctl restart docker`
1. `sudo swapoff -a`
1. `kubeadm config images pull` # tests for connectivity
1. `kubeadm init`
1. `mkdir -p $HOME/.kube`
1. `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
1. `sudo chown $(id -u):$(id -g) $HOME/.kube/config`
1. `kubectl apply -f https://github.com/vmware-tanzu/antrea/releases/download/v0.2.0/antrea.yml`
1. `kubeadmn join <k8 control plane>:6443 sha256:`
1. `kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml`

```
To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.110.178.10:6443 --token s85m1y.w952wnfzjeaexfte \
    --discovery-token-ca-cert-hash sha256:586f3873e663d152ee1ef1b485d642ceb62290557cab7858696fa3ae6cecfad7
```

#### useful commands
1. `kubeadm token list`
1. `kubeadm token create`
1. `openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed `s/^.* //``
1. `kubectl drain <node name> --delete-local-data --force --ignore-daemonsets`
1. `kubectl delete node <node name>`
1. `kubectl taint nodes --all node-role.kubernetes.io/master-`

## nginx ingress controller

## minikube

This is a minimalized version of k8 as typically k8 requires 3+ machines, with minikube, you could run on your laptop.

### running on fusion 11

_Note: Binaries downloaded from the web will require setting mac os x permission to run, go to systems preference > security & privacy and allow binaries to run._

1. `curl -s https://github.com/machine-drivers/docker-machine-driver-vmware/releases/download/v0.1.0/docker-machine-driver-vmware_darwin_amd64` # this driver works with fusion and workstation
1. `curl -s https://github.com/kubernetes/minikube/releases/download/v1.6.2/minikube-darwin-amd64`
1. `sudo install docker-machine-driver-vmware_darwin_amd64 /usr/local/bin/docker-machine-driver-vmware`
1. `sudo install minikube-darwin-amd64 /usr/local/bin/minikube`
1. `minikube start --vm-driver=vmware`  # this command takes forever, maybe in the range or 20-30m
1. `minikube status`
1. `minikube dashboard`

#### helpful commands

1. `minikube stop`
1. `minikube delete`

## rancher (experimental)

Based on my experience using rancher, it is quite buggy, not recommended, but here are some simple instructions in case you still wish to use rancher.

1. `docker pull rancher/rancher`
1. `docker run -d -p 8888:443 --restart=unless-stopped rancher/rancher`
1. https://address:8888 open in browser # note that rancher ui seems to disable http and redirect to https


## references

* https://kubernetes.io/docs/tasks/tools/install-minikube/
* https://minikube.sigs.k8s.io/docs/start/macos/
* https://github.com/kubernetes/minikube
* https://github.com/machine-drivers/docker-machine-driver-vmware
* https://kubernetes.io/docs/setup/learning-environment/minikube/#specifying-the-vm-driver
* https://kubethink.com/posts/create-an-on-premises-cluster-in-china-v1-12/
* https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl
* https://github.com/kubernetes/kubernetes/issues/56038
* https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
* https://kubernetes.io/docs/setup/production-environment/container-runtimes/
* https://github.com/kubernetes/kubeadm/issues/610
* https://github.com/vmware-tanzu/antrea/blob/master/docs/getting-started.md
* https://github.com/kubernetes/dashboard#kubernetes-dashboard
* https://docs.microsoft.com/en-us/azure/aks/best-practices
* https://dzone.com/articles/running-local-docker-images-in-kubernetes-1
