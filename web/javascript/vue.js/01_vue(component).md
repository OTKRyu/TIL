# vue

lts(long term service)

버전 바뀔 떄를 대비해 node version management라고 nvm이라는 것을 쓰게 될 수도 있다.


## sfc

- 컴포넌트(component)
  - 기본 html를 확장하여 재사용 가능한 코드를 캡슐화 하는데 도움을 줌
  - cs에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미
  - 유지보수를 쉽게, 재사용성의 측면에서도 좋음
  - vue 컴포넌트 === vue 인스턴스
- single file component
  - 하나의 파일이 하나의 컴포넌트역할을 함.
  - 화면의 특정 영역에 대한 html,css,javascript를 하나의 파일에서 관리

## vue cli

vue.js 개발을 위한 표준 도구

표준 tool 역할을 해줌

- node.js
  - 자바스크립트를 브라우저 외의 환경에서도 구동할 수 있도록 하는 js 런타임 환경
  - 크롬 v8 엔진으로 구동, 여러 os 환경에서 실행할수 있는 환경을 제공
  - ssr에서도 사용 가능하도록 함.
- node package manage
  - 자바스크립트 언어를 위한 패키지 관리자
    - pip와 같은 역할을 함
- vue cli 관련 명령어
  - `npm install -g @vue/cli` : 설치
  - `npm install` : npm을 실행한 위치의 pakage-lock.json을 찾아내서 그 환경과 같게 만들어준다.
  - `vue --version` : 버전 확인
  - `vue create project-name ` : 프로젝트 생성, 될 수 있으면 vscode의 터미널을 이용
    - 생성시 선택해야되는 옵션이 몇 가지 있다. git bash의 경우 interactive하지는 않기 때문에 설정시에도 명령어로 해야해서 vscode의 터미널을 이용하는 방법을 추천
    - vue3가 아직 완성되지는 않았기 때문에 vue2를 선택할 것을 권장
    - 생성시 폴더에 묶여서 만들어지기 때문에 한 칸 앞으로 이동하고 작업을 시작해야함
    - 깃과 연동되어 프로젝트를 만들자마자 git폴더로 만들어져있고 gitignore도 생성되어 있다.
  - `npm run serve` : 서버를 켜는 명령어
- babel 
  - javascript Transcompiler
  - 자바스크립트 신버전 코드를 구번전으로 번역해주는 도구
  - 파편화의 영향으로 브라우저마다 작동여부를 확신할 수 없기 때문에 이를 보장하기 위해 사용
- webpack
  - static module bundler
  - 모듈 간의 의존성 문제를 해결하기 위한 도구
  - 원래는 js는 모듈 없이 작성되었으나 어플리케이션이 복잡해지고 크기가 커지자, 라이브러리나 모듈을 작성하여 작성하려고 시도했다.
  - 현재는 대부분의 브라우저와 node.js가 모듈 시스템을 지원
  - 다만 이렇게 되면 문제가 모듈간의 의존성이 점점 커지면서 이 관계를 파악하지 못하면 제대로 디버깅을 할 수 없게 된다. 이를 해결하기 위한 것이 webpack이다.
  - bundler란 모듈 의존성 문제를 해결해주는 작업인 bundling을 해주는 도구를 말하고 webpack은 그중 하나이다.
  - 모듈들을 하나로 묶어주고 순서에 영향을 받지 않고도 작동하도록 만들어줌.
  - 이를 통해 유지 보수 측면에서 나아짐
- vue router
  - 공식 라우터
  - 중첩된 라우트, 매핑 등등 라우팅에 필요한 기능을 구현하는데 도움을 준다.
  - vue cli에서 `vue add router` 해주면 된다.
  - 다만 router는 폴더의 구조를 바꾸기 때문에 처음에 할 게 아니면, 백업을 해놓고 설치해야한다.
  - 원래 router가 하는 것처럼 주소를 배정해주는 것처럼 보이지만 실은 한 페이지에서 바뀐 것처럼 보인 것 뿐이다.
  - 원리는 router가 장고에서 url에 따라 다른 반응을 한 것처럼 routes를 작성해놓고 요청에 따라 컴포넌트를 다르게 보이도록 렌더링을 해주는 것이다.
  - 추가적으로 숏컷을 제공하는데 /src를 @로 적을 수 있다.
  - 동적 인자 전달(variable routing)
    - url 동적 인자 전달을 위해 vue router에서 쓰는 방법은 :로 시작하고 들어온 인자를 어떤 이름으로 저장할 것인지를 써주는 것이다.
    - 이렇게 전달된 동적 인자는 $route.params에 key, value쌍으로 저장되어 있다. 이 요소도 vue instance에 직계자식으로 저장되어 있으니 이 사실을 기억하고 함수에서 이 요소를 찾아서 쓰면 된다. 
  - 실제 쓰이는 태그
    - router-link
      - a 태그지만 get 요청을 제거한 형태로 구성되어 있다.
      - 이름을 등록해놓으면 장고의 url처럼 렌더링시에 알아서 이름 찾아가게 만들 수 있다.
    - router-view
      - 위의 링크중 어디를 클릭했는지 확인하고 그에 맞게 렌더링을 한다.
  - history mode
    - html history api를 사용해서 router를 구현한 것
    - 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능을 지원, 이를 통해 뒤로가기나 앞으로 가기가 가능
  - 필요한 이유
    1. spa 이전
       - 서버가 모든 라우팅을 통제
       - 요청 경로에 맞는 html을 제공
    2. spa 이후
       - 서버는 index.html 하나만 제공
       - 이후 모든 처리는 html위에서 js코드를 활용해 진행
       - 요청에 대한 처리를 더이상 서버가 하지 않는다.
    3. 라우팅 처리 차이
       - ssr의 경우는 여전히 라우팅을 서버가 결정
       - csr의 경우 더 이상 서버로 요청을 보내지 않고 응답 받은 html 문서안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트를 렌더링, 라우팅에 대한 결정권을 클라이언트를 가짐
       - vue.js가 라우팅의 결정권을 가지고 있을 때 이를 편하게 해주는 라이브러리다.
  - components vs view
    - 사실 어떻게 써야된다고 딱 정해져있는 것은 아니나 아래와 같은 방법을 고려해볼만하다.
    - views
      - router에 매핑되는 컴포넌트를 모아두는 폴더
    - components
      - router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더
