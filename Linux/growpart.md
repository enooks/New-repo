# LVM 이 아닌 Standard partition 이용 시 사용 명령어

fdisk 명령어로 / (루트) 에 마운트 된 디바이스명 확인 e.g.) sda2 or lsblk 명령어로 확인


```shell
$ growpart /dev/sda 2
```

파일시스템 타입이 xfs 라면

```shell
$ xfs_growfs /
```

growpart 명령어가 없다면

```shell
$ yum install cloud-utils-growpart
```


**블록 디바이스에 남은 공간 없음** 오류를 방지하려면 임시 파일 시스템 **tmpfs**를 **/tmp** 탑재 지점에 탑재합니다. 그러면 **/tmp**에 탑재된 10M **tmpfs**가 생성됩니다.

```shell
$ sudo mount -o size=10M,rw,nodev,nosuid -t tmpfs tmpfs /tmp
```

용량 차지 상위 디렉토리 리스트 출력
```shell
sudo du -x -h / | sort -h | tail -40 | sort -h -r
```



Troubleshooting

```shell
[root@doj-dev-opr-els-01 yum.repos.d]# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * epel: mirror-icn.yuki.net.uk
updates/7/x86_64/primary_db    FAILED
http://ftp.daumkakao.com/centos/7/os/x86_64/repodata/20fed44d0064e4e52de2083efa2ac06588083a724c1bded6b9fb552ee74135ec-primary.sqlite.bz2: [Errno 14] HTTP Error 404 - Not Found
Trying other mirror.
To address this issue please refer to the below knowledge base article

https://access.redhat.com/articles/1320623

If above article doesn't help to resolve this issue please create a bug on https://bugs.centos.org/

http://ftp.daumkakao.com/centos/7/os/x86_64/repodata/20fed44d0064e4e52de2083efa2ac06588083a724c1bded6b9fb552ee74135ec-primary.sqlite.bz2: [Errno 14] HTTP Error 404 - Not Found
Trying other mirror.
updates/7/x86_64/primary_db    FAILED
http://ftp.daumkakao.com/centos/7/os/x86_64/repodata/20fed44d0064e4e52de2083efa2ac06588083a724c1bded6b9fb552ee74135ec-primary.sqlite.bz2: [Errno 14] HTTP Error 404 - Not Found
Trying other mirror.
http://ftp.daumkakao.com/centos/7/os/x86_64/repodata/20fed44d0064e4e52de2083efa2ac06588083a724c1bded6b9fb552ee74135ec-primary.sqlite.bz2: [Errno 14] HTTP Error 404 - Not Found
Trying other mirror.
repo id                                                            repo name                                                                                        status
base/7/x86_64                                                      CentOS-7 - Base                                                                                  10,072
epel/x86_64                                                        Extra Packages for Enterprise Linux 7 - x86_64                                                   13,767
extras/7/x86_64                                                    CentOS-7 - Extras                                                                                   518
updates/7/x86_64                                                   CentOS-7 - Updates                                                                                    0
repolist: 24,357
```



yum clean all -> cache 정리

yum check-update -> 현재 인스톨된 프로그램 중에 업데이트 된 것을 체크
