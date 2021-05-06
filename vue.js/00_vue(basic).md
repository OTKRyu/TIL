# vue

확장 프로그램 vetur 사용

## intro

프론트엔드 프레임워크(html, css and javascript를 활용한 화면 만들기)

spa(single page aplication)을 완벽하게 지원

evan yu에 의해서 제작 및 발표

## spa

현재 페이지를 동적으로 작성함으로서 사용자와 소통하는 웹 애플리케이션

단일 페이지로 구성되며 서버로부터 처음에만 페이지를 받아오고 이후에는 동적으로 dom을 구성

- 연속적으로 이어지므로 사용자 경험을 향상

동작 원리의 일부가 csr(client side rendering)을 따름

- 장점 : 트래픽 감소, 사용자 경험 향상
- 단점 : 검색엔진 최적화 문제(seo) 발생
  - 해결 방법도 따로 있다.

반대 개념으로 ssr(server side rendering)이 있다.

spa 등장 배경

- 과거에는 매번 새로운 페이지를 응답했음(multi page form)
- 스마트폰이 나오면서 이와같은 방식이 사용자 경험을 해침

### why vue.js?

- 일단 인기가 있는 프론트엔드 프레임워크다.

- 바닐라 js만으로 관리하기가 힘들다
  - 한 정보로 여러 개의 태그를 구성하는데 바닐라 js만으로 이를 처리하려면 일일히 태그를 전부 잡아와서 바꿔야하기 때문이다.
  - vue.js는 선언형으로 짜여있기 떄문에, 위의 문제를 편하게 해결할 수 있다.
  - 즉 data와 dom이 연결되게 짜여있어서 data만 바꾸면 알아서 dom을 바꿔준다. 신경써야하는것은 data뿐이게 된다.

## concept of vue.js

- mvvm pattern
  - 구성 요소
    1. model(data)
    2. view(template or screen)
    3. view model(intermediater of view and model)
- 작동 방식
  - data가 변하면 dom도 같이 변화한다.
    1. data 로직 작성
    2. dom 작성

## 사용법

1. cdn 혹은 실제 본문을 사용하고자하는 script에 넣는다.
2. 필요한 obj(특정 속성들을 정의하는 곳)를 생성, 이 때 신중해야하는게 속성을 추가해줄 수 있긴 하지만 초깃값이 아니면 vue가 반응을 하지 않는다. 고로 반응해야할 모든 요소를 초기에 고민해서 넣어놔야한다.
3. obj Vue에 입력해 view model을 생성한다. 다른 말로는 Vue instance를 만든다. 이를 태그에 마운트 되어있다라고 표현하기도 한다.
   - view model's attribute : Vue instance가 기본적으로 가지고 있는 속성들
     - `el` : vue 인스턴스에 연결할 dom element
       - 셀렉터로 잡으면 됨
     - `data` : vue instance의 데이터
       - 주의
         - 화살표 함수 쓰면 안 됨.
         - this도 써서는 안 된다. 
     - `methods` : vue instance에 추가할 메서드( 동작 및 데이터 세팅)
       - js 함수 쓰듯이 추가하면 된다.
       - 화살표 함수 쓰면 안 된다. this객체가 나오기 때문
       - 개발자 도구의 vue탭에 표시되지 않고, 접근할 때는 vm.methodname식으로 접근하면 된다. 그리고 여기서 쓰는 this는 실행될 때는 vue instance를 의미한다. vue에서 vue instance를 생성할 때 obj를 분해해서 이런식으로 달기 때문에 이런 일이 발생한다.
     - `computed` : vue instance에 추가할 연산된 값((computed)data getting)
       - method를 추가하면 그 메서드의 이름으로 메서드의 반환 값이 저장된다.
       - 즉 실제로는 함수가 작동하여 값을 가져오지만, 이를 그냥 반환값을 객체 취급하여 쓰고 싶은 곳에서 쓴다. 파이썬의 @property 데코레이터를 단 것처럼 쓰는것이다. 그리고 vue rendering을 할 때 data의 값처럼 쓸 수 있다.
       - 사실 methods에 적고 실행시켜서 값을 가져와서 쓰는 것도 가능하다. 하지만 computed의 데이터는 캐싱되어 있다가 종속대상이 변할 때만 실행되어 변화를 준다. 
     - `watch` : vue 인스턴스의 데이터 변경을 관찰하고 이에 반응하는 속성
4. 이후 view에서 directives를 사용해 html을 꾸민다.
   - directives :  `v-`로 시작하는 속성을 줌으로서 Vue가 이 속성을 보고 채워넣게 만드는 것들을 말한다. 이 후에 str으로 js의 표현식을 적어준다. 
     - `v-text` : 태그의 내용에 해당하는 것(innerText)
     - `v-html` : 태그 안에 html 요소가 들어간 것(innerHTML), 여전히 xss 공격에 취약하므로 임의의 사용자에게 받은 내용은 절 대 사용 금지, 에러나서 vue의 작동이 멈춰버린다.
     - `v-show` : 단순 엘레멘트에 display속성을 토글, false로 주면 display='none'을 줌
     - `v-if, v-else, v-else-if` : 조건부 렌더링을 걸 수 있는 속성이다. 조건에 해당되는 얘들만 넣어주고 아니면 안 넣는다. show와 다르게 렌더링 과정에서 html상에 없게된다.
     - `v-for` : 원본 데이터를 기반으로 v-for를 속성으로 지닌 태그를 여러번 렌더링. ""안에 js가 아닌 문법이 들어갈 수 있다. 파이썬처럼 쓸 수 있다. 
     - `v-on` : `v-on:click="changMessage"`처럼 액션을 붙여주고 그 때 실행될 함수명을 써주면 된다. 이를 줄여서 v-on대신 @를 써도 작동한다.
     - `v-bind` : 속성값과 연결, 즉 data의 값을 속성의 값과 연결시켜주는 역할이다.`v-bind:src="imgUrl"`처럼 원 속성 앞에 v-bind를 붙여주면 된다. `:`만 써주면 shortcut이다.
     - `v-model` : data를 연결해 직접 동기화할 때 쓴다. 인풋태그에  `v-model="dataname"`이라고 써주면 dataname을 인풋값을 dataname과 동기화시켜서 dataname을 쓰는 모든 다른 태그에 값을 변경시킨다. 인풋태그와만 엮어서 쓰며, 중간과정에 개입할 수가 없으므로 동기화에만 쓴다.
   - 혹은 보간법(interpolation)을 이용하여 넣을 수도 있다. ex) `{{ message }}`

