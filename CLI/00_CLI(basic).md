# Basic CLI

## 뜻

Command-line interface의 약자로 말그대로 명령어 한줄씩 써서 작동하는 접점을 의미한다.

## 폴더(directory) 관련

폴더 상징 특수기호

- `~`
  - Home directory
  - /c/users/(name)
- `/`
  - Root directory()
- `.`
  - 현재 위치
- `..`
  - 상위 폴더
- `-`
  - 왔던 경로상의 이전 경로(뒤로가기)

## 명령어

- `cd[대상폴더]`
  - 대상폴더로 이동
- `ls`
  - 폴더 내부 항목 리스트업
- `rm`
  - 삭제
  - -r dirctory 삭제
  - -a 하위내용 전부 삭제
  - -f 강제 삭제
- `mkdir`
  - 디렉토리 생성
  - `-p`를 붙여서 하면 경로상에 해당하는 모든 폴더를 만들면서 진행한다. 즉 없는 폴더 안에 폴더를 만든다 하더라도 직접 만들면서 진행한다.
- `touch`
  - 생성
- `mv filelocation/ dirlocation/`
  - filelocation에 있는 파일을 dirlocation으로 이동시킨다.
  - 혹은 같은 자리에서 이름만 바꾼다면 이름을 변경하는 명령어이기도 하다.
- `start`
  - 기본적으로 연결되어 있는 프로그램을 이용해 해당 프로그램을 실행한다.
  - 폴더에 쓸 경우, 그 폴더를 gui로 열어준다.
- `which` 
  - 지금 부르는 프로그램의 실제 위치가 어디인지 알려준다.
  - ex) which python
- `python -m venv name`
  - 파이썬을 name이란 이름으로 가상환경에 새로 만드는 명령어, 특별한 이유가 없으면 venv라는 이름으로 짓는다
  - 만들고 나서 아무런 말도 안 하면 여전히 global 경로로 연결되어 있다.
  - 그리고 이렇게 파이썬 파일을 복제해 오게되면 git repo인 경우 새로운 변경사항으로 인식하기 때문에 이를 무시하게 만들기 위한 작업을 추가로 해줘야한다.
- `source name/Scripts/activate`
  - 정확히는 이 위치에 해당하는 파일을 실행하는 명령어이다.
  - 다만 이를 실행시키면 python을 실행시켰을 때의 경로를 이쪽으로 변경시켜준다.
  - 단순하게는 가상환경을 활성화하는 것이다.
- `deactivate`
  - 활성화되어있는 가상환경을 비활성화한다.
- `ctrl + l` 
  - 터미널에 있는 기록들을 날리고 새롭게 시작한다.
- `ctrl + c`
  - 터미널에 진행되고 있던 작업을 중단시킨다.
- `ctrl  + d`
  - 터미널 자체를 닿아버린다.
- `cp 대상경로 복사할경로` 
  - 대상경로에 있는 파일을 복사할 위치에 복사한다.
  - `-r` 옵션을 주면 대상 폴더에 있는 모든 파일을 그대로 가져온다.
- `grep 'objectstring' targetstring` : 파일 안에서 objectstring에 해당하는 내용이 있는지를 찾아서 출력해 준다.
  - global regular expression print
- `|` : 리눅스에서 앞의 파일을 뒤의 명령의 삽입 장소에 넣어준다. ex)`pip list | grep 'django'`
- `curl` : git bash에서 제공하는 명령어로 뒤에 method와 url을 작성해서 실행하면 요청을 실제로 보낸다.
- `pwe` : 현재 어디서 작업하고 있는지 위치를 반환