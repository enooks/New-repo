kubernetes에서 context name 변경 방법

```shell
$ kubectl config rename-context old-name new-name
```

확인 방법
```shell
$ kubectl config get-contexts
```


e.g.)
```shell
$ kubectl config rename-context tst-kube kubernetes-admin-skb-tst-kiw01.btvpaas.com@skb-tst-kiw01.btvpaas.com admin@skb-suy-tst-kiw
```

old-name = kubernetes-admin-skb-tst-kiw01.btvpaas.com\@skb-tst-kiw01.btvpaas.com
new-name = admin@skb-suy-tst-kiw
