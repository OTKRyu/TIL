# Json web token

## structure

- header
  - 알고리즘과 타입
- payload
  - 데이터
- signature
  - 검증용

## property

- 이 것들은 모두 encode된 것 뿐이기 때문에 decode하면 정보를 읽을 수 있다.
- self-contatined한 속성(jwt자체가 필요한 정보를 들고 있음)
- stateless하다(백엔드 서버가 바뀌더라도 인증용 키가 같다면 똑같이 작동한다.)
  - scale out하더라도 대응이 가능하다.(서버를 변경하더라도 똑같이 작동가능)
  - 비밀번호를 다시 입력할 필요가 없음
  - validation check만으로 검증이 가능함(다만 그럼에도 불구하고 white list 방식의 추가 검증을 하는 경우도 있음)
- 모바일 환경에서 다시 로그인할 필요가 없다.
- access token
  - 짧은 생명 주기
  - 서버로 요청을 할 때
- refresh token
  - 긴 생명 주기
  - access token이 만료되면 refresh token을 통해 새로운 access token 발행(서버와 클라이언트의 시간차이로 인한 이슈가 있을수 있다.)
- 이런 구조인 이유는 stateless하기때문에 access token만으로 인증이 가능하기 때문(서버에서 들고 있을 필요가 없다)
- access token이 탈취당하는 경우 공격자는 사용자와 동일한 권한을 갖게 되기 때문에 jwt를 사용하는 경우 반드시 ssl을 이용한 암호화 통신을 사용해야 한다.