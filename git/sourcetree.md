﻿# SourceTree

Add 하고 Commit 하고 Pull 하고 Conflict 발생하고 Merge 하고 Push하면 된다.

## Repository 만들기
	가장 처음 repository를 만들 때
	Clone/New 에서 Create Repository 를 누르고
	폴더를 선택하면 Repository가 만들어진다.

## 버전 만들기(Commit)
	파일을 추가(Add)하고 Commit 하게 되면
	그 파일은 버전관리 대상이 되는 파일이 된다.
	해당 파일은 Working Copy 영역에서 버전관리가 되는 것을 알 수 있다.

## Working Copy, Unstaged
	Add 되기 이전의 공간(수정된 파일이 표시됨)


## index, Staging Area, Staged
	Add 된 이후의 공간
	Staging Area로, commit 했을 때 버전에 합류할 파일들의 리스트가 존재하는 공간
	변경된 모든 파일을 하나에 두지 않고 각각의 변경사항 중에서 원하는 파일만 골라서 하나의 버전을 만들 수 있음

## Repositoy
	Commit 된 파일이 반영된 장소

## 되돌리기(Discard) - Commit 을 하지 않았을 때
	파일을 마구 수정했을 때 예전에 Commit 했던 버전으로 돌아가고 싶으면 Discard 를 이용한다.

## 되돌리기(Reset)
````
이후의 버전으로 돌아가고 싶으면서 이전의 버전을 모두 지우고 완전히 이전 버전으로 돌아가고 싶을 때는
우측 마우스 클릭 - Reset current branch to this commit
````	

- Soft - 자신의 브랜치를 두고 해당 버전으로 Reset한다.
- Mixed - 버전은 지우지만 작업한 내용은 남는다.
- Hard - 버전이랑 소스까지도 사라져서 완전히 이전 버전으로 돌아간다.

````
이후의 버전으로 돌아가고 싶으면서 이전의 버전을 모두 지우고 현재까지 작업한 내용은 남기고 싶을 때는
Mixed 를 하게 되면 이전 버전으로 돌아가면서 현재까지 작업한 내용은 남게된다.
따라서 현재까지 작업한 내용은 Uncommited changes 로 뜨게 된다.
````

## 되돌리기(Revert)
````
선택한 버전을 취소하고 이전 버전 상태로 되돌리는 것(취소한 버전도 유지)
해당 버전에 오른쪽 마우스 클릭 - Reverse commit
하게 되면 취소한 버전을 유지하면서 이전 버전으로 되돌아간다.
Reset은 버전을 삭제하는 반면 Revert는 버전을 유지한다
````

## 브랜치(Branch)
	예를 들어 작업을 실험적인 것과 실제 일상적인 부분으로 나누어 작업해야 할 때 브랜치를 생성하여 작업을 분리할 수 있다.
	Branch - New Branch 탭에서 Branch 이름 적고 - Create Branch
	하게 되면 새로운 브랜치가 생성된다.
	왼쪽 브랜치 메뉴에서 더블클릭으로 브랜치를 전환할 수 있다.

## 브랜치 병합(Merge)
	병합을 하려면 병합을 받는 쪽으로 브랜치를 체크아웃 해야 한다.
	병합 하려는 브랜치 오른쪽 마우스 클릭 - Merge [병합당하는 브랜치명] into current branch
	를 누르게 되면 Merge 가 실시되며 자동으로 Commit을 실행한다.
	Merge가 끝난 브랜치는 삭제해도 된다.


## 충돌 해결(Resolve Conflicts)
	서로 다른 브랜치에서 같은 내용을 수정하고 Merge를 하게 될 시 Conflict 가 발생한다.
	그리고 GIt 에서는 사람에게 이 일을 위임한다. (Merge는 되고 소스가 변경됨)

````diff
<<<<<<< HEAD
		document.write("안녕");
=======
		document.write("Hello");
>>>>>>> [브랜치명]
````

````
중간에 있는 ======= 을 기준으로 <<<<<<< HEAD 사이에 있는 소스는
현재 자신이 선택한 current Branch의 소스이며 그 브랜치가 바로 HEAD이다.

======= 을 기준으로 >>>>>>> [브랜치명] 사이에 있는 소스는
병합당하는 브랜치의 소스이다.

그 둘 중 필요한 소스를 고르고 필요 없는 소스를 모두 지우고 저장한다.
그 후 working copy에서 confict 난 파일 우측 마우스 클릭 - Resolve Conficts - Mark Resolved
Mark Resolved 란 충돌이 났던 상황을 다 정리했다는 사실을 Git 에게 알려주는 것이다.
````

## 충돌 해결2(자동 해결)
	Resolve Conflicts 에서
	Resolve Using 'Mine' 을 누르게 되면 병합 브랜치의 내용으로 변경되고
	Resolve Using 'Theirs' 를 누르게 되면 피병합 브랜치의 내용으로 변경된다.


## 충돌 전 충돌 최소화
	병합 브랜치와 피병할 브랜치가 있으면 피병합 브랜치에서는 코딩전에 병합 브랜치로부터 내용을 가져온다.
	(피병합 브랜치로 체크아웃하여 병합 브랜치를 Merge 한 다음 코딩한다.)
	이렇게 하게되면 충돌을 최소화 할수 있고 충돌이 나더라도 작은 충돌들이 일어난다.
	이렇게 작은 Conflict 들을 해결하는 것이 나중에 큰 Conflict 를 만나는 것보다 좋다.


