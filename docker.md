# docker


## install

docker-compose on linux (included in docker desktop for mac os x)

* https://docs.docker.com/engine/install/ubuntu/

* `wget https://github.com/docker/compose/releases/download/1.25.0/docker-compose-Linux-x86_64`
* `sudo install docker-compose-Linux-x86_64 /usr/local/bin/docker-compose`

## docker-compose setup

sample docker-compose.yml file with traefik

## commands

* `docker stats`

## docker postgres psql

* `docker exec -it postgres psql -U zp -W zpdata`

## links

* https://github.com/phusion/baseimage-docker/issues/415#issuecomment-305877243 # base ubuntu image doesn't include tzdata which stores timezones in /usr/share/zoneinfo, i guess they probably want to save on size of the docker ubuntu image
* https://docs.docker.com/engine/reference/commandline/stats/
* https://blog.jessfraz.com/post/two-objects-not-namespaced-linux-kernel/
* https://docs.docker.com/config/containers/resource_constraints/

* https://www.docker.com/blog/docker-secrets-management/
* https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-using-a-repository
* https://serverfault.com/questions/871090/how-to-use-docker-secrets-without-a-swarm-cluster
* https://kubernetes.io/docs/concepts/configuration/secret/
* https://github.com/kubernetes/community/blob/master/contributors/design-proposals/auth/secrets.md
* https://blog.kubernauts.io/managing-secrets-in-kubernetes-with-vault-by-hashicorp-f0db45cc208a
* https://docs.docker.com/storage/
* https://docs.docker.com/engine/swarm/configs/
* https://docs.docker.com/compose/compose-file/
* https://docs.docker.com/compose/reference/
* https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/
* https://blog.oddbit.com/post/2015-02-05-creating-minimal-docker-images/
* https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html#environment-variables
* https://blog.container-solutions.com/tagging-docker-images-the-right-way
* https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
* https://stackoverflow.com/questions/47714278/why-is-compiling-with-cgo-enabled-0-slower
* https://www.how2shout.com/how-to/how-to-install-docker-ce-on-ubuntu-20-04-lts-focal-fossa.html
* https://linuxhandbook.com/docker-permission-denied/

## useful commands

* docker stats --no-stream

## production kubernetes environment

### Requirements

1.  Ubuntu 18.04+
1.  Docker

### GF

* https://mirrors.aliyun.com/kubernetes/apt/ instead of https://apt.kubernetes.io/
* https://mirrors.aliyun.com/docker-ce/linux/ubuntu instead of https://download.docker.com/linux/ubuntu
* curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
* `export HTTP_PROXY=proxy.vmware.com:3128`
* `export HTTPS_PROXY=proxy.vmware.com:3128`

### Setup

#### docker-ce, docker-ce-cli, containerd.io, kubeadm, kubectl, kubelet

1. `sudo apt-get update && sudo apt-get -y upgrade`
1. `sudo apt-get install apt-transport-https`
1. `curl -s https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -`
1. `sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"`
1. `sudo apt-get update`
1. `sudo apt-get install docker-ce docker-ce-cli containerd.io`
1. `sudo usermod -a -G docker $USER` # exit/re-enter shell instance if you get a permission issue


#### useful commands
* `docker save imageName -o image.tar`
* `docker load -i image.tar`





## references


* https://docs.docker.com/install/linux/docker-ce/ubuntu/
* https://rancher.com/docs/rancher/v2.x/en/quick-start-guide/deployment/quickstart-manual-setup/
