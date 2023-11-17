yum install bash-completion 으로 설치 할 수 있음.

/usr/share/bash-completion/bash_completion 생성



## Kubectl 자동 완성

### BASH

```bash
source <(kubectl completion bash) # bash-completion 패키지를 먼저 설치한 후, bash의 자동 완성을 현재 셸에 설정한다
echo "source <(kubectl completion bash)" >> ~/.bashrc # 자동 완성을 bash 셸에 영구적으로 추가한다
```

또한, `kubectl`의 의미로 사용되는 약칭을 사용할 수 있다.

```bash
alias k=kubectl
complete -o default -F __start_kubectl k
```



```bash
echo 'complete -o default -F __start_kubectl k' >>~/.bashrc
```




확인 명령어

`type _init_completion`