# sed 명령어는 무슨 명령어?
sed명령어도 vi 편집기처럼 편집에 특화된 명령어. 수정, 치환, 삭제, 글추가 등 편집기 기능은 왠만하면 다 가능

근데 vi 편집기는 편집기를 열어서 서로 소통하듯 수정/변경을 해나가는 대화형 방식. 커서로 라인도 옮겨다니고, 지우

고, 글쓰는 등등 워드파일 수정하듯이. 하지만 sed 명령어는 명령행에서 파일을 인자로 받아 명령어를 통해 작업한 후 

결과를 화면으로 확인하는 방식. 마치 편집기 명령어를 쓰듯 사용하는 것과 같음.

sed 명령어를 이용해 파일을 변경했을 경우의 특징은 sed 편집기는 '원본'을 손상하지 않음.

쉘 리다이렉션을 이용해 편집 결과를 저장하기 전까지는 파일에 아무런 변경도 가하지 않음.

모든 결과는 명령을 수행 후 화면으로 출력됨. 출력된 결과가 원본과 다르더라도 **==원본에 손해가 없음==**. 

이것이 sed 명령어의 특징.

***sed 명령어는 streamlined editor를 의미. streamlined는 '능률적인'을 의미***.


## sed 명령어 자주쓰는 대표적 사용법

### 특정 범위만큼 파일내용 출력하기
```bash
sed -n '1p' employees;
sed -n '1,3p' employees;
sed -n '8,$p' employees;
```

여기서 `p는 print`의 약자로 출력을 의미. `컴마는 주소 범위`를 지정.
sed는 원본을 건드리지 않는다했으니 sed로 작업한 부분만 억제해서 출력시키고 싶다면 `-n` 옵션을 써줘야함.
`-n` 옵션과 `p` 는 항상 같이 사용됨.

1. employees 파일에서 첫 번째 행만 출력해서 화면에 보여줌.
2. employees 파일에서 1~3라인 범위의 내용을 출력해서 보여줌.
3. employees 파일에서 8라인부터 파일끝까지 보여줌. `'$'`는 '끝'을 의미함.

1라인과 8라인부터 끝까지 출력을 하려면
```bash
sed -n -e '1p' -e '8,$p' employees
```


### 특정 단어로 시작하는 행들만 출력하기
```bash
sed -n '/^107/p' employees
sed -n '/103/p' employees
```

`'^'`는 메타문자로, '시작'을 의미. 특정 단어를 포함하고 있는 행들을 뽑을때는 `'^'`메타 문자 없이 사용.

1. employees 파일에서 107로 '시작'하는 행만 출력.
2. employees 파일에서 103을 포함하고 있는 행 출력.


### 파일에서 공백으로 이루어지거나 빈줄 제거하기
```bash
sed '/^$/d' employees
sed '/^$/d' employees > new_employees
sed '/^ *$/d' emplyees > new_emplyees
```

1. employees 파일에서 빈 라인들을 지운 후 내용 출력.
2. employees 파일에서 빈 라인들을 삭제한 후 출력되는 결과 new_employees 라는 파일명으로 저장.
3. employees 파일에서 빈 라인들이나 공백으로 채워진 행들을 삭제한 후 new_employees 라는 파일명으로 저장.

여기서 보이는 `'d' 서브 명령어는 delete의 약자로 삭제`를 의미.
'/' 사이에 들어있는 단어를 포함하고 있는 모든 줄을 삭제시키겠다는 의미.
`'^'는 행의 처음을 의미하고 '$'는 행의 끝을 의미`하니까 행의 처음과 끝이 같이 만나있는 것 즉 빈 줄을 의미.
employees 파일에서 빈 줄을 모두 제거하나는 뜻의 명령어.
1번처럼 명령어를 실행하면 원본 파일의 수정이 없기때문에 2번처럼 redirection으로 new_employees 파일을 따로 저장해야 완벽한 수정이 됨.

3번에보이는 `'*'는 메타문자로 앞의 문자를 0개 이상` 찾음.
행의 시작이 0개 이상의 공백으로 이뤄지다 끝을 맺는거니, 공백이거나 빈 줄을 찾아낸다는 의미.


### 단어 치환
IT_PROG라고 되어있는 부분을 DEVELOPER로 변경
```bash
sed 's/IT_PROG/DEVELOPER/g' employees
sed 's/IT_PROG?DEVELOPER/g' employees > new_employees
sed 's/it_prog/DEVELOPER/gi' employees > new_employees
```

s는 치환할 때 사용하는 옵션.
여기서 s와 같이 쓰이는 `g 플래그는 치환이 행에서 전체를 대상으로 이루어짐`을 의미.

3번에 보이는 `i플래그는 변경 대상 단어를 찾을 때 대소문자를 무시한다`는 플래그.


# 다양한 사용 예시
## 1. 출력 `p` 명령어 예시
```bash
sed -n '/love/p' file
```
-> file 파일에서 love가 포함된 행들을 찾아 출력. (-n 옵션이 있어야 love 패턴을 포함하는 행 출력.)


 

```bash
sed -n '/west/,/east/p' file
```
-> file 파일에서 west가 나오는 행과 east가 나오는 행 사이의 모든 행들이 출력




```bash
sed -n '3, /^employee/p' file
```
-> 3번째 행부터 employee로 시작되는 행까지 출력.



```shell
sed -i s/IPADDR=.*$/IPADDR=${ip}/    # IPADDR= 이 뒤에 적혀있는 건 다 삭제
```
