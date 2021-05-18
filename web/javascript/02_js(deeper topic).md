# javascript

## threads

- 프로그램이 작업을 완료하는데 사용할 수 있는 단일 프로세스
- 각 스레드는 한번에 하나의 작업만 수행할 수 있음
- 다음 작업이 시작되려면 반드시 앞의 작업이 완료되어야 함
- 컴퓨터 cpu는 여러 코어를 가지고 있다보니 한번에 여러가지 일을 처리 가능
- js는 기본적으로 single thread이다. 
  - 여러개의 요청을 해놔도 하나로만 진행한다는 것이다.
  - 즉 이벤트를 처리하는 call stack이 하나인 언어라는 의미
  - 이 문제를 해결하기 위해 js는 즉시 처리하지 못하는 이벤트는 web api로 보내서 처리하도록 하고, 처리된 이벤트들은 처리된 순서대로 task queue에 줄을 세워 놓고, call stack이 비면 대기 줄의 제일 오래 된 이벤트를 call stack으로 보냄

### concurrency model

- event loop를 기반으로 하는 동시성 모델

  1. call stack
  2. web api
     - js가 아닌 브라우저가 제공하는 api
     - setTimeout(), DOM events 그리고 ajax로 데이터를 가져오는 시간이 소요되는 일들을 처리
     - setTimeout같은 경우 지정해주는 시간은 web api에서 대기하는 시간을 의미한다. 고로 딱 대기시간 후에 실행될거라고는 보장할 수 없다, 즉 최소 지정된 시간만큼 후에 실행이 될 수 있다는 의미이다. 
     - 따라서 순서를 보장해주는 것이 아닌 그냥 최소시간이 지난후 끝난 작업부터 넣어주는 것에 불과하다는 것이다.
  3. task queue
     - 콜백 함수들이 대기하는 큐
     - main thread가 비면 그제서야 실행, 후속 javascript 코드가 차단되는 것을 방지
  4. event loop
     - call stack이 비어있는 지 여부를 확인
     - 비어 있는 경우 task queue에서 실행 대기중인 콜백이 있는지 확인
     - task queue에 대기중인 콜백이 있다면 가장 앞에 있는 콜백을 call stack으로 push

- 순차적인 비동기 처리

  위의 문제(실행 순서 불명확) 때문에 이를 처리하기 위한 2가지 작성 방식

  1. async callbacks
     - 백그라운드에서 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
     - 이렇게 하면 무조건 실행할 함수가 시작되고 나서야 콜백함수가 작동할 수 있기 때문이다.
     - callback function이란 것은 단순히 명시적인 호출이 아닌 다른 routine이나 action에 의해 호출되는 함수를 말하고, 이것이 가능한 언어려면 함수를 이런 식으로 주고 받을 수 있어야 한다. 이와 관련된 개념이 1급 객체라는 것으로 인자로 넣을 수 있고, 함수의 반환값으로 반환할 수 있고, 변수에 할당할 수 있는 객체를 말한다. javascript에서 함수는 1급 객체이다. 고로 비동기 로직을 위해서는 콜백 함수는 필수적이다.
     - 다만  순차적인 연쇄 비동기 작업을 처리하기 위해 여러 개의 연쇄 비동기 작업을 마주했을 때, 디버깅도 어려워지고 코드 가독성도 떨어지기 때문에 , 이를 callback hell이라고 한다. 이를 위해 아래와 같은 방법을 고민한다.
       1. 코드를 얕게 유지
       2. 모듈화
       3. 모든 단일 오류 처리
       4. promise형식
     - async callbacks은 백그라운드에서 코드 실행을 시작할 함수를 호출할 때 인자로 지정된 함수를 말하며, 백그라운드 코드 실행이 끝나면 콜백 함수를 불러와 작업이 완료되었음을 알려주거나 하는 함수.
  2. promise-style
     - modern web apis에서의 새로운 코드 스타일
     - xmlhttprequest보다 조금 더 현대적인 버전
     - 보장해 주는 것
       1. event loop에 배치된 순차적으로 진행되기 때문에 순서를 지킴
       2. 여러 번 사용하여 여러 개의 콜백을 추가할 수 있음(chaining)
          - 연쇄를 할 수 있다는 점이 이런 방식에 가장 좋은 점이다.
     - promise object
       - 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
         - 미래의 완료 또는 실패와 결과 값을 나타냄
         - 미래의 어떤 상황에 대한 약속
       - 성공에 대한 약속 : `.then(callback)`
         - 성공했을 때 수행할 작업을 나타내는 콜백 함수
         - 성공 결과를 인자로 전달 받음
       - 실패에 대한 약속: `.catch(callback)`
         - 실패했을 때 수행할 작업을 나타내는 콜백 함수
         - 이전 작업의 실패로 인해 생성된 error객체는 catch 블록 안에서 사용할 수 있음
       - 결과와 상관없이 무조건 실행 : `.finally(callback)`
         - promise 객체를 반환
         - 결과와 상관없기 때문에 어떤한 인자도 전달 받지 않음
           - 무조건 실행되기 때문에 어차피 성공했는지 틀렸는지 알수가 없기 때문
         - 무조건 실행되는 절에서 사용
       - 상태
         1. 대기(pending)
         2. 이행(fulfilled)
         3. 거부(rejected)
     - 작성 방법
       - 초기에 promise(resolve,reject)객체 생성
       - 성공시 resolve, 거부시 reject()
       - 각각의`.then()` 블록은 서로 다른 promise를 반환
         - 즉 .then()을 여러 개 사용하여 연쇄적인 작업을 수행할 수 있음, 다만 이럴 필요가 있는 것은 순차적으로 이루어져야만 하는 작업은 대체로 요청을 보내거나 시간을 표시할 필요가 있는 일들 뿐이다.
       - 마찬가지로 .then과 .catch 모두 promise를 반환하기 때문에 연쇄 가능
       - 실패시 error를 넘겨줘 왜 틀렸는지에 대한 작업도 가능
         - 이 때 error.response에 왜 에러가 났는지에 대한 데이터를 대부분의 서버가 함께 담아주므로 이를 표기해주면 좋다.
       - 주의
         - 반환 값이 반드시 있어야 함
         - 없다면 콜백 함수가 이전의 promise 결과를 받을 수 없음