## 원격 저장소 추가(Add Remote)
	하나의 로컬저장소는 여러개의 원격저장소(Remotes)에 저장할 수 있다.
	로컬저장소를 선택하고 Add Remote 한 뒤에 URL/Path 에 주소를 넣는다.
	그리고 현재 연결하는 원격저장소가 로컬저장소의 기본저장소라는 것을 명시해 주려면 Default Remote 를 체크해준다.
	그러면 origin 이라는 원격저장소의 별칭이 자동으로 입력된다.


## 원격 저장소에 버전 올리기(Push)
	Push를 누르고 원격저장소를 선택한다.
	그리고 업로드하려는 로컬저장소의 브랜치와 원격저장소의 브랜치를 선택하고 
	Push를 한 뒤 Authenticate 창이 뜨면 해당 원격저장소가 있는 아이디와 패스워드를 입력해준다.
	그러면 자동으로 origin/master 브랜치가 보이게 된다.
	origin/master 는 origin 원격저장소의 master 브랜치를 뜻한다.
	
	원격저장소와 연결 한 뒤 
	Commit 하게 되면 원격저장소와 로컬저장소간 버전의 차이(ahead)가 발생한다.
	이것을 소스트리에선 origin/master 와 master 로 브랜치를 나누어 보여준다.
	

## 원격저장소 복제(Clone Repository)
	Clone Repository 에서 URL/Path에 주소를 넣어주고
	Destination Path 에 로컬저장소가 만들어질 폴더를 정해준다.
	그리고 Clone 을 누르면 된다.


## 원격 저장소에서 버전 가져오기(Pull)
협업시 주의사항-1
	협업시 코딩을 하기 전에 습관적으로 Pull 하여 원격저장소의 소스를 로컬저장소로 가져와야 한다.
	그리고 코딩을 끝내고 Push하기 전에도 Pull을 하고 Push를 한다.
	

## 협업시 주의사항-2
	SourceFlower라는 로컬저장소에서 Push 를 하고
	SourceTree 라는 로컬저장소에서도 Push를 하면
	Git이 나중에 들어온 Push를 거절한다.
	그러면 Pull을 먼저한 다음 Push를 해야한다.
	
	Pull을 하게 되면 git이 자동으로 Merge를 실시하여 자동으로 버전을 하나 만든다.
	여기서 충돌이 없는지 확인하고 Push를 해야한다.


## 협업시 충돌해결
	Branch 간 Merge시 충돌해결 하는 것처럼 하면 된다.
	먼저 Push한 로컬저장소에서는 충돌이 일어나지 않았기 때문에 괜찮지만
	충돌이 일어난 로컬저장소에서는 충돌을 해결해야 한다.
	즉, 충돌을 해결하는 문제는 나중에 Push를 한 로컬저장소에게로 넘어간다.
	충돌을 해결할 때에는 협업자와 같이 해결하는 것이 좋다.


## 비교 및 병합도구 추가하기(diff)
	Tools - Options - Diff 에서 External Diff Tool 과 Merge Tool 을 추가한다.
	

## 완성하지 못한 기능을 Commit 하지 않고 저장해두기(Stash)
	Stash를 누르고 분별할 수 있는 이름을 써준다.
	그러면 이전 버전으로 돌아가면서 Uncommited Changes 는 사라지고
	좌측 Stashes 에서 저장된 소스를 볼 수 있다.
	중간에 다른 소스를 commit 해도 된다.
	그리고 다시 stashes 에서 적용시키면 Uncommited changes 가 나타난다.

	그리고 만약 Add를 하고 Commit 을 하지 않은 상태에서 다시 코딩을 한다음
	Stash를 하게 되면 Working copy 영역과 Staging Area 의 내용이 합쳐져서 Stash 된다.

	또한 Stash한 부분과 Stash하지 않고 다시 똑같은 영역에 코딩을 하면
	Conflict가 발생한다.


## 태그 추가하기(Tag)
	해당 버전을 누르고 태그를 누르면 
	Specified commit 에 자동으로 선택한 버전의 commit id가 입력된다.
	
	버전에 태그를 추가하면 
	Specified commit에 태그를 commit id 대신에 태그 이름을 적어도 된다.

	태그는 Push를 할 때 원격저장소로 올리고 싶으면
	Push 할 때 Push all tags 체크박스를 체크해준다.


## Add할 필요없는 파일 Working Copy에서 제거하기(Ignore)
	해당 파일에 Ignore 를 클릭
	
	Ignore exact filename : 정확히 해당 파일만을 무시
	Ignore all files with this extension : 해당 파일의 확장자를 무시
	Ignore everything beneath : 
	Ignore custom pattern : 커스텀 패턴을 정해서 무시할 수 있다.

	Ignore 를 하고나면 .gitIgnore 라는 파일이 생긴다.
	그 안에는 무시할 파일들의 패턴이 적혀있다.
	그리고 .gitIgnore 를 원격저장소로 같이 commit 하고 push 하게 되면
	같이 협업하는 사람들도 .gitIgnore 를 가지고 있으므로 똑같이 무시하게 된다.

	www.gitignore.io 라는 사이트에서 .gitIgnore 파일을 만들어준다.


## 소스트리에서 연동된 계정정보 바꾸기
	View - Show Hosted Repositories - Edit Accounts
