# django

## how to start

1. 프로젝트를 생성하려는 디렉토리에서 프롬프트
2. 빈 폴더(프로젝트 root)를 만든다.(헷갈리지 않기 위해서 대문자로 쓰는 걸 권장)
   1. `.gitignore`
   2. `$ git init`
   3. `README.md`
   4. 원격 저장소 생성 후 연결
   5. `add` , `commit`, `push`
3. 해당 폴더로 이동해서 venv(가상독립환경)를 만든다.
4. venv를 활성화한다.
5. `$ pip install`을 통해 필요한 패키지들을 설치 한다.
6. `$ django-admin startproject <project name> . ` 명령어를 통해 프로젝트 초기화

## how to access project

반드시 프로젝트 루트의 안에 들어가서 vscode열기

혹은 프로젝트 루트를 우클릭하고 vscode 열기

## project venv setting

1. ctrl + shift + p
2. python: select interpreter 입력
3.  자동으로 venv폴더 안의 python을 잡는다면 그대로 진행
4. 아니라면 enter interpreter path, find를 통해 직접 Script폴더 안의 python.exe를 지정
5. 완료 이후 좌하단에 'python version('venv')' 문구 확인
6. 프롬프트에 venv 확인

## convention

- appname/new : 생성 html
- appname/create : 저장
- appname/ : 전체 조회
- appname/id/ : 단일 데이터 조회
- appname/id/edit/ : 단일 데이터 수정 html
- appname/id/update/ : 단일 데이터 수정
- appname/id/delete/ : 단일 데이터 삭제

## 추가 라이브러리

- django-seed 
  - 더미데이터를 만들어주는 추가 라이브러리
  - `python manage.py seed appname --number=num` : num개의 데이터를 appname에 해당하는 db에 생성해준다.