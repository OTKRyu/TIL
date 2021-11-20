# javascript

## 기능

- dom 조작(document object model)
  - html 조작
- bom 조작(browser object model)
  - screen, location, xhr등등
- javascript core(프로그래밍 언어로서의 기능)
  - 데이터 구조, 배열, 반복 등등


### dom

- html, xml 등과 같은 문서를 다루기 위한언어 독립적인 문서 모델 인터페이스
- 문서를 구조화하고 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델
  - 즉 html에서 구조화를 했던 것처럼 하나하나의 태그를 객체로 취급하여 위계를 가진 구조로 취급한다는 것이다.
- 문서가 구조화되어 있고, 각 요소를 객체로 취급
- 속성 접근, 매서드 활용 이상의 프로그래밍 언어기능까지 사용가능
- 주요 객체
  - window: dom을 표현하는 창, 가장 최상위 객체, 브라우저 기준 한 탭
  - document: 페이지 콘텐츠의 entry point역할을 하며 body 등과 같은 수많은 다른 요소들을 포함
  - navigator, 등등
- 해석
  - parsing
    - 구문 분석, 해석
    - 브라우저가 문자열을 해석하여 dom tree로 만드는 과정
- dom 객체
  - htmlcollection and nodelist
    - 둘 다 배열과 같이 각 항목에 접근하기 위한 인덱스를 제공
    - htmlcollection
      - name, id, 인덱스 속성으로 각 항목들에 접근 가능
    - nodelist
      - 인덱스 번호로만 각 항목들에 접근 가능
      - 다만 배열에서 사용하는 함수 및 다양한 메서드 사용가능
    - 둘 다 live collection으로 dom의 변경사항을 실시간으로 반영하지만, querySelectorAll()에 의해 반환되는 NodeList는 static collection
  - live and static collection
    - live
      - 문서가 바뀔 때 실시간으로 업데이트
      - dom의 변경사항을 실시간으로 collection에 반영
    - static
      - dom이 변경되어도 collection 내용에는 영향을 주지 않음
      - querySelectorAll()의 반환 NodeList만 static

#### dom 조작

- 문서 한장에 해당하는 것을 조작
- dom 조작 순서
  1. 선택(select)
  2. 변경(manipulation)
- dom 관련 객체의 상속 구조
  - eventtarget
    - Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터페이스
  - node
  - element and document
    - element
      - document안에 각 태그같은 요소들을 말함
    - document
      - entry point 역할
      - 웹 페이지 그 자체를 나타냄
  - htmlelement
- dom 선택 매서드(js는 데이터타입을 지정을 해줘야한다. const, let)
  - 크롬 개발자 도구에서 객체 우클릭 > copy > copy selector를 하면 그 객체를 부를 수 있는 셀렉터를 뽑아낼 수 있다.
  - `document.querySelector()`
    - 제공한 선택자와 일치하는 element 하나 선택
    - 제공한 CSS selector를 만족하는 첫번째 element 객체를 반환 (없다면 null)
  - `document.querySelectorAll()`
    - 제공한 선택자와 일치하는 여러 element를 선택
    - 매칭할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 문자로 받음
    - 지정된 셀렉터에 일치하는 nodelist를 반환
  - `document.getElementById()`
  - `document.getElementBytagName()`
  - `document.getElementsByClassName()`
  - 위의 셋과 같은 선택지가 있지만 queryselector가 더 유연하게 사용가능하므로 위의 셋은 잘 쓰지 않는다.
- dom 추가 삭제 메서드
  - `document.createElement('li')`
    - 주어진 태그명을 이용해 html요소를 만들어 반환
  - `document.write('<h1>hello></h1>')`
    - html 문법에 맞춘 str이 들어오면 그걸 그대로 html에 적어준다.
  - `parentNode.append()`
    - 특정 부모 노드의 자식 노드 리스트 중 마지막 자식 다음에 node객체나 domstring을 삽입(반환 값 없음)
    - 여러 개의 node 객체, domstring을 추가 할 수 있음
  - `node.appendChild()`
    - 한 노드를 특정 부모 노드의 자식 노드 리스트 중 마지막 자식으로 삽입(node만 추가 가능)
    - 만약 주어진 노드가 이미 문서에 존재하는 다른 노드를 참조한다면 새로운 위치로 이동
  - `ChildNode.remove()`
    - 이를 포함하는 트리로부터 특정 객체를 제거
    - 다만 할당해놓은 객체는 저장되어 있다.
  - `node.removeChild()`
    - dom에서 자식 노드를 제거하고 제거 된 노드를 반환
    - node는 인자로 들어가는 자식 노드의 부모 노드
