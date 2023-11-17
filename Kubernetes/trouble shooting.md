```bash
$ k get nodes
E0216 13:38:35.255294    1107 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused                                        E0216 13:38:35.255734    1107 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused                                        E0216 13:38:35.257221    1107 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused                                        E0216 13:38:35.258691    1107 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused                                        E0216 13:38:35.260113    1107 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused                                        The connection to the server localhost:8080 was refused - did you specify the right host or port? 
```

-> $HOME/.kube 디렉토리 밑 파일 이름이 admin.conf로 설정됨.  kubectl은 config 파일을 $HOME/.kube에서 찾는데 원래 이름을 'config'로 수정하니 정상 작동됨



```bash
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
```

-> unset KUBECONFIG -> export KUBECONFIG=/etc/kubernetes/admin.conf 
