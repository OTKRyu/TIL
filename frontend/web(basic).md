# web

## basic

-  ip

 django로 서버를 열면 기본적으로 설정되는 주소가 127.0.0.1인데 이것은 ip이며 이 ip는 내 컴퓨터라는 것을 의미한다.

-  port 

컴퓨터에 신호가 들어올 수 있는 창구가 3만개 정도 있는데 이 중 어떤 포트로 이 요청을 보낼 것인지를 말한다.

대부분의 프로그램이나 브라우저에서 기본값을 가지고 있다.

## server, client

현재 공부는 다 client입장에서 가상 server를 만들어서 하고 있지만, 실제로 server를 열어야할 경우 현재 컴퓨터에 해놓은 것처럼 client용으로 짤 수가 없다

즉 세팅을 새로 해줘야하는데  이 세팅을 하는 것이 만만하지 않다. 어떤 라이브러리가 어떤 일을 하는 지 정확히 파악하는 것이 어렵기 때문이다.

이를 방지하기 위해 가상환경을 설정하고, 프로젝트 때마다 이를 위한 환경을 가상의 공간에만 저장하여 서로간의 프로젝트가 영향을 끼치지 않게 하는 것이다. 이렇게 해두면 나중에 서버로 옮길 때에도 필요한 것을 명확하게 가를 수 있으며, 동시에 여러 프로젝트를 진행하더라도 서로의 라이브러리가 오염되는 경우도 막을 수 있다.

그래서 어떤 프로젝트를 진행하더라도 그 프로젝트마다 가상환경이 하나씩 필요하다.

그리고 이 프로젝트 하나당 하나의 git repo를 만든다.

이 때 파이썬의 경우 가상환경이 필요로 하는 파이썬 라이브러리가 뭔지 적어놓는 곳을 requirements.txt로 하는 것이 관습이고, pip -r 이란 명령어로 이 내부에 있는 명령어를 읽어서 다 설치를 할 수 있다.

## HTTP(hyperText Transfer Protocol)

html 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜

특징

- 비연결 지향(connetionless) : 서버는 응답 후 접속을 끊음
- 무상태(stateless): 접속이 끊어지면 통신이 끝나고 상태를 저장해놓지 않는다.

통신 프로토콜로 이에 대한 반응을 정보(status code)로 되돌려 주는데, 그 반응에 대한 분류를 적는다. 상세한 내용은 웹표준을 정하는 곳에 자세히 적혀있으니 몇 번대인지 보고 뭐가 잘못인지정도를 아는 정도면 된다.

- 100 :  지나가는 것들
- 200 : 기본적으로 성공적으로 수행됐을 때 나오는 메세지이며, 200번대에 있는 반응은 모두 성공에 대한 것이다.
- 300 : 기본적으로 리다이렉트와 관련된 정보를 주는 메세지이다.
  - 302 : 리다이렉트되어서 현재의 페이지에 왔음을 알려주는 메세지이다.
- 400(bad request) :  기본적으로 실패했을 때 나오는 메세지이다. 다만 이 때는 책임소재라고 부를만한 것이 클라이언트에 있다.
- 500 : 기본적으로 실패했을 때 나오는 메세지이다. 다만 이 때는 책임소재라고 부를만한 것이 서버에 있다.

## socket

http와는 다른 형식의 통신 방법이다.

실시간으로 해야될 때 쓴다. request와 response로 이루어진 한 사이클이 아닌 request없어도 response를 지속적으로 보내줘야하는 상황에 쓰게 된다.

ex) 채팅, 실시간 게임, 스트리밍 등등

## cookie

서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각

브라우저가 이 전송받은 쿠키를 key-value의 데이터 형식으로 저장, 동일한 서버에 재 요청 시 저장된 쿠키를 함께 전송

즉 웹 페이지에 접속하면 그 웹 페이지에 해당하는 쿠키를 로컬에 저장, 그 후 재요청마다 쿠키를 함께 전송

### 사용 목적

- 세션 관리 : 로그인, 아이디 자동완성, 공지 하루 안보기, 팝업 체크, 장바구니 등의 정보 관리
- 개인화 : 사용자 선호나 테마 등의 세팅
- 트래킹 : 사용자 행동을 기록 및 분석하는 용도

### 일생

1. session cookie
   - 세션이 종료되면 삭제
   - 브라우저가 현재 세션이 종료되는 시기를 정함
   - 일부 브라우저는 다시 시작할 때 세션 복원을 사용해 지속될 수 있도록 함
2. permanet cookie
   - expires 속성 혹은 max-age 속성에 끝날 시점을 명시해두고 이 시점이 지나면 만료

## Session

- 사이트와 특정 브라우저 사이의 상태를 유지
- 서버에서 할당해준 session id를 쿠키에 저장해놓고 통신시마다 보냄으로서 이를 수행
- 이 session id는 유저의 로그인 상태를 확인할 때 쓰기 때문에 서버에서도 저장함

## browser

### 단축키

- `f5` : 새로고침
- `ctrl + shift + f5` : 강력 새로고침(캐쉬에 저장된 정보도 모두 날리고 새로고침)