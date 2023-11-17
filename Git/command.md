# Git

git init
- 해당 디렉토리를 버전관리하는 디렉토리로 사용하는 명령어 (master로 변경됨) 저장소 초기화

git status
- Untracked files -> version 관리가 되고 있지 않음
- git add 명령어로 관리하는 파일을 추가
- 다시 git status로 확인해보면 new file로 추가된걸 확인할 수 있음
- 새로 추가한 파일 같은 경우엔 add 명령어 사용
- 버전 관리를 할 때 핵심 파일과 임시 파일이 있을 수 있는데 관리 하고자 하는 파일을 알려주기 위해 add 명령어를 사용하는 것

### 버전이란?

버전과 변화는 차이가 있음
버전 -> 의미 있는 변화
어떠한 작업이 있을 때 그 작업이 완결된 상태

git commit
- 편집기로 넘어가짐 (windows에서는 다른 편집툴이 열리는듯)
- 맨 위쪽 라인 버전의 메세지 작성
- 버전의 메세지는 변화가 어떤 변화가 있는지? 왜 변경되었는지 이유 작성 (commit message)

git log
- 버전이 잘 만들었는지 확인
- 버전 메세지가 보임
- 누가 만들었는지, 언제 만들었는지 확인할 수 있음(시간, 이메일주소 나옴)

기존 파일을 수정하고 git status로 확인 시 modifed(수정되었음)을 확인해 볼 수 있음

git에서 파일을 수정 할 때도 git add 명령어 사용
최초로 추적할때도 git add, 수정할때도 git add 

수정 후 git add 확인 -> git status보면 modified 수정됨



#### 왜 commit 하기 전 add를 해야 하는가?

원하는 파일만 commit 하기 위해서 사용.

프로젝트를 하다보면 여러 소스코드를 생성
commit하는 시기를 놓칠수도잇음
commit은 하나씩 하는게 좋음


commit 대기 상태 -> stage area

add를하면 commit '대기상태'로 넘어가진다.
git commit하면 대기상태인 파일들만 commit 됨

stage -> commit 대기상태인 파일들이 있는 곳
repository -> commit이 된 결과가 저장 되어잇는 곳 


git reset
- git reset 돌아가고싶은 commit 해시 --hard
	- git reset 7b031a8ff51cca9aa8805003f6f13f08a2b5ce2c
- git에서는 왠만하면 삭제하지 않는다. reset해도.. 복구하려면 복구할수있음

git revert
- 이것도 되돌릴수있는 명령어