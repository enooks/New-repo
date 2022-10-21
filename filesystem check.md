[[study]]
# 파일 검사 및 수리 명령어
<br></br>
## fsck 와 e2fsck
---------------------
이 명령어들은 파일을 검사하거나 수리해주는 명령어

사실 파일 검사는 부팅할 때 자동으로 리눅스가 파일 시스템을 점검함

그리고 손상된게 있으면 알아서 자동으로 복구해줌
<br></br>
### fsck (filesystem check)
리눅스 파일 시스템을 검사하고 수리하는 명령어

e2fsck = fsck의 확장 명령어

\- fsck [option] 장치명

\- e2fsck [option] 장치명

e2fsck가 확장버전이기 때문에 실질적으로 현재 리눅스 배포판에서 fsck 명령어를 실행하면 e2fsck 실행됨

fsck 명령어는 손상된 디렉토리나 파일을 수정할 때 임시로 /lost+found 디렉토리에서 작업을 수행하고

정상적인 복구가 되면 사라짐
<br></br>
## Option
-----------
|옵션|내용|
|---|---|
|-a|명령 수행에 대한 확인 질문 없이 무조건 수행 (권장하지 않음)|
|-v|점검 내역 상세 보기, 자세한 출력|
|-y|모든 응답 yes|
|-n|모든 질문에 대한 응답 no|
|-f|파일 시스템이 이상 유무에 상관 없이 ==강제적==으로 파일 시스템 체크|


<br></br>
fsck 명령어를 수행하면 내부적으로 동작 단계는 아래와 같음

1. 블록들과 파일 크기 검사

2. 중복된 이름이 있는지 검사

3. 경로명 검사

4. 연결성 검사

5. Shadows/ACLs 인증

6. 참조 수 검사

7. 싸이클 그룹 검사

<br></br>
## fsck 실행 시 중요 사항
-------------------------------
fsck/e2fsck 명령어 사용 시 ==마운트된== 드라이브에서는 ==절대절대== 사용하면 안됨

만약, 마운트된 드라이브에서 fsck를 수행하면 그 드라이브는 망가질 확률이 높다고 보면 됨

그러므로 fsck 수행하기 전 unmount 작업 진행 

<br></br>
## error code
---------------
0 - No errors

1 - Filesystem errors corrected

2 - System should be rebooted

4 - Filesystem errors left uncorrected

8 - Operational error

16 - Usage or syntax error

32 - Fsck canceled by user request

128 - Shared-library error

\* error가 없으면 'clean'으로 나옴
\* error가 있는 경우 'error 2' 처럼 error + 숫자 형식으로 나옴

<br></br>
## 어떤 상황에서 주로 파일이 손상될까
----------------------------------------------
### 1) 시스템이 갑자기 shutdown
\- 갑자기 서버가 중지됐을 때, 전원이 나가버렸을때 등 이럴때 파일 시스템이 손상 입을 수 있음

이럴 경우 서버의 갑작스런 종료로 인해 생겨난 파일 시스템 이상으로 재부팅시 

파일시스템이 마운트 되지 않는 경우가 종종 발생. 이때 다시 시스템을 부팅하여 사용하기 

이전에 fsck 명령어를 사용해 시스템 상 모든 파일 시스템을 점검하여 조치를 취해야 함 

하지만 요새는 최신 저널링 파일 시스템 덕분에 충돌나서 다운되도 fsck 안해줘도 된다 함
<br></br>
### 2) Clearing group locks improperly
\- 두 프로세스가 인지하지 못한채 동시에 서로 내부 구조나 내용을 변경했을 때
<br></br>
### 3) overriding the built-in file protection mechanisms
\- 다른 프로세스에 의해서 한 파일이 열려있는 상태인데 그 상황에서 다른 프로세스가 그 파일을 삭제하려 할 떄
<br></br>
### 4) System debugger when used incorrectly 
\- 시스템 디버거가 잘못쓰여졌을 때






