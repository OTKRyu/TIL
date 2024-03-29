# secure coding

## OWASP TOP 10

- xss(cross site scripting)
  - 가해자로부터 받은 내용을 검증하지 않을 경우, 이 악성코드를 웹사이트에 그대로 삽입하게 됨.
  - 그 후 다른 사용자의 브라우저에 띄우는 것으로 공격하는 방법
  - 쿠키나 세션등을 탈취할 수 있게 된다.
  - 이를 방지하는 방법은 여러 가지가 있지만 제일 기본적인 것은 innerHTML을 조심해서 사용하는 것이다. 사용자에게 받은 내용은 innerHTML에 쓰지 않는 편이 좋다.
  - backend django의경우 xss-filter와 같은 middleware에서 걸러줄 수 있다.
  - axios를 쓸 때, query보다 path variable로 쓰게 되면 script와 같은 것을 막을 수 있다.
- csrf
  - 가해자는 일단 유효한 인증을 받음
  - 이 유효한 인증을 토대로 피해자에게 피싱 사이트로 유도하고 피해자가 이에 걸릴 경우 거기서 추출한 정보로 서버에 접근함
  - 유효한 인증을 토대로 했기 때문에 요청을 유효하다고 서버가 판단하게 되고, 그 결과를 가해자가 가로챌 수 있게 된다.
  - backend django의 경우 csrf middleware를 쓰거나 웹사이트를 만들때 csrf token를 첨가하는 방법도 있다.
  - 가장 좋은 것은 jwt나 https를 사용하여 도중에 다른 사람이 가로챌 수 없도록 만들어놓는 것이다.

## 분석

- 정적 분석
  - 어플리케이션 소스 코드
  - 패턴 비교 분석
  - 어플리케이션 개발 시점
  - 소스의 라인 별 결가 표시
  - 대표적으로 소나큐브가 있다.
    - 기본적으로 나타날 수 있는 취약점이나 버그를 분석해준다.
    - 그리고 버그나 취약점에 대한 등급을 나눠 점수를 매겨서 보안상태가 얼마나 잘 되어 있는지를 알려준다.
- 동적 분석
  - 실제 어플리케이션 운영 서버
  - HTTP 메세지의 변경 점검
  - 어플리케이션 운영 시점
  - http 요청/ 응답에 다른 결과 표시