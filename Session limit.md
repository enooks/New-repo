[[study]]
**/etc/security/limits.conf 에서 특정 사용자 계정의 동시 로그인 수를 제한 할 수 있음**

## 1. 사용방법
----------------

<br></br>
### 1) /etc/security/limits.conf 파일 수정
\# vi /etc/security/limits.conf

\<domain> 사용자 계정을 작성

\<type> soft/hard/- 를 정함

\<item> 제한할 항목을 작성

\<value> 값
<br></br>

*예제) centos란 계정의 동시 로그인 수를 1로 정하기*
```bash
#<domin>       <type>     <item>      <value>
#
@centos          -       maxlogins       1
```