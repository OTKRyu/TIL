

# vue

## server and client

- client
  - 서비스 요청
  - 서버가 원하는 방식에 맞게 인자를 제공
  - 사용자에게 적절한 방식으로 표현

## cors

- same-origin policy(sop)

  - 특정 출처에서 불러온 데이터가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 보안 형식
  - 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임
  - 정의
    - 두 url의 protocol,port,host가 모두 같아야 동일한 출처라고 할 수 있다.

- cross origin resource sharing(cors)

  - 추가 http header를 사용하여, 특정 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 이쓴 권한을 부여하도록 브라우저에 알려주는 체제

  - 보안 상의 이유로 브라우저는 교차 출처 http요청을 제한(sop)

  - 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 cors header를 포함한 응답을 반환해야한다.

  - corsp: cors상황에서의 정책(policy)

  - why cors?

    1. 브라우저 및 웹 애플리케이션 보호
       - 악의적인 사이트의 데이터를 가져오지 않도록 사전 차단
       - 응답으로 받는 자원에 대한 최소한의 검증
       - 서버는 정상적으로 응답하더라도 브라우저에서 차단
    2. 서버의 자원 관리
       - 누가 해당 리소스에 접근할 수 있는지 관리 가능

  - how cors?

    - cors 표준에 의해 추가된 http header를 통해 이를 통제
    - 요청의 origin항목을 보면 어디서 요청이 왔는지 알 수 있음
    - access control allow origin 

      - 응답이 주어진 출처로부터 요청 코드와 공유될 수 있는지를 나타냄
    - 예시
      1. 서버로 요청
      2. 서버에서 header에 cors에 관한 정보를 담아 응답
      3. 브라우저에서 이를 읽고 허용이 되는 지 여부를 결정
    - 프레임워크별로 이를 지원하는 라이브러리가 있다.
      - django의 경우 django cors headers라는 라이브러리가 있다. 자세한 사용법은 공식문서 참조
## authorization

- 권한 부여, 허가

- 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정

- 보안 화경에서 권한 부여는 항상 인증을 따라야 함

- 인증을 받았어도 할 수 있는 권한은 다를 수 있음  
- 만약 문제가 생기면, 403이 발생 사용자가 누구인지는 아는 상태
- 대부분 인증과 권한 생성을 묶어서 구성함
- 물론 인증을 한다고 같은 권한을 부여받는 것은 아님

## authentication

### session based

### token based

- json web token

  - json 포맷을 활용하여 요소 간 안전하게 정보를 교환하기 위한 표준 포맷

  - jwt 자체가 필요한 정보를 모두 갖기 때문에(self-contained) 이를 검증하기 위한 다른 검증 수단이 필요하지 않다.

  - why jwt?

    - session에 비해 상대로적으로 html, http환경에 사용하기 용이
      - session 테이블을 따로 관리할 필요없음
      - 다만 session처럼 서버에서 관리하지 않으므로, 유효기간이 지나서 사라지는 것외에 서버에서 jwt를 없앨 방법이 없음. 이런 점에서는 보안이 취약함
    - 높은 보안 수준
    - json의 범용성
    - server에 테이블이 없어도 되므로 서버 자원 절약 가능
  

  - jwt 구조
  
    - header
      - 어떤 알고리즘을 이용해 hashing했는지 등의 정보를 담는다.
    - payload
      - 토큰에 넣을 정보로 claim단위로 이루어져 있다.
      - 여러개 넣을 수 있다.
    - signature
      - header와 payload의 encoding 값을 더하고 거기게 private key로 만들어 함께 보냄
      - 장고의 경우 프로젝트의 secret_key를 jwt생성의 private key로 사용한다.
  - jwt 구동과정
  
    1. 로그인
    2. 서버에서 jwt발급 및 클라이언트에서 저장
    3. 클라이언트에서 서버에 접근할 때마다 jwt동봉
  - django rest frame jwt를 사용해 구현할 수 있다. 자세한 사용법은 공식문서 참조
  - 이렇게 작성된 jwt가 유효한 지를 보는 기능이  `rest_framework_jwt.authentication`에 있다.
  - `isAuthenticated`를 통해 유효한 유저인지를 확인하는 것도 가능한다.
  - 그리고 `rest_framework`에 decoration을 보면 authentication_classes라는 decorator가 있고,이를 쓰면 유효한 jwt인지, 유효한 user인지 확인할 수 있다.
