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