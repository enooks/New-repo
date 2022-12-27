[[study]]
# chage 명령어란?


>- 사용자의 패스워드 만기 정보를 변경 및 설정하는 명령어
 \- 시스템에게 로그인한 사용자가 패스워드를 변경해야 하는지를 알려줌
 \- 시스템 관리 명령어이다 보니, root 권한을 가진 사용자만 사용 가능


## 주요 옵션
|옵션|내용|
|----|---|
|-l|지정한 계정의 정보를 보여줌|
|-d|최근 패스워드를 바꾼 날 수정 'YYYY-MM-DD' 형식|
|-E|계정의 만료일 설정 'YYYY-MM-DD' 형식|
|-m|패스워드 변경일로부터 최소 며칠이 경과해야 다른 패스워드로 변경할 수 있는지를 설정 패스워드 최소 의무 사용일 수 지정|
|-M|패스워드 최종 변경일로부터 패스워드 변경 없이 사용할 수 있는 최대 일수 설정|
|-I|패스워드 만요일까지 패스워드를 바꾸지 않으면 계정이 만료되어 비활성화. 이때, 별도의 유예기간을 지정하고자 할 떄 사용하는 옵션|
|-W|패스워드 만료 며칠 전 부터 사용자에게 경고 메세지를 보낼지 설정|





## *1. -l (소문자 L, 지정한 계정의 패스워드 만기 정보 확인)*

- 지정한 계정의 패스워드 만기 정보를 보여줌.

- 문법 : **chage -l 계정명**

사용자 패스워드의 자세한 정보를 확인하기 위해서는 '-l' 옵션을 사용. 만약, 'user2' 계정의 패스워드 정보를 확인하고 싶다면, 다음과 같이 입력 단, root 권한이 필요하므로 sudo를 함께 사용

```bash
yhjeong@ubuntu:~$ sudo chage -l user2
Last password change		        : May 20, 2020
Password expires					: never
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
```


## *2. -d (패스워드의 마지막 변경일을 수정)*

- 패스워드의 마지막 변경일을 수정하는 옵션

- 문법 : **chage -d 날짜 계정명**

-날짜 형식으로 지정할 경우, 'YYYY-MM-DD' 형식을 따름

- 숫자 형식으로 지정할 경우, 1970년 1월 1일부터의 날짜 수를 적어줌

해당 계정이 마지막으로 패스워드를 바꾼 날짜를 수정하는 옵션. 변경일은 숫자 및 날짜 형식으로 설정 가능. 단, 숫자로 변경할 때는 1970년 1월 1일부터 지정한 날짜까지의 경과한 날짜 수를 입력. '-l' 옵션을 통해 조회된 정보 중에서는 'Last password change' 항목에 대응하는 값.

user2 계정의 최근 패스워드 수정일을 변경하는 예제

_*** 날짜 형식으로 지정 - 2020년 6월 30일로 변경**_

```bash
yhjeong@ubuntu:/$ sudo chage -d '2020-06-30' user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Jun 30, 2020
Password expires					: May 26, 2042
Password inactive					: May 31, 2042
Account expires						: Dec 31, 2020
Minimum number of days between password change		: 6
Maximum number of days between password change		: 8000
Number of days of warning before password expires	: 3
```

_*** 숫자 형식으로 지정 - 2020년 7월 30일로 변경**_

1970년 1월 1일로부터 2020년 7월 30일 사이의 날짜 수는 18,473일.

```bash
yhjeong@ubuntu:/$ sudo chage -d 18473 user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Jul 30, 2020
Password expires					: Jun 25, 2042
Password inactive					: Jun 30, 2042
Account expires						: Dec 31, 2020
Minimum number of days between password change		: 6
Maximum number of days between password change		: 8000
Number of days of warning before password expires	: 3
```

## *3. -E (지정한 계정의 만기일 설정)*

- 지정한 계정의 만기일을 지정

-날짜 형식으로 지정할 경우, 'YYYY-MM-DD' 형식

- 숫자 형식으로 지정할 경우, 1970년 1월 1일부터의 날짜 수를 적어줌

- 문법 : **chage -E 계정 만기일 계정명**

user2의 계정 만기일을 설정해보되 날짜 및 숫자 형식을 모두 사용. **주의할 점은** **계정의 패스워드 만기일이 아닌 계정 자체의 만기일****을 지정하는 옵션이라는 점.** '-l' 옵션을 통해 조회된 정보 중에서는 'Account expires' 항목에 대응하는 값

