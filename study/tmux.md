tmux란?
==============
> tmux는 사용자가 단일 단말기 창 또는 원격 터미널 세션 안에서 **여러 별도의 터미널 세션에서 액세스**할 수 있도록 ***여러 가상 콘솔을 다중화***하는데 사용할 수 있는 응용 소프트웨어 이 응용 프로그램은 명령어 인터페이스로부터 다수의 프로그램을 처리하고 유닉스 셸로부터 프로그램을 분리하는 데에 아주 유용

쉽게 말하자면 **tmux**는 **기존의 터미널 창을 분할해 사용**할 수 있도록 해줘 새로운 터미널 창을 열지 않지 않고도 여러

작업을 실행할 수 있게 해줌 이런 프로그램을 **터미널 멀티플렉서**라고 부름


## Session, Windows, Panes


### Session
하나 이상의 윈도우가 있는 독립된 작업 공간

### Windows
동일한 세션에서 시각적으로 분리된 부분 (브라우저의 탭을 생각하면 이해하기 쉬움)

### Panes
동일한 윈도우에서 분리된 부분 (시각적으로 분리되어 있지 않음)

## 명령어

### Session

- tmux : 새로운 세션 시작
- tmux new -s *\<Session Name>* : 세션 이름과 함께 새로운 세션 시작
- tmux ls : 현제 세션 목록 나열
- tmux 실행 중일때 ctrl + a -> d : 현재 세션 빠져나오기
- tmux a : 마지막 세션으로 들어가기
- tmux a -t *\<Session Name>* : 특정 세션으로 들어가기
- tmux 실행 중일때 exit : 세션 종료
- tmux kill-session -t *\<Session Name>* : 특정 세션 kill

### Window

- ctrl+a -> c : 새로운 윈도우 생성

- ctrl+a -> d : 윈도우 닫기
- ctrl+a -> p : 이전 윈도우로 이동
- ctrl+a -> n : 다음 윈도우로 이동
- ctrl+a -> , : 현재 윈도우 이름 바꾸기
- ctrl+a -> w : 현재 윈도우 목록 나열
<br></br>
### Pane
- ctrl+a -> " : 현재 창을 가로로 나누기

- ctrl+a -> % : 현재 창을 세로로 나누기
- ctrl+a -> 방향키 : 방향키 방향의 창으로 이동
- ctrl+a -> z : 현재 창 확대/축소 전환
- ctrl+a -> \[ : space를 누르면 선택을 시작하고 enter를 누르면 선택 내용이 복사
- ctrl+a -> space : 창 배열을 순환
- ctrl+a -> s : synchronize-panes on으로 모든 Pane을 동시 입력 가능




파일 적용
_tmux source_-file ~/._tmux_.conf






----------------- tmux instal ------------------

# Install tmux 2.8 on Centos

# install deps

yum install gcc kernel-devel make ncurses-devel

# cd src

cd /usr/local/src

# DOWNLOAD SOURCES FOR LIBEVENT AND MAKE AND INSTALL

curl -LO https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz

tar -xf libevent-2.1.8-stable.tar.gz

cd libevent-2.1.8-stable

./configure --prefix=/usr/local

make

make install

# DOWNLOAD SOURCES FOR TMUX AND MAKE AND INSTALL

curl -LO https://github.com/tmux/tmux/releases/download/2.8/tmux-2.8.tar.gz

tar -xf tmux-2.8.tar.gz

cd tmux-2.8

LDFLAGS="-L/usr/local/lib -Wl,-rpath=/usr/local/lib" ./configure --prefix=/usr/local

make

make install

# pkill tmux

# close your terminal window (flushes cached tmux executable)

# open new shell and check tmux version

tmux -V
