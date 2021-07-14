# git

멀티 플레이시

- 정확한 커밋 컨벤션

## git flow

- master(출시용)
- hotfix
- release
- bugfix
- develop(개발용)
- feature

### git flow 시작하기

- 시작 명령어 : `git flow init`
- 추가 기능 개발시 명령어 : `git flow feature start sign_up`
- 끝나고 develop branch에 병합 : `git flow feature finish sign_up`
- 다만 처음부터 이를 사용하는 것은 추천하지 않음

## trunk based

- master
- realease
- features(short-lived branches)
- keep fresh master branch

## .gitignore 관리

- 키값 등 올라가면 안 되는 것들 안 올라가게 조심해야한다.

## git 명령어 연습

https://learngitbranching.js.org

git with d3

pro git