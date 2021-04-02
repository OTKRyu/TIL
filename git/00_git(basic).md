# git
## convention

git은 버전 관리 프로그램이므로 결국 버전 자체를 쓰는 컨벤션도 필요하기에 여기에 적는다.

대체로 appname.major_change_count.minor_change_count.patch_or_bugfix_count

## 시작
- `git init` : 폴더를 깃에 올릴 수 있는 폴더로 변경
- `git remote add origin 'https~'` : 온라인 상의 주소를 origin이라는 이름으로 저장
- `git config user.name` : 깃 폴더에 찍을 인장의 내용중 이름을 설정
- `git config user.email ` : 깃 폴더에 찍을 인장의 내용 중 이메일을 설정
  - 공통적으로 --global 옵션을 주면 공통적으로 계속 쓸 고정된 인장을 생성.

## 조회
- `git log` : commit들을 보여줌
  - `--oneline` 이라는 추가 옵션을 붙여주면 한줄로 조금 더 깔끔하게 보여줌.
  - `--graph`이라는 추가 옵션을 주면 로그와 함게 브랜치가 어떻게 움직였는지 그래프를 보여줌
- `git lg` : 요약된 commit들을 보여줌
- `git reflog` : commit들과 HEAD의 이동들을 축약해 보여줌
- `git remote -v` : 저장된 remote들을 보여줌
- `git status` : git 폴더 안에 있는 파일들이 어떤 상태인지를 보여줌

## 입력
- `git add 'foldername'` : 대상을 staging
- `git commit -m 'message'` : staging된 것들을 문구와 함께 commit
- `git push origin 'branchname'` : origin이란 branchname에 commit을 업로드

## 출력

출력의 경우 데이터를 가져가는 행위이므로 오픈 소스가 아니라면 제약이 걸리기 마련이다. 대부분의 경우 계정을 요구한 후 그 계정의 권한만큼 가져갈 수 있게 되어있는 편인데,  이 때 window에서 내가 이런 과정을 거치는 것을 보고 이 정보를 컴퓨터 내에 저장을 해놓는다.

그래서 그 다음부터는 자동적으로 계정을 보내게 되는데 이런 정보가 저장되어 있는 곳이 자격 증명 관리자이다.

- `git pull remotename branchname` : 리모트의 브랜치에 해당하는 내용을 현재 작업중인 브랜치 위로 가져오게 된다. 이 때 순조롭게 병합이 되지 않는다면 merge때의 절차를 그대로 밟게 된다.
  - clone으로 가져온 git 폴더의 경우 origin master가 기본적으로 세팅이 되어 있으므로 `git pull`만 쳐도commit 내용을 자동으로 가져오게 된다. 
  - 이 때 중요한게 자신이 있는 branch 위치다. master 브랜치에서 이를 진행하면 master 브랜치로 가져와 병합하려 시도하게 된다. 이 때 local에서 merge할 때와 동일한 절차를 밟게 된다. 이 절차가 끝나고 commit을 한 번 남겨주면 그제서야 pull이 완료되게 된다.
- `git clone 'path' 'dirname'` : 웹상의 있는 깃 리포를 그대로 본 떠 내 working directory로 가져온다. 이렇게 할 경우 remote origin이 기본적으로 clone을 뜬 주소로 설정된다. 추가적으로 이렇게 가져올 시 웹상의 깃 리포의 이름을 본 따 폴더를 만들고 그 안에 깃 리포를 복제해 넣어준다. dirname을 적어주면 dirname으로 폴더를 생성한다. 

## 수정
- `git restore` : staging된 내용을 되돌림, 이는 이미 commit된 적이 있는 파일을 staging이 되기전으로 돌리는 명령어다. 

- `git reset` : commit된 내용을 되돌림
	
	- 함부로 쓸 경우 push되어 origin에 올라간 내용과 log의 내용이 차이가 날 위험이 존재
	- 기본값은 `--mixed latestcommithash` 이다
	- `--hard commithash`  옵션을 줄 경우 파일까지 완전히 commithash때의 상태로 돌려버린다.
	- `--soft commithash` 옵션을 줄 경우 commithash때의 상태로 commit들은 되돌아가나, 실제 파일에 적용하지는 않는다. 변경사항은 staging에 저장은 해놓는다.
	- `--mixed commithash` 옵션을 줄 경우 commithash때로 돌아가나 soft와 마찬가지로 실제 파일에는 적용하지 않고, 변경사항은 workign directory에만 남아있게 된다.
	
