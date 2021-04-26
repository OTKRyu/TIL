# django

## rest api

representational state transfer의 약자

결국 자원을 위치로 잘 표기하고, 그 자원에 대한 행위는 method로 표기한다는 것이 핵심이다.

대체로 이런걸 작성하게 될 일은 현업에 나갈 때나 아니면 코테나 면접용 과제로 나왔을 때일 건데 이럴 때는 db를 미리 만들어서 줄 가능성이 있다.

이런 때에는 장고가 제공하는 기능인 inspectdb라는 기능을 통해  db에 담겨있는 테이블의 구조를 model로 뽑아낼 수가 있다. 이렇게 이미 데이터가 들어있는 데이터베이스를 legacy database라고 한다.

혹은 그 이상을 요구한다면 로딩속도에 관한 것일 때는 데이터베이스 정규화일 가능성이 높다. 여기까지 요구할 일은 잘 없지만 이게 뭔지는 알고 있는 것이 좋다.

다만 sqlite가 아닌 db를 받았을 경우 settings.py에 들어가서 어떤 db를 대상으로 할 건지를 바꾸어주어야한다.

- 웹 설계 상의 장점을 활용할 수 있는 아키텍처 방법론

- 네트워크 아키텍쳐 원리의 모음

  - 자원을 정의(resource)
  - 자원에 대한 주소를 지정하는 방법
  - 결국 자원과  주소를 어떻게 잘 정할 것이냐에 대한 방법

- rest 원리를 따르는 시스템 혹은 api를 restful api라고 하기도 함

- 구성

  - 자원(uri)

    - uniform resource identifier
    - 통합 자원 식별자
    - 인터넷의 자원을 나타내는 유일한 주소
    - url의 상위 개념
      - uniform resource locator
      - 통합 자원 위치
      - 네트워크 상에 자원이 어디 있는지를 알려주기 위한 약속
      - 다만 urn을 사용자가 쓸 일은 거의 없기 때문에  uri와 url을 혼동하여 쓰기도 한다.
    - urn의 상위 개념
      - uniform resource name
      - 통합 자원 이름
      - url과 달리 자원의 위치에 영향을 받지 않는 유일한 이름의 역할을 함
      - 자원의 위치를 옮겨도 문제없이 동작
      - ex)ISBN
    - url과 urn의 비교
      - url은 자원의 주소, urn은 자원의 고유 이름
      - 자원의 위치가 변하면 url은 수정해야하지만 urn은 변하지 않음
      - urn은 자원의 id를 정의, url은 자원을 찾아갈 방법을 제공
    - uri의 구조 : (scheme/protocol)://localhost:port/path
    - ex) http://google.com/search?q=http(query)
    - ex) http://getbootstrap.com/docs/4.1/layout/overview#contatiners(fragment)
    - 대체로 path까지는 url이라고 보고, 그 후의 쿼리나 프래그먼트는 uri의 영역이라고 본다
    - 주의사항
      - 가독성을 위해 밑줄 대신 하이픈사용
      - 대소문자 구분을 인식하기 때문에 소문자로 통일
      - 파일 확장자는 포함시키지 않음

  - 행위(http method)

    - 자원에 수행하길 원하는 행동
    - 의미론적으로 행위를 규정하는 것이라 실제로 그 행위 자체가 수행되는 걸 보장하진 않음
    - http verbs라고도 함
    - 종류
      - GET
        - 특정 자원의 표시를 요청, 데이터를 받기만 함
      - POST
        - 서버로 데이터를 전송, 서버에 변경사항을 만듦
      - PUT
        - 요청한 주소의 자원을 수정
      - DELETE
        - 지정한 자원을 삭제

  - 표현(representations)

    - json 

      javascript object notation

      - lightweight data-interchange format
      - 자바스크립트 객체 문법을 따름, 구조화된 데이터를 표현하기 위한 문자 기반 데이터 포맷
      - 일반적으로 웹 어플리케이션에서 클라이언트로 데이터를 전송할 때 사용
      - 자바스크립트 문법에 기반을 두고 있지만 차이가 있으니 주의
      - 특징
        - 사람이 읽고 쓰기가 쉽고 기계가 해석 및 분석
          (parsing)하고 만들어 내기 쉬움
          - 파이썬의 dict,  자바스크립트의 object처럼 c계열 언어가 가진 구조로 바꾸기 쉬움

### api

프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스, 즉 어플리케이션과 프로그래밍으로 소통하는 방법

프로그래밍을 통해서 기능을 수행할 수 있게 해주는 것

- web api
  - 웹 어플리케이션 개발에서 다른 서비스에 요청하고 응답을 받기 위해 정의된 명세
  - 요즘은 open api를 활용하는 추세
  - json형태로 응답하는 편이다.
- api server
  - 요청을 받아 응답으로 json과 같은 데이터를 보내는 서버.
  - 데이터만으로는 웹서비스가 불가능하므로, vue.js같은 프론트엔드 프레임워크를 사용하여 이 json를 적절히 변환하여 보낸다.