- dom 변경
  - 내용
    - `node.innerText`
      - 노드와 그 자손의 텍스트 콘텐츠를 표현(사람이 읽을 수 있는 요소만 남김)(domstring, raw string)
      - 즉 줄 바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용된 모습으로 표현
    - `element.innerHTML`
      - 요소 내에 포함된 HTML 마크업을 반환
      - xss 공격에 취약하므로 조심해서 써야한다.
  - 속성
    - `element.setAttribut(name,value)`
      - 지정된 요소의 값을 설정
      - 속성이 이미 존재하면 값을 업데이트, 없으면 새 속성 추가
    - `element.getAttribute()`
      - 해당 요소의 지정된 값(문자열)을 반환
      - 인자는 값을 얻고자 하는 속성의 이름
    - `element.classList.add('classname')`
      - 요소의 클래스에 추가로 클래스를 넣을 때 쓰는 명령어
    - `element.classList.toggle('classname')`
      - 클래스가 이미 적용되어 있다면 그 클래스를 해제
      - 클래스가 적용되어 있지 않다면 그 클래스를 추가
    - `element.classList.remove('classname')`
      - 해당 클래스를 제거

### event

- 네트워크 활동 혹은 사용자와의 상호작용 같은 사건의 발생을 알리기 위한 객체
- 이벤트는 마우스를 클릭하거나 키보드를 누르는 등 사용자 행동에 의해 발생할 수도 있고, 특정 메서드를 호출하여 프로그래밍적으로도 만들어낼 수 있음
- 이벤트 처리기(Event-handlers)
  - EventTarget.addEventListener()
  - 해당 메서드를 통해 다양한 요소에서 이벤트를 붙일 수 잇음
  - removeEventListener()를 통해 이벤트를 제거 가능
  - 지정한 이벤트가 대상에 전달될 때마다 호출할 함수를 설정
  - 이벤트를 지원하는 모든 객체를 대상으로 지정 가능
  - target.addEventListener(type, listener[, options]) : target에 type이 일어나면 listener를 하겠다.
    - type : 반응할 이벤트 유형( 대소문자 구분 문자열)
    - listener : 지정된 타입의 이벤트가 발생했을 때 알림을 받는 객체,  EventListerner 인터페이스 혹은 JS function 콜백 함수라고도 한다. 콜백함수는 말 그대로 생성하자마자 쓰인다기보다는 만들어놓고 나중에 필요한 상황이 오면 그제서야 불린다는 뜻인데, 대부분의 함수가 그렇기 때문에 그렇게 크게 중요하게 생각할 필요는 없다. 이 함수는 기본적으로 event라는 인자를 가지고 있어야한다.
      - event.preventDefault():
        - 현재 이벤트의 기본 동작을 중단
        - 태그의 기본 동작(a 태그는 링크로 이동, form 태그는 폼 데이터 전송)
        - 이벤트의 전파를 막지 않고 이벤트의 기본 동작만 중단, 즉 이벤트는 발생해서 진행되는데 이로 인해 html에서 태그가 기본적으로 하는 작동은 막는다.
        - event.target.reset()를 하면 target을 기본상태로 돌릴 수 있다.
        - 위에서 볼 수 있다시피 event.target은 이벤트가 일어나는 주체를 말한다.
        - 다만 모든 경우에 작동하는 것은 아니고 scroll과 같은 행동은 막을 수 없다. event.cancelable에서 값을 확인해보면 가능한지 아닌지 알수가 있다.
        - 이 모든게 가능한 이유는 이 명령어가 다른 태그들의 기본 명령어보다 우선 실행되는 상위 명령어이기 때문이다.
- event 기반 인터페이스
  - animationevent, clipboardevent, dragevent 등
  - uievent
    - 간단한 사용자 인터페이스 이벤트
    - event의 상속을 받음
    - mouseEvent, KeyboardEvent, InputEvent, FocusEvent 등의 부모 역할을 함
    - 종류
      - input : input태그에 문자가 쓰여지거나 지워질 때마다 실행, 이 때 그 태그에 적힌 데이터는 event.target.value에 적혀있다. event.data에도 event를 일어나게 만든 데이터를 저장해놓고는 있지만 누적이 안 되기 때문에 지금 쓰고 있는 문자에 대한 이벤트를 작성할때만 의미가 있다.
      - click : 선택한 요소를 클릭했을 때 실행, 특수하게 폼 안에 있는 버튼을 클릭하는 행위는 폼을 제출하는 행위와 연결이 되어 있어서, input에서 enter만 쳐도 똑같이 작동을 한다.
      - submit : 데이터를 전송하려고 할 때 발생
      - scroll : 스크롤로 움직이려고 할 때 발생 
      - keydown : 어떤 버튼이든 간에 누르면 발생
      - keyup : 어떤 버튼을 눌렀다 뗐을 때에 발생, .enter와 같이 특정키를지정가능
      - change : 포커스에서 벗어나면 발생

### bom

- browser object model
- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
- window 객체는 모든 브라우저로부터 지원을 받는다.

#### bom 조작

- `console.log()`
- 프린트에 해당하는 명령어
- `console.error()`
  - 프린트인데 사실상 배경 색깔만 바꿔주는 것에 불과하다.