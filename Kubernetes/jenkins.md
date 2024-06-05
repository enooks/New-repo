\*참고 https://www.jenkins.io/doc/book/installing/kubernetes/#create-a-namespace

# Setup Jenkins On Kubernetes

1. Create a Namespace
2. Create a service account with Kubernetes admin permissions.
3. Create local persistent volume for persistent Jenkins data on Pod restarts.
4. Create a deployment YAML and deploy it.
5. Create a service YAML and deploy it.

## Step 1: Jenkins용 네임스페이스 만들기

모든 DevOps 도구를 다른 애플리케이션과 별도의 네임스페이스로 분류하는 것이 좋다.

```bash
kubectl create namespace devops-tools
```

## Step 2: 'serviceAccount.yaml' 파일을 만들고 다음 관리자 서비스 계정 manifest 복사

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: jenkins-admin
rules:
  - apiGroups: [""]
    resources: ["*"]
    verbs: ["*"]
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-admin
  namespace: devops-tools
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: jenkins-admin
subjects:
  - kind: ServiceAccount
    name: jenkins-admin
    namespace: devops-tools
```

'serviceAccount.yaml'은 'jenkins-admin' 클러스터 역할(clusterRole), 'jenkins-admin' 서비스 계정(ServiceAccount)을 생성하고 'clusterRole'을 service account에 바인딩한다.

'jenkins-admin' 클러스터 역할은 클러스터 구성 요소를 관리할 수 있는 모든 권한을 가진다. 개별 리소스 작업을 지정하여 액세스를 제한할 수도 있다.

kubectl 명령어로 생성

```bash
kubectl apply -f serviceAccount.yaml
```

clusterrole에 jenkins-admin 생성된 것 확인

```bash
kubectl get clusterrole -n devops-tools
NAME                                                                   CREATED AT
admin                                                                  2024-06-04T08:54:29Z
cluster-admin                                                          2024-06-04T08:54:29Z
edit                                                                   2024-06-04T08:54:29Z
flannel                                                                2024-06-05T04:25:20Z
jenkins-admin                                                          2024-06-05T05:39:02Z
kubeadm:get-nodes                                                      2024-06-04T08:54:30Z
system:aggregate-to-admin                                              2024-06-04T08:54:29Z
system:aggregate-to-edit                                               2024-06-04T08:54:29Z
system:aggregate-to-view                                               2024-06-04T08:54:29Z
system:auth-delegator                                                  2024-06-04T08:54:29Z
system:basic-user                                                      2024-06-04T08:54:29Z
system:certificates.k8s.io:certificatesigningrequests:nodeclient       2024-06-04T08:54:29Z
...
```

serviceaccounts에서도 devops-tools namespace에 jenkins-admin이 생성된 걸 확인 가능

```bash
kubectl get serviceaccounts -A
NAMESPACE         NAME                                 SECRETS   AGE
default           default                              0         21h
devops-tools      default                              0         57m
devops-tools      jenkins-admin                        0         56m
kube-flannel      default                              0         130m
kube-flannel      flannel                              0         130m
kube-node-lease   default                              0         21h
kube-public       default                              0         21h
kube-system       attachdetach-controller              0         21h
kube-system       bootstrap-signer                     0         21h
kube-system       certificate-controller               0         21h
kube-system       clusterrole-aggregation-controller   0         21h
kube-system       coredns                              0         21h
kube-system       cronjob-controller                   0         21h
kube-system       daemon-set-controller                0         21h
kube-system       default                              0         21h
kube-system       deployment-controller                0         21h
...
```

## Step 3: 'volume.yaml'을 생성하고 다음 퍼시스턴트 볼륨 manifest 복사

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: jenkins-pv-volume
  labels:
    type: local
spec:
  storageClassName: local-storage
  claimRef:
    name: jenkins-pv-claim
    namespace: devops-tools
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  local:
    path: /mnt
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - <your-worker-node>
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: jenkins-pv-claim
  namespace: devops-tools
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

<aside>
💡자신의 worker node의 이름으로 수정하기
</aside><br>

볼륨의 경우 데모를 위해 '로컬' 스토리지 클래스를 사용하고 있다. 즉, '/mnt' 위치 아래의 특정 노드에 'PersistentVolume' volume을 생성한다.

'로컬' 스토리지 클래스에는 node selector가 필요하므로, 특정 노드에서 Jenkins 파드를 스케줄링하려면 워커 노드 이름을 올바르게 지정해야한다.

파드가 삭제되거나 다시 시작되면 데이터는 노드 볼륨에 유지됩니다. 그러나 노드가 삭제되면 모든 데이터가 손실된다.

이상적으로는 노드 장애 시 데이터를 보존하기 위해 클라우드 제공업체에서 사용 가능한 스토리지 클래스 또는 클러스터 관리자가 제공한 클래스를 사용하는 영구 볼륨을 사용해야 한다.

kubectl 명령어로 생성

```bash
kubectl create -f volume.yaml
```

## Step 4: 'deployment.yaml'이라는 이름의 배포 파일을 생성하고 다음 배포 manifest 복사

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
  namespace: devops-tools
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins-server
  template:
    metadata:
      labels:
        app: jenkins-server
    spec:
      securityContext:
        fsGroup: 1000
        runAsUser: 1000
      serviceAccountName: jenkins-admin
      containers:
        - name: jenkins
          image: jenkins/jenkins:lts
          resources:
            limits:
              memory: "2Gi"
              cpu: "1000m"
            requests:
              memory: "500Mi"
              cpu: "500m"
          ports:
            - name: httpport
              containerPort: 8080
            - name: jnlpport
              containerPort: 50000
          livenessProbe:
            httpGet:
              path: "/login"
              port: 8080
            initialDelaySeconds: 90
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 5
          readinessProbe:
            httpGet:
              path: "/login"
              port: 8080
            initialDelaySeconds: 60
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 3
          volumeMounts:
            - name: jenkins-data
              mountPath: /var/jenkins_home
      volumes:
        - name: jenkins-data
          persistentVolumeClaim:
            claimName: jenkins-pv-claim
```

