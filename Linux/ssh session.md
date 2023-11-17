# SSH 연결 안끊기게 설정

/etc/ssh/sshd_config
-> ClientAliveInterval : 클라이언트 살아있는지 확인하는 간격
-> ClientAliveCountMax : 클라이언트 응답 없어도 접속 유지하는 횟수


e.g.) ClientAliveInterval=15, ClientAliveCountMax=3 이면 45초 후 접속 끊김