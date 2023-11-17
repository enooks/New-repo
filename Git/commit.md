commit을 하면 commit message 정보가 object 디렉토리 안에 저장됨.

commit을 하면 버전도 파일의 내용처럼 object 안에 들어감

commit도 내부적으로 보면 object(객체)라고 할 수 있음.

commit에는 주요한 정보가 두 가지 있음



첫 번째, 
이전 commit이 무엇인가? 부모를 나타내는 parent


두 번째, 
commit이 일어난 시점. 
작업 디렉토리에 있는 파일의 이름과 그 파일의 이름이 담고있는 내용사이에 정보가 tree로 표시됨
그래서 각각의 버전마다 서도 다른 tree를 가르킨다.
tree에는 파일의 이름과 파일의 담겨있는 내용이 각각 링크되어있음
버전에 적혀있는 tree를 통해서 버전이 만들어진 시점에 프로젝트의 대한 상태를 파악할수있음 (snapshot)
버전이 만들어진 시점에 tree라는 정보 구조를 통해서 가지고있음.

object 디렉토리 안에 들어가는 파일은 object file
object file 은 크게 3가지 중 하나

1. 파일의 내용을 담고있는 'blob'
2. 디렉토리의 파일명과 파일 내용에 해당하는 blob의 정보를 담고있는 tree
3. commit 각각의 commit은 object id를 가지고있음