1. Jenkins 파드가 로컬 퍼시스턴트 볼륨에 쓸 수 있도록 'securityContext'를 설정한다.
2. Jenkins 파드의 상태를 모니터링하기 위한 라이브 및 준비 상태 프로브.
3. Jenkins 데이터 경로 '/var/jenkins_home'을 보유한 로컬 스토리지 클래스를 기반으로 하는 로컬 퍼시스턴트 볼륨.

kubectl 명령어로 생성

```bash
kubectl apply -f deployment.yaml
```

deployment 생성 확인

```bash
kubectl get deployment -n devops-tools
```

## Step 5: 쿠버네티스 서비스를 사용하여 젠킨스에 액세스하기

이제 배포를 생성했다. 그러나 외부에서는 액세스할 수 없다. 외부에서 Jenkins 배포에 액세스하려면 서비스를 만들고 이를 배포에 매핑해야 한다.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: devops-tools
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/path: /
    prometheus.io/port: "8080"
spec:
  selector:
    app: jenkins-server
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 32000
```

여기서는 포트 32000의 모든 kubernetes 노드 IP에서 Jenkins를 노출하는 'NodePort'로 유형을 사용하고 있습니다. 인그레스 설정이 있는 경우, 인그레스 규칙을 생성하여 Jenkins에 액세스할 수 있습니다. 또한 AWS, Google 또는 Azure 클라우드에서 클러스터를 실행하는 경우 Jenkins 서비스를 로드밸런서로 노출할 수 있습니다.

kubectl 명령으로 생성

```bash
kubectl apply -f service.yaml
```

browser에서 확인

```
ssh -D 17755 skbb.dev.g2.gw
> ssh Dinamic 으로 열어 놓고 proxy 17755로 접근하여 사용가능

http://<node-ip>:32000
```

처음 접속하게 되면 패스워드를 입력해야하는데 찾는 방법은 먼저 pod name을 확인 후

```bash
kubectl get pods --namespace=devops-tools
```

log를 확인해보면 된다.

```bash
kubectl logs <your-pod-name> --namespace=devops-tools
```

또는 pod에 직접 접근하여 확인하는 방법도 있다.

```bash
kubectl exec -it <your-pod-name> cat /var/jenkins_home/secrets/initialAdminPassword -namespace=devops-tools
이 명령어는 곧 사라질 명령어라 이런식으로도 확인할 수 있다 (작성일 24/06/05)

kubectl exec --namespace=devops-tools <your-pod-name> -- cat /var/jenkins_home/secrets/initialAdminPassword
```

비밀번호 입력 후 제안되는 plugin을 설치하여 jenkins dash보드를 이용할 수 있다.
