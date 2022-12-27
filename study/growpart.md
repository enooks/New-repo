[[study]]
# LVM 이 아닌 Standard partition 이용 시 사용 명령어

fdisk 명령어로 / (루트) 에 마운트 된 디바이스명 확인 ex) sda2


```shell
$ growpart /dev/sda 2
```

파일시스템 타입이 xfs 라면

```shell
$ xfs_grofs /
```

growpart 명령어가 없다면

```shell
$ yum install cloud-utils-growpart
```

