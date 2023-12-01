```bash
#!/bin/bash

# 서버 IP와 패스워드가 담긴 파일명
SERVERS_FILE="/path/to/ip_password"

# 변경할 패스워드 일자
CHAGE_DATE="2023-08-12"

# 서버별로 chage 날짜 변경
while read line
do

  SERVER=`echo $line | cut -d ' ' -f 1`
  PASSWORD=`echo $line | cut -d ' ' -f 2`

  echo "server: $SERVER"

  sshpass -p "$PASSWORD" ssh -n -o StrictHostKeyChecking=no -o ConnectTimeout=3 root@"$SERVER" "chage -d '$CHAGE_DATE' root"

  if [ $? -eq 0 ]; then
    echo -e "\033[032mchage success\033[0m"
  else
    echo -e "\033[031mfailed\033[0m"
  fi

done < "$SERVERS_FILE"
```

while 문 돌릴때 계속 한 번 밖에 실행되고 끝났는데 `ssh '-n'` 옵션을 줘서 해결했다.

/dev/stdin 관련 문제로 확인됨
