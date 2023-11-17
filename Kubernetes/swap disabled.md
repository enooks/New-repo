Swap disabled. You **MUST** disable swap in order for the kubelet to work properly.
[kubernetes.io](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

`swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab`

# swap memory disable 이유는?

* Pod를 할당하고 제어하는 kubelet은 swap 상황을 처리하도록 설계되지 않음. 
	*-> kubernetes에서 가장 기본이 되는 Pod의 컨셉 자체가 필요한 Resource만큼만 Host 자원에서 할당 받아 사용하는 구조이기 때문. 
	따라서 kubernetes 개발팀은 memory swap을 고려하지 않고 설계했기 때문에 
	Cluster node로 사용할 Sever Machine들은 모두 swamp memory를 disable 해야함.



