# regex

- 텍스트를 찾고 조작하는데 쓰는 문자열
- 텍스트 검색, 치환에 사용
- 수십 라인의 프로그래밍 없이 정규식 1~2줄로 대부부늬 문자열 작업 가능
- 사용 환경에 따라 조금씩 상이한 문법과 지원 여부
- sw 엔지니어에게 '외국어'같은 존재
- 다양한 문제 해결법
- 친숙해진다면 강력한 도구

## 추가 기능

- 전방 탐색
  - 일치 영역을 발견해도 그 값을 반환하지 않는 패턴
  - 실제로는하위표현시기며 가튼 형식으로 작서
  - (?=일치할 텍스트)
- 후방 탐색
  - 전방 탐색과 동일한 개념으로 방햐만 역방향
  - (?<=일치할 텍스트)
- 전후방 탐색
  - 후방부터 탐색 후에 전방 탐색으로 잘라주면 된다
- 부정형 전후방 탐색
  - 지정한 패턴과 일치하지 않는 텍스트를 탐색
  - 부정형 전방 탐색 (?!)
  - 부정형 후방 탐색 (?<!)
- 역참조 조건 사용
  - 정규 표현식 조건은 ?를 사용해 정의
  - 역참조 조건은 이전 하위 표현시기 검색에 성공했을 경우에 한하여 다시 해당 표현식을 검사
  - (?(역참조)true|false)

## 배울 수 있는 사이트

- https://regexr.com



