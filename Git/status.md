git status 명령어 입력 시 Git은 어떻게 처리할 파일이 없는지 알 수 있을까?

.git/index 파일 중요

최신 commit의 tree와 index와의 내용이 일치한다면 commit 할 것이 없다는걸 Git은 알 수 있음



index파일의 적혀있는 파일의 hash 값과 파일의 내용이 만들어내는 내용이 다르다면 수정 되었다는걸 알 수 있음

수정만하면 commit만 했을때 수정의 대상에 들어가지않음
add를 했을때 commit의 대상이 된다.

index에서 수정된 파일을 가르키는 내용과 수정한 파일의 내용이 같으니 
git은 파일이 commit 대기상태인걸 알 수 있음

git은 index의 내용과 가장 최신 commit의 tree가 가르키는 파일의 내용이 다르다면
index의 add가 되어서 commit 대기상태인걸 알 수 있음

git commit을 하면 commit이 만들어지고 tree에는
수정한 파일의 hash값
저장소, 인덱스와 프로젝트 폴더