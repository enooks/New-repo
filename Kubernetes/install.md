kubernetes 사용하려면

Container runtime 설치
kubeadm, kubelet kubectl 설치
docker 설치
cri-docker 설치

1. Kubernetes 는 Pod 에서 Container를 실행하려면 Container Runtime을 사용해야함
2. Docker engine을 사용했지만 v1.24 부터는 cri-dockerd 추가 설치해야함 (컨테이너 런타임이 쿠버네티스와 호환되기 위한 요구 사항인 CRI를 만족하지 않기때문)
3. kubeadm, kubelet, kubectl install

## Installation process (Redhat/CentOS)

### swapoff

```bash
# swapoff 후 /etc/fstab swap 줄 비활성화

swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab
```

-> `free` 명령어로 확인시 swap쪽 0으로 확인됨
-> `/etc/fstab` 확인시 swap 줄 주석(#) 처리

### Uninstall old versions

Older versions of Docker went by the names of `docker` or `docker-engine`. Uninstall any such older versions before attempting to install a new version, along with associated dependencies:

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

-> docker가 미리 설치되어 있으면 삭제해주기

### Set up the repository

```bash
sudo yum install -y yum-utils

sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
```

-> docker yum repository(저장소) setting

### Install Docker Engine

Latest install

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable docker

sudo systemctl start docker
```

-> docker 최신버전 설치(CentOS/Redhat)

### cri-docker install

k8s version 1.24 (22/05) 이후 kubernetes에서 기본적으로 내부 연결 지원해주던 `dockershim`이 제거가 되어 `cri-docker` 추가 설치하여 k8s 연결

```bash
VER=$(curl -s https://api.github.com/repos/Mirantis/cri-dockerd/releases/latest|grep tag_name | cut -d '"' -f 4|sed 's/v//g')

echo $VER

wget https://github.com/Mirantis/cri-dockerd/releases/download/v${VER}/cri-dockerd-${VER}.amd64.tgz

tar xvf cri-dockerd-${VER}.amd64.tgz

sudo mv cri-dockerd/cri-dockerd /usr/local/bin/



# cri-docker version check

cri-dockerd --version

wget https://raw.githubusercontent.com/Mirantis/cri-dockerd/master/packaging/systemd/cri-docker.service

wget https://raw.githubusercontent.com/Mirantis/cri-dockerd/master/packaging/systemd/cri-docker.socket

sudo mv cri-docker.socket cri-docker.service /etc/systemd/system/

sudo sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service

sudo systemctl daemon-reload

sudo systemctl enable cri-docker.service

sudo systemctl enable --now cri-docker.socket



# cri-docker active check

sudo systemctl restart docker && sudo systemctl restart cri-docker

sudo systemctl status cri-docker.socket --no-pager
```

```bash
# docker cgroup change require to systemd

cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
    },
    "storage-driver": "overlay2"
}
EOF

sudo systemctl restart docker && sudo systemctl restart cri-docker

sudo docker info | grep Cgroup

# kernel forwarding

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system
```

-> cri-docker 설치 후 설정

### Kubeadm install

```bash
ifconfig -a

sudo cat /sys/class/dmi/id/product_uuid
```

-> mac address, uuid 고유한값인지 확인

```bash
# Red Hat-based distributions

cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
```

```bash
# Set SELinux in permissive mode (effectively disabling it)

sudo setenforce 0

sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet
```

```bash
kubectl version --short
Flag --short has been deprecated, and will be removed in the future. The --short output will become the default.
Client Version: v1.26.1
Kustomize Version: v4.5.7
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

### Master node setting

Linux

| Runtime                              | Path to Unix domain socket                 |
| ------------------------------------ | ------------------------------------------ |
| containerd                           | unix:///var/run/containerd/containerd.sock |
| CRI-O                                | unix:///var/run/crio/crio.sock             |
| ==Docker Engine(using cri-dockerd)== | unix:///var/run/cri-dockerd.sock           |

```bash
sudo kubeadm config images pull --cri-socket /var/run/cri-dockerd.sock

sudo kubeadm init --ignore-preflight-errors=all --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=controller ip(master) --cri-socket /var/run/cri-dockerd.sock

kubeadm join 10.10.219.96:6443 --token iskvve.9j0sz0lvzxv28dzp \  # token {iskvve.9j0sz01vzxv28dzp}
        --discovery-token-ca-cert-hash sha256:f0e238b5fa16053cc722f379f89390184cb71add9504a1f988d231d300d87255



# kubeadm 을 root 처럼 사용하기 위한 추가 설정 (root 계정 아닐때)

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/




# Token 기억 안 날 경우

kubeadm token create --print-join-command


# hash 값 조회

openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'

# 서비스 확인 (Ready, Running)

kubectl get nodes -o wide
kubectl get pod -A
```

### CNI install (master)

```bash
wget https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

# flannel default podCIDR = 10.244.0.0/16

master node에서 kubeadm init 했을때 pod-network-cidr=192.168.0.0/16 설정놨음

파일 다운로드 받은 후 cidr 10.244.0.0/16 -> 192.168.0.0/16 수정..
```

### Worker node join

```bash
kubeadm join (control-plane ip):(control-plane port) --token (token) \
            --discovery-token-ca-cert-hash sha256(hash) --cri-socket /var/run/cri-dockerd.sock
```