- `git revert commithash` : 할 경우 reset처럼 commithash를 기준으로 돌아가게 만들어줄 수 있으나, commit 기록들은 남기고 commithash 당시의 파일과 현재의 파일을 merge시키는 것처럼 움직인다. conflict를 해결해주고 나서 다시 커밋을 해주면 reverting이 완료된다. 사실상 commithash당시의 파일 상황대로 working directory를 만든 뒤 새 커밋을 찍어주는 것과 같다. 다만 프로젝트가 커지면 commithash 당시의 파일 상황이 어떤지를 아는것이 쉬운 일이 아니므로 이런 기능이 필요하다.

- `git rm --cached filename` : staging된 내용 중 filename에 관한 것만 취소함, 다만 이는 이 파일에 대한 로그 자체를 날려버리는 명령어로 처음에 커밋도 하기 전에 staging된 내용을 내릴 때만 한다. 만약 이미 여러번 commit을 했는데 이 명령어를 쓰면 로그가 날아가 버린다.

- `git commit --amend` : 가장 최근에 한 commit내용을 staging단으로 끌고 내려와서 수정할 수 있다. 고로 이 때 추가로 staging할 내용이 있다면 미리 staging해둔다면 알아서 commit내용에 추가되게 된다. 이 때 수정을 vim이란 프로그램을 통해서 하게된다.  

  ### vim

  git에서 쓰는 수정용 프로그램이다.

  편집기 이므로 열고싶은 파일을 vim filename으로 연다.
  
  연습을 해보고 싶다면 openvim.com이나 vim-adventures.com(2단계까지인가 무료) 같은 사이트에서 해볼 수 있다.
  
  - `i` : insert mode로 변경
  - `esc` : 명령모드로 복귀
  - `q`  : 종료 명령
  - `w` : 저장 명령
    - wq : 저장 후 종료
    - x : wq와 같다.
  - `dd` : 지우기 명령
  - `!` : 강제 명령으로 명령 뒤에 붙이면 강제로 실행된다.  
  - `hljk`: 왼오른위아래로 움직일 수 있다.

## branch 관련
`HEAD` : 커밋을 기준으로 봤을 때 내가 지금 있는 곳

- `git branch` :  branch 조회
- `git branch -d 'branchname'` : branch 삭제
- `git branch -D 'branchname'` : branch 강제 삭제
- `git branch 'branchname'` : branch 추가, 이 때 HEAD 기준으로 갈라져나온 branch임을 git이 인지를 함. 여기서 HEAD가 원류라고 볼 수 있다.
- `git branch -c 'branchname'` : branch 생성과 이동(branch 생성과 동시에 switch로 생성한 branch로 이동)
- `git merge 'branchname'` : branch상에 되었던 커밋을 내가 지금 있는 HEAD에 해당하는 branch와 병합함 
	- branch가 하나였을 경우 master가 branch가 나간만큼 따라가면서 병합
		- 이를 패스트포워딩이라고 함(fastforwarding)
	- branch가 여러 개일 경우 각 branch가 가지고 있는 수정 사항들을 합쳐 하나의 commit을 새로 만들고 거기로 병합, 이 경우를 recursion이라고 부름
	- 같은 폴더를 다른 방식으로 여러 branch에서 동시에 고친 후 merge를 하면 conflict가 일어나서 병합이 안 되었다고 알려주며, 이를 해결하라고 표시를 해줌
	  - vscode가 이를 좀 편하게 할 수 있도록 편의기능을 제공함
	- 위의 병합을 사용했을 때 각 branch의 commit들을 모두 가지고 오고 시간순으로 배열해 보여줌.
- `git switch(checkout) 'branchname'` :  branch와 master를 이동할 때 사용(switch와 checkout 둘 다 같은 명령어지만 git이 버전 업데이트를 하면서 이름이 좀 바뀐 것 뿐이다. 둘 다 사용 가능하다.)

## remote 관련

웹상에 올려놓은 branch로 local branch보다 상위 권한을 지니며 결국 remote의 branch를 기준으로 일을 진행하게 된다.


