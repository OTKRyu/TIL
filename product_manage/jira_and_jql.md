# jira

## 이슈타입

- story : 유저의 사용경험
- task : 스토리에 따른 해야할 일들
- bug : 말 그대로 버그
- epic : 하나의 큰 틀 

### 이슈별 정보

- 제목
- 상태
- 담당자
- 컴포넌트(팀, 개발 단위, 혹은 에픽으로 두긴 어려운 묶음 등등)

## jql

- jira query language
- 지라 이슈를 구조적으로 검색하기 위해 제공하는 언어
- sql과 유사한 문법
- 지라의 필드에 맞는 특수한 예약어 제공
- issue들을 재가공해 유의미한 데이터를 도출해 내는데 활용

### jql operators

- =, !=, >, >=
- in, not in
- ~(contains), !~
- is empty, is not empty, is null, is not null
- relative dates : 현재 날짜를 기준으로 1d, -1d, 1w, -1w처럼 날짜를 검색할 수 있다.

### jql functions

- endOfDay(), startOfDay() 
- endOfWeek() (Saturday), startOfWeek() (Sunday)
- endOfMonth(), startOfMonth(), endOfYear(), startOfYear()
- currentLogin() : 가장 최근에 로그인한 시점
- currentUser()

### jql 활용

- jql filter share

## dashboard

- add gadget
  - 만들어놓은 filter를 통해 gadget의 데이터를 할당할 수 있다.

### board

- scrum board
- kanban board

## 현업에서의 활용

- 이슈 트래킹
- 리포, 호스팅
- 코드 리뷰
- 빌드 및 배포
- 지식 공유
- 그 외에도 다양한 플러그인