- vue project 구조
  - node-modules : 모듈 담아놓는곳
  - public : 
  - src : 실제로 코드 및 요소를 넣는곳
    - app.vue : 최상위 컴포넌트
    - components : 하위 컴포넌트들을 모아놓는곳, 이름은 PascalCase로 작성
      - 하나의 컴포넌트에는 내용을 담을 하나의 큰 태그가 필요함 대체로 div로 작성
      - components중에 하위컴포넌트가 없는 컴포넌트들은 The를 이름앞에 붙이는게 관습이다.
    - assets : 웹페이지를 구성하는데 쓰이는 static 데이터를 넣어놓는 곳
    - main.js : 뷰에 쓸 최상단의 js를 구성하는 곳. 
  - package.json : 프로젝트에 대한 메타데이터를 전부 넣어놓은 곳이로, 딱히 손 댈 일은 없다.
  - package-lock.json: requirements처럼 뭘 썼는지와 같은 정보를 담아놓는곳. 개발간에 동일한 환경을 유지하기 위한 정보를 담아놓는곳, 딱히 손 댈 일은 없다.
  - pass props & emit events
    - 부모 자식 컴포넌트간에 데이터를 주고 받기 위해 쓰는 개념
    - 부모는 자식에서 정보를 전달하기 위해 props 옵션을 사용하여 데이터를 전달한다. 다만 하위에서 위로 접근할 수는 없다.
    - 이런 데이터 흐름을 단방향 데이터 흐름이라고 부른다. 대부분 위계가 있으므로 상위에서 하위로만 데이터를 흐르게 하기 위해 사용한다.
    - 반대로 자식에서 일어난 일을 부모 컴포넌트가 알게 하기 위해서 쓰는 것이 emit events다.
    - 사용시에 event를 걸고 거기에 this.$emit('eventname', 'data')이런식으로 적어주면 이벤트가 발생할 경우 상위 컴포넌트에 알려주게 된다.
    - 이 때 동봉할 데이터를 페이로드에 실어서 보내주면 상위 컴포넌트가 받아서 이에 대응할 수가 있다.
    - 이 때 듣기 위해서 상위 컴포넌트에서는 하위 컴퍼넌트 태그에 v-on을 이용하여 이 이벤트가 일어났을 때의 대처할 매서드를 만들 수 있다.
    - 이렇게 만드는 매서드의 첫번째 인자는 페이로드로 실려온 데이터이다. 
    - event는 자동 대소문자 변환이 없다.
- vue 구조
  - 생성시  vue하고 tab하면 아래 3단계를 자동으로 완성해준다.
  - template
    - 가져온 콤포넌트들을 사용할 수 있다.
  - script
    - 하위 컴포넌트를 불러오는 기능(import)
    - export default
      - name : 대체로 vue파일 이름과 일치시키는 편이다. vue 확장프로그램으로 봤을 때 각 컴포넌트의 대푯값을 의미한다고 볼 수 있다.
      - components : 불러온 하위 컴포넌트를 등록하는 곳이다.
      - props: 상위 컴포넌트에 렌더링되었을 때의 값을 받아서 반환할 것들을 등록하는 곳, property의 약자. 작성하는 방법으로는 오브젝트로 들어올 이름과 데이터 타입을 모두 정해줄 수도 있고, 단순히 어레이로 받아올 갯수와 순서만 정해줄 수도 있다. 이렇게 정해놨을 때 잘못들어오게 되면 에러가 날 수 있다. 그 이상으로 데이터타입 뿐만 아니라 될 수 있는 한 많은 데이터를 넣어주는 것 또한 권장한다. 
      - data: cdn으로 쓸 때와는 다르게 함수로 작성하고 그 결과로 반환되는 값을 쓰게 된다. 왜냐하면 이렇게 쓸 경우 데이터라는 오브젝트가 전역이 되어버리기 때문이다. 컴포넌트 여러개로 하나의 페이지를 구성하려고 하고 있으므로 이렇게 쓰는 것은 바람직하지 못하다. 고로 데이터를 각 컴포넌트마다의 스코프안에 가두기 위해서 함수를 쓰게 된다.
  - style
    - scoped : 이 옵션을 주면 이 컴포넌트에서만 적용된다.
