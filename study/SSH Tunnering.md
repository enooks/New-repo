[[study]]
# SSH 터널링

## SSH 터널링 3가지 유형

1. 로컬 포트 포워딩 (Local Port Forwarding) : SSH Client의 연결이 SSH 서버를 통해 대상 서버로 포워딩.
	SSH Client -> SSH Server -> Target Server
2. 원격 포트 포워딩 (Remote Port Forwarding) : SSH Server의 연결이 SSH Client를 통해 대상 서버로 포워딩.
	SSH Server -> SSH Client -> Target Server
3. 동적 포트 포워딩 (Dynamic Port Forwarding) : 다양한 프로그램의 연결이 SSH Client를 통해 SSH Server로 포워딩된 다음 대상 서버로 포워딩.
	다양한 프로그램 -> SSH Client -> SSH Server -> Target Server



### Local Port Forwarding

![[Pasted image 20221102170540.png]]


your host : local PC, SSH Client
remote host : wny-bastion, SSH Server
farawayhost : wny-bastion 안에 Docker가 설치되어 있는 Server, Target Server

*command*
ssh -L 1234(임의설정한 포트):192.168.77.10(bastion 서버 안에 있는 서버 IP):8080(열어 놓은 포트(wordpress를 8080으로 열어놓음)) wny-bastion(ssh server)
ssh -L 1234:192.168.77.10:8080 wny-bastion


farawayhost를 사설 IP로 사용한 이유는 다른 N/W 망에 있기때문

![[Pasted image 20221102174754.png]]

### Remote Port Forwarding

![[Pasted image 20221102173601.png]]

ssh -R 1234:175.113.150.8(doj bastion):18022(열려있는 포트) wny-bastion -> ssh manager@localhost -p 1234(이 포트번호로 열어놓게 설정)

*command*
ssh -R 1234:175.113.150.8:18022 wny-bastion -> ssh manager@localhost -p 1234

! IP를 사설망이 아닌 공인 IP를 사용한 이유 !
- nearhost, your host, remotehost 는 모두 따로따로 N/W 망으로 이루어져있기때문

이렇게 접속 후 chrome에서 1234 프록시 등록 해놓고 사용하면 
기존 SKB에서 막아놓았던 사이트를 wny-bastion 타고 웹접속하여 접속이 가능하게 됨


------------------------------------------------
*실제 사용 사례*
==ssh -L 1234:10.10.219.140:9090 skbb.dev.g2.gw==
Local port forwarding 1234 port로 forwarding 열어놓은 port는 9090 (prometheus)

remotehost = ==skbb.dev.g2.gw (175.113.150.8)==
farawayhost = ==10.10.219.140 (DNS primary server)==
localhost = ==사무실PC (210.94.1.27)==