_*** 날짜 형식으로 지정 - 2020년 10월 1일로 지정**_

```bash
yhjeong@ubuntu:/$ sudo chage -E '2020-10-01' user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Feb 02, 1970
Password expires					: Feb 14, 1970
Password inactive					: Feb 19, 1970
Account expires						: Oct 01, 2020
Minimum number of days between password change		: 15
Maximum number of days between password change		: 12
Number of days of warning before password expires	: 10
```

_*** 숫자 형식으로 지정 - 2020년 11월 1일로 지정**_

1970년 1월 1일로부터 2020년 11월 1일 사이의 날짜 수는 18,567일

```bash
yhjeong@ubuntu:/$ sudo chage -E 18567 user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Feb 02, 1970
Password expires					: Feb 14, 1970
Password inactive					: Feb 19, 1970
Account expires						: Nov 01, 2020
Minimum number of days between password change		: 15
Maximum number of days between password change		: 12
Number of days of warning before password expires	: 10
```

## *4. -m (패스워드 최소 의무 사용일 수를 지정하는 옵션)*

- 지정한 계정의 패스워드 변경일로부터 최소 며칠이 경과해야 다른 패스워드로 변경할 수 있는지를 지정

- 문법 : **chage -m 날짜 수 계정명**

예를 들어, user2라는 사용자가 2020년 5월 21일 날 패스워드를 변경. 그런데 패스워드를 다른 것으로 또 바꾸고 싶어졌다. 이때, 날짜 수를 2로 지정하면 user2 사용자는 2020년 5월 23일 이후부터 다른 패스워드를 변경할 수 있음. 만약, 날짜 수를 0으로 지정하면 별다른 제약 없이 패스워드를 변경할 수 있음.

user2의 패스워드 변경 후, 최소 의무 사용일 수를 5일로 설정.

```bash
yhjeong@ubuntu:/$ sudo chage -m 5 user2
```

user2의 패스워드 만기 정보를 확인해보면, 'Minimum number of days between password change' 항목이 5로 변경된 것을 확인할 수 있음.

```bash
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Feb 02, 1970
Password expires					: Feb 17, 1970
Password inactive					: Feb 22, 1970
Account expires						: Nov 01, 2020
Minimum number of days between password change		: 5
Maximum number of days between password change		: 15
Number of days of warning before password expires	: 10
```

## *5. -M (패스워드 변경 없이 사용할 수 있는 최대 일수 설정)*

- 지정한 계정의 패스워드를 변경 없이 사용할 수 있는 최대 일수를 지정하는 옵션

- 문법 : **chage -M 날짜 수 계정명**

-m 옵션과 사용법이 같음.

user2라는 계정의 패스워드 최대 사용일 수를 180일로 설정하려면 다음과 같이 사용

```bash
yhjeong@ubuntu:/$ sudo chage -M 180 user2
```

user2의 패스워드 만기 정보를 확인해보면, 'Maximum number of days between password change' 항목이 180으로 변경된 것을 확인할 수 있음. 이와 동시에 'Password expires' 항목도 'Last password change' 항목에 설정된 2020년 7월 30일에서 180일 후인, 2021년 1월 26로 변경되었음을 알 수 있음.

```bash
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Jul 30, 2020
Password expires					: Jan 26, 2021
Password inactive					: Jan 31, 2021
Account expires						: Dec 31, 2020
Minimum number of days between password change		: 6
Maximum number of days between password change		: 180
Number of days of warning before password expires	: 3
```

## *6. -I (대문자 i, 패스워드 만료 시 비활성화 상태로 유예기간을 지정하는 옵션)*

- 패스워드 만료일 도래 시, 계정이 비활성화 상태로 전환될 때까지의 유예기간을 지정하는 옵션

- 옵션명은 inactive의 첫 글자 i에서 따왔음.

- 문법 : **chage -I 날짜 수 계정명**

사용자의 패스워드 유효기간이 만료되었는데도 불구하고, 사용자가 제때 패스워드를 변경하지 않으면 계정은 비활성화됨. 이때, 바로 비활성화 상태로 전환되지 않고, 별도의 유예기간을 설정하고 싶을 때 사용하는 옵션

user2의 패스워드 정보를 확인해보면, 패스워드의 만기일은 2021년 1월 26일, 비활성화는 2021년 1월 31일부로 설정되어 있음. 이를 통해, 기존의 비활성화까지의 유예기간이 5일로 설정되어 있었음을 유추

