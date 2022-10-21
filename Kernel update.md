[[study]]
CentOS7 Kenel update
=======================================

리눅스 커널을 수동으로 업그레이드하려고 하면 복잡하며 귀찮은 작업들이 많고 시간도 오래 걸림

따라서 **elrepo** **저장소**를 통해 커널을 업그레이드하는 방법을 알아보자

> **elrepo란?**
>
> Enterprise Linux 용 커뮤니티 기반 저장소이며, RHEL(RedHat Enterprise Linux) 및 이를 기반으로 꾸며진 기타 배포판들(Fedora, CentOS, Scientific)에 대한 지원을 제공하는 저장소  
 > 
> 주로 소프트웨어적인 부분 보다 커널, 파일 시스템 드라이버, 그래픽 드라이버 등을 비롯한 하드웨어와 관련된 패키지에 중점을 두고 있는 저장소

커널을 업데이트하기 전 가장 먼저 해야 할 일은 모든 패키지를 최신 버전으로 업데이트하는 것

구버전의 패키지들이 지원하지 않거나, 요소가 없으면 문제가 생기기 때문에 업데이는 필수

아래의 명령어를 통해 패키지를 업데이트

```bash
$ yum -y update
$ yum install yum-plugin-fastestmirror
```

위 명령어를 통해 CentOS7 시스템이 업데이트 되고, 모든 패키지가 최신 버전으로 업데이트됨
<br></br>

## 1. 커널 버전 확인
-----------------------

아래의 명령어를 입력하여 CentOS7 운영체제 및 커널 버전 확인

### 운영체제 버전 확인
```bash
$ cat /etc/redhat-release

CentOS Linux release 7.7.1908 (Core)
```

```bash
$ cat /etc/os-release
 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
 
CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```


### 커널 버전 확인
```bash
$ uname -msr

Linux 5.4.0-1.el7.elrepo.x86_64 x86_64
```

<br></br>
## 2. elrepo 저장소 추가
------------------------------------------
새로운 커널을 **elrepo 저장소**를 통해 설치하려면 먼저 **elrepo 저장소**가 추가되어있어야함

아래의 명령어를 입력하여 **RPM-GPG-KEY**를 먼저 추가
```bash
$ rpm —import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```
> **GPG-KEY란?**
>
> GNU Privacy Guard-KEY의 약자로 배포되고 있는 패키지가 안전한 패키지가 맞는지 확인하는 일종의 인증키

이제 yum 명령어를 통해 elrepo 저장소를 추가

```bash
$ yum install https://www.elrepo.org/elrepo-release-7.0-4.el7.elrepo.noarch.rpm
```
<br></br>
이제 시스템에 **elrepo 저장소**가 성공적으로 등록되었는지 확인

```bash
$ yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.navercorp.com
 * elrepo: ftp.ne.jp
 * epel: ftp.riken.jp
 * extras: mirror.navercorp.com
 * updates: mirror.navercorp.com
repo id                            repo name                                                              status                                   base/7/x86_64                      CentOS-7 - Base                                                        10,072      
elrepo                             ELRepo.org Community Enterprise Linux Repository - el7                    150           epel/x86_64                        Extra Packages for Enterprise Linux 7 - x86_64                         13,758        extras/7/x86_64                    CentOS-7 - Extras                                                         512    updates/7/x86_64                   CentOS-7 - Updates                                                      4,135       repolist: 28,627
```
<br></br>

## 3. 새로운 커널 설치
-----------------------
아래의 명령어를 입력하여 최신 버전의 새로운 커널을 설치
```bash
$ yum --enablerepo=elrepo-kernel install kernel-ml kernel-ml-devel
```
> **--enablerepo 옵션?**
>
> 기본적으로 일반적인 저장소가 활성되어있으나, elrepo에서 최신 커널을 받아와 설치해야 하기 때문에 해당 저장소를 사용한다고 옵션을 통해 명시해 주어야함

<br>
</br>

## 4. 부팅 순서 변경
---------------------
새로운 최신 버전의 커널을 설치했으니, 이제 부팅할 때 **예전 버전의 커널**이 아닌 **새로 설치한 커널**로 **부팅**하도록 설정할 것이다.

아래의 명령어를 입력하여 현재 부팅 가능한 커널을 확인한다. *(kenel 버전만 뽑아내는 명령어)*

```bash
$ grep ^menuentry /boot/grub2/grub.cfg | cut -d "'" -f2
 
CentOS Linux (5.4.0-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux (5.3.11-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux (3.10.0-1062.4.1.el7.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.21.3.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-db37ede35ceb4dcfbe37a4e157315bec) 7 (Core)

```

그다음 아래 명령어를 통해 최신 커널로 부팅하도록 설정

```bash
$ grub2-set-default "CentOS Linux (5.4.0-1.el7.elrepo.x86_64) 7"
```

부팅 순서가 정상적으로 변경되었는지 확인

```bash
$ grub2-editenv list

saved_entry=CentOS Linux (5.4.0-1.el7.elrepo.x86_64) 7
```

순서가 변경되었으면 재부팅을 통해 현재 커널을 최신 커널로 변경

```bash
$ reboot
```

<br></br>
## 5. 오래된 커널 제거(Optional)
---------------------------------------------
새로운 커널이 설치되었으니 저장 공간을 위해서 예전 버전의 커널을 삭제

삭제하기 전에 현재 부팅한 커널이 새로운 커널이 맞는지 확인

```bash
$ uname -msr

Linux 5.4.0-1.el7.elrepo.x86_64 x86_64
```

최신 커널들을 놔두고 옛날 커널들을 삭제하기 위하여 아래의 방법을 사용

일단, 커널 수정을 위해 아래의 명령어를 입력하여 yum-utils를 설치

```bash
$ yum install yum-utils
```

설치가 완료되면, 아래의 명령어를 입력하여 최신 커널 2개(혹은, 그 이상)을 놔두도록 함

```bash
$ package-cleanup --oldkernels --count=2
```
> "--count=* 옵션"
> 2개를 남기고 싶으면, 2를 입력하고 혹은 원하는 숫자를 입력하면 됨
