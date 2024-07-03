# Symbolic Link 삭제 시 복구방법
* OS: Rocky Linux

## Edit Mode 진입
VM 종료 후, 재시작 하여 'e'를 눌러 Edit Mode로 넘어간다.

## Boot Parameter 변경
기존에 있는 hgb quiet를 init=/bin/sh 로 수정 후 'Ctrl + x' 를 입력하여 저장 후 재시작 해준다.

* rhgb (Red Hat Graphical Boot): 부팅 과정 동안 텍스트 기반의 부팅 메시지 대신 그래픽 사용자 인터페이스(GUI)로 부팅 진행 상태를 표시합니다. 일반적으로 Red Hat 기반 배포판(RHEL, CentOS, Rocky Linux 등)에서 사용
* quiet: 부팅 시 커널 로그 출력 메시지를 최소화하는 옵션
* init=/bin/sh: 최소한의 환경에서 Shell을 실행하도록 시스템을 설정하여, 문제를 해결할 수 있는 Prompt로 진입 할 수 있게 해주는 옵션

## Symbolic Link 재생성
접속 한 이후에 / 파티션은 ro(Read Only) 로 mount 되어있어 커맨드 입력이 되지 않는다. rw(Read Write) 로 remout 해줘야 한다.
```bash
mount -o remount,rw /
```

remount를 해준 후 Symbolic Link를 생성해야하는 디렉토리로 이동 한다.
```bash
cd /usr/lib64
```

Symbolic Link를 생성해준다.
```bash
ln -s libcrypto.so.1.1.1k libcrypto.so.1.1
```
각 부분의 의미를 자세히 설명하면 다음과 같습니다:

ln 명령어

* ln은 링크(link)를 생성하는 명령어입니다. 기본적으로 하드 링크를 생성하지만, -s 옵션을 사용하면 심볼릭 링크를 생성합니다.

-s 옵션

* -s는 심볼릭 링크를 생성하기 위한 옵션입니다. 심볼릭 링크는 파일 시스템 내의 다른 파일이나 디렉토리를 가리키는 특별한 유형의 파일입니다. 이는 원본 파일의 경로를 포함하는 텍스트 파일처럼 동작합니다.

libcrypto.so.1.1.1k

* 이 부분은 심볼릭 링크가 가리킬 실제 파일입니다. 예를 들어, OpenSSL 라이브러리의 특정 버전 파일입니다. 이 파일이 존재해야만 심볼릭 링크가 유효합니다. 

libcrypto.so.1.1

이 부분은 생성될 심볼릭 링크의 이름입니다. 이 링크를 통해 libcrypto.so.1.1로 접근할 때 실제로는 libcrypto.so.1.1.1k 파일에 접근하게 됩니다. 

### 전체 명령어의 동작

1. libcrypto.so.1.1라는 이름의 심볼릭 링크를 생성합니다.
2. 이 링크는 libcrypto.so.1.1.1k 파일을 가리킵니다.
3. 따라서 libcrypto.so.1.1를 참조하는 프로그램이나 명령어는 실제로 libcrypto.so.1.1.1k 파일을 사용하게 됩니다.