## AJAX

asynchronous javcript and xml(비동기식 js와 xml)

- 서버와 통신하기 위해 xmlhttprequest 객체 활용
  - xmlhttpsrequest는 동기식과 비동기식 통신을 모두 지원
- 페이지 전체를 reload를 하지 않고서도 수행되는 비동기성
  - 사용자의 event가 있으면 전체 페이지가 아닌 일부분만을 업데이트
- ajax의 x가 xml이지만 요즘은 json을 더 많이 사용

### 배경

- 구글 맵과 지메일 등에 활용되는 기술을 설명하기 위해 최초로 사용
- 기존의 여러 기술을 사용하는 새로운 접근법을 의미
- 예시
  - gmail : 전송을 눌러놓고 다른 페이지로 넘어가 다른 일 가능
  - 구글 맵 : 스크롤하는 행위 하나하나가 모두 요청이지만 페이지는 갱신되지 않음

### xmlhttprequest object

- 서버와 상호작용하기 위해 사용되며, 전체 새로고침없이 url로부터 데이터 받아올 수 있음

- 사용자를 방해 안 하면서 페이지의 일부를 업데이트할 수 있음

- xml말고도 다른 데이터를 받아오는 것도 가능

- 생성자
  - `XMLHTTpRequest()`
  
  ```javascript
  const xhr = new XMLHttpRequest()
  // 준비단계
  xhr.open('GET', 'http://google.com')
  // 실제 요청
  xhr.send()
  xhr.onload = function () {
      if (xhr.status === 200) {
          const todo = xhr.response
          console.log(todo)
      }
  }
  ```

### asychronous js

- 동기식

  - 순차적, 직렬적 태스크 수행
  - 요청을 하고 응답을 받아야 다음 동작이 이루어짐(blocking)

- 비동기식

  - 병렬적 태스크 수행

  - 요청을 보낸 후 응답을 기다리지 않고 다음 동작이 이루어짐(non-blocking)

    - 즉, 요청 보내놓고 다음 할 일을 함

  - 왜 사용하는가? 사용자 경험


### axios

- promise based HTTP client for the browser and node.js

- 브라우저를 위한 promise 기반의 클라이언트

- 원래는 xhr이라는 내장 객체를 썼는데 이보다 편리하게 ajax요청이 가능하도록 도와줌

  - 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공
  - cdn으로 넣자마자 axios 변수까지 만들어 놓음
  - axios는 추가적으로 많은 일을 해주는데 그중에 하나가 parsing이다.
  - 아래 코드처럼 날릴 수도 있지만, 통신이 그렇듯 오브젝트형태({})로 날릴 수도 있다.
  - 추가적으로 axios의 인자순서는 URL, payLoad(body), config(header)순이다. 고로 토큰같은 것을 보낼 때는 config오브젝트안에 headers란 항목을 추가해서 그 안에 적어주면 된다.
  
  ```javascript
  const URL = 'url'
  
  axios.get(URL)
  	.then(function (response) {
      console.log(response.data)
  })
  	.catch(function (err) {
      
  })
  	.finally(() => {
      console.log('언제든 실행됨')
  })
  ```

### async&await

- axios도 .then의 연쇄로 인해 코드가 아래로 길어지는 문제까지 해결하지는 못했음.

- 이를 해결하기 위해 나온 기능으로 es2017에 나온 syntatic sugar다 

  