```bash
yhjeong@ubuntu:/$ sudo chage -I user2
Last password change				: Jul 30, 2020
Password expires					: Jan 26, 2021
Password inactive					: Jan 31, 2021
...
```

아래는 패스워드 만료일 후, 유예기간을 10일로 설정하는 예제 명령 실행 후, 패스워드 만료일인 1월 26일부터 10일 후인 2월 5일부터 비활성화가 이루어진다는 정보를 확인할 수 있음.

```bash
yhjeong@ubuntu:/$ sudo chage -I 10 user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Jul 30, 2020
Password expires					: Jan 26, 2021
Password inactive					: Feb 05, 2021
Account expires						: Dec 31, 2020
Minimum number of days between password change		: 6
Maximum number of days between password change		: 180
Number of days of warning before password expires	: 3
```

## *7. -W(패스워드 만료 며칠 전부터 사용자에게 경고 메시지를 보낼지를 설정)*

- 패스워드 만료 며칠 전부터 사용자에게 패스워드 변경 경고 메시지를 보낼지 설정하는 옵션이다.

- 문법 : **chage -W 날짜 수 계정명**

사용자의 패스워드 만료일이 도래하기 전, 해당 사용자에게 패스워드를 바꾸라는 warning 메시지를 전달할 수도 있다. 이때 사용하는 것이 '-W' 옵션

user2 계정에 패스워드 만료 10일 전부터 경고 메시지를 전달하고자 할 때, 다음과 같이 명령어를 입력하면 됨. 명령 실행 후엔 'Number of days of warning before password expires' 항목에 대응하는 값이 바뀌었음을 확인할 수 있음. 즉, user2사용자는 패스워드 만료 10일 전인 2021년 1월 16일부터 패스워드를 변경하라는 경고 메시지를 받게 됨.

```bash
yhjeong@ubuntu:/$ sudo chage -W 10 user2
yhjeong@ubuntu:/$ sudo chage -l user2
Last password change				: Jul 30, 2020
Password expires					: Jan 26, 2021
Password inactive					: Feb 05, 2021
Account expires						: Dec 31, 2020
Minimum number of days between password change		: 6
Maximum number of days between password change		: 180
Number of days of warning before password expires	: 10
```

## */etc/shadow 파일과의 연계성*

리눅스의 사용자 정보는 /etc/passwd 파일에 기록되어 있음. 하지만, /etc/passwd 파일의 2번째 필드인 패스워드 항목은 보안상의 문제로 /etc/shadow라는 파일에서 별도로 관리. 왜냐하면 해커들이 crack 툴을 활용하여 실제 패스워드를 알아낼 수 있기 때문.

chage도 패스워드에 대한 정보를 수정하는 명령어이기 때문에, 명령이 성공적으로 수행되면 /etc/shadow 파일도 동시에 업데이트. 그럼 chage 명령어를 통해 정보가 업데이트가 될 때, /etc/shadow의 어떤 필드가 서로 긴밀하게 연관되어 변경되는지 살펴보자.

위의 예제로 들었던 user2의 패스워드 정보를 살펴본다(보안상 패스워드, 다른 계정 정보는 보이지 않게 가져옴).

```bash
yhjeong@ubuntu:/$ sudo cat /etc/shadow | grep user2 
user2:*:18473:6:180:10:10:18627:
```

":"로 구분되어, 총 9개의 필드로 구성된 /etc/shadow 파일의 각 필드 정보는 다음과 같다.

|1|2|3|4|5|6|7|8|9|
|-|-|-|-|-|-|-|-|-|
|*user*|<center>\*</center>|<center>18473</center>|<center>6</center>|<center>180</center>|<center>10</center>|<center>10</center>|<center>18627</center>|
|*사용자명*|패스워드|패스워드 최종 수정일|패스워드 최소 사요일 수|패스워드 최대 사용일 수|패스워드 만료 전 경고 시작 일 수|비활성화 기간| 정 만료일| 예약 필드|

각 필드를 chage 명령어의 옵션과 연관 지어 본다면 다음과 같이 정리할 수 있겠다.

|1|2|3|4|5|6|7|8|9|
|-|-|-|-|-|-|-|-|-|
|*user*|\*|18473|6|180|10|10|18627|
|||-d|-m|-M|\-W|-I|-E|


