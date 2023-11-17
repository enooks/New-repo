source https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

### worker node upgrade

현재 kubernetes version

```shell
[k8s@k8s-control-plane ~]$ kubectl get nodes
NAME                 STATUS   ROLES           AGE     VERSION
k8s-control-plane    Ready    control-plane   7d14h   v1.28.0
k8s-worker-node-01   Ready    <none>          6d14h   v1.27.4  # v1.27.4 -> v1.28.0 upgrade
k8s-worker-node-02   Ready    <none>          6d14h   v1.27.4
k8s-worker-node-03   Ready    <none>          26h     v1.28.0
```

*CentOS, RHEL or Fedora

1. kubernetes의 패치 release를 찾기
```shell
sudo yum list --showduplicates kubeadm kubelet kubectl --disableexcludes=kubernetes
# 목록에서 최신 버전을 찾는다
# 현재 버전은 초록색으로 표기 됨
```


2. kubeadm, kubectl, kubelet upgrade
```shell
# 1.28.x-0에서 x를 최신 패치 버전으로 바꾼다
sudo yum install -y kubelet-1.28.0-0 kubeadm-1.28.0-0 kubectl-1.28.0-0 --disableexcludes=kubernetes
```

3. kubeadm upgrade 호출
```shell
[k8s@k8s-worker-node-01 ~]$ sudo kubeadm upgrade node
[upgrade] Reading configuration from the cluster...
[upgrade] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[preflight] Running pre-flight checks
[preflight] Skipping prepull. Not a control plane node.
[upgrade] Skipping phase. Not a control plane node.
[upgrade] Backing up kubelet config file to /etc/kubernetes/tmp/kubeadm-kubelet-config2262348852/config.yaml
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[upgrade] The configuration for this node was successfully updated!
[upgrade] Now you should go ahead and upgrade the kubelet package using your package manager.
```

4. kubelet restart
```shell
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

5. cluster status check
```shell
[k8s@k8s-control-plane ~]$ kubectl get nodes
NAME                 STATUS   ROLES           AGE     VERSION
k8s-control-plane    Ready    control-plane   7d15h   v1.28.0
k8s-worker-node-01   Ready    <none>          6d14h   v1.28.0   <--- version upgrade 확인
k8s-worker-node-02   Ready    <none>          6d14h   v1.27.4
k8s-worker-node-03   Ready    <none>          26h     v1.28.0
```

6. kubeadm, kubectl, kubelet version check
```shell
[k8s@k8s-worker-node-01 ~]$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"28", GitVersion:"v1.28.0", GitCommit:"855e7c48de7388eb330da0f8d9d2394ee818fb8d", GitTreeState:"clean", BuildDate:"2023-08-15T10:20:15Z", GoVersion:"go1.20.7", Compiler:"gc", Platform:"linux/amd64"}
[k8s@k8s-worker-node-01 ~]$ kubectl version
Client Version: v1.28.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.28.0
[k8s@k8s-worker-node-01 ~]$ kubelet --version
Kubernetes v1.28.0
```
