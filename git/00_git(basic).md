# git
## 시작
- `git init` : 폴더를 깃에 올릴 수 있는 폴더로 변경
- `git remote origin 'https~'` : 깃 폴더를 온라인 상의 어디와 연결할 것인지를 설정

## 조회
- `git log` : commit들을 보여줌
- `git reflog` : commit들과 HEAD의 이동들을 축약해 보여줌

## 입력
- `git add 'foldername'` : 대상을 staging
- `git commit -m 'message'` : staging된 것들을 문구와 함께 commit
- `git push origin 'branchname'` : origin으로 branchname에 저장된 commit들을 업로드

## 출력
- `git pull` : clone으로 가져온 git 폴더의 commit 내용을 받아옴
- `git clone 'https~' : 웹상의 git폴더를 내 컴퓨터에 폴더로 받아옴

## 수정
- `git restore` : staging된 내용을 되돌림
- `git reset` : commit된 내용을 되돌림
	- 함부로 쓸 경우 push되어 origin에 올라간 내용과 log의 내용이 차이가 날 위험이 존재

## branch 관련
`HEAD` : 커밋을 기준으로 봤을 때 내가 지금 있는 곳

- `git branch` :  branch 조회
- `git branch -d 'branchname'` : branch 삭제
- `git branch -D 'branchname'` : branch 강제 삭제
- `git branch 'branchname'` : branch 추가
- `git branch -c 'branchname'` : branch 생성과 이동
- `git merge 'branchname'` : branch상에 되었던 커밋을 master에 병합
	- branch가 하나였을 경우 master가 branch가 나간만큼 따라가면서 병합
		 - 이를 패스트포워딩이라고 함
	- branch가 여러 개일 경우 각 branch가 가지고 있는 수정 사항들을 합쳐 하나의 commit을 새로 만들고 거기로 병합
	- 같은 폴더를 다른 방식으로 여러 branch에서 동시에 고친 후 merge를 하면 conflict가 일어나서 병합이 안 되었다고 알려주며, 이를 해결하라고 표시를 해줌
		- vscode가 이를 좀 편하게 할 수 있도록 편의기능을 제공함
	- 위의 병합을 사용했을 때 각 branch의 commit들을 모두 가지고 오고 시간순으로 배열해 보여줌.
- `git switch(checkout) 'branchname'` :  branch와 master를 이동할 때 사용
