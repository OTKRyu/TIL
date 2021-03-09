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
- `touch`
  - 생성
- `mv filelocation/ dirlocation/`
  - filelocation에 있는 파일을 dirlocation으로 이동시킨다.
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
