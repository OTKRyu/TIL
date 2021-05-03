# javascript

## intro

배우는 이유

- 브라우저 화면을 동적으로 만들기 위해
- 브라우저를 조작할 수 있는 유일한 언어

브라우저(browser)

- 웹 서버에서 이동하며 양방향 통신을 하는 gui 기반의 소프트웨어
- 인터넷의컨텐츠를 검색 및 열람하게 도와줌

## 역사

1. 핵심 인물
   - 팀 버너스리
     - www,url,http,html 최초 설계자
     - 웹의 아버지
   - 브랜던 아이크
     - javascript 최초 설계자
     - 모질라 재단 공동 설립자
     - 파이어폭스의 전신인 코드네임 피닉스 프로젝트 진행

2. js의 탄생
   - 94년대에는 netscape navigator가 점유율이 가장 높았으며 표준 역할을 함
   - 넷스케이프에 재직중이던 브랜든 아이크가 js를 개발
   - mocha - livescript - javascript
   - ms가 js를 채택해 커스텀후 Jscript라는 이름으로 ie에 탑재
3. 1차 브라우저 전쟁
   - 넷스케이프 vs 마이크로소프트
   - ms가 ie 4 가 시장을 장악
     - 윈도우의 시장 독점
   - ms의 승리
   - 브랜던 아이크가 넷스케이프에서 나와 모질라 재단 설립
     - 파이어 폭스 개발해 ie에 대항
4. 2차 브라우저 전쟁
   - ms vs google
   - google의 크롬 발표
   - 3년만에 파이어폭스 점유율돌파, 그 1년 후 1위 탈활
   - 승리 요인
     - 웹 표준 잘 지킴
     - 압도적인 속도
     - 강력한 개발자 도구 제공
5. 파편화 및 표준화
   - 이런 경쟁의 결과 자바스크립트가 여러 개 등장하게 됨
   - 브라우저마다 짜는게 다 달라지게 돼서 크로스 브라우징 이슈가 발생, 그래서 웹 표준의 필요성이 제기
   - 크로스 브라우징
     - w3c에서 채택한 표준 웹 기술을 채택해 다른 브라우저에서도 비슷하게 작동할 수 있게 하는 것
   - 넷스케이프는 표준 제정에 대해 꾸준히 요청함
     - ecma 인터내셔널에 표준 제정 요청
   - ecmascript 1 탄생
   - 크롬의 등장 이후 파편화를 해결하기 위해 표준화에 적극적으로 동참
6. 현재의 js
   - es2015(es6) 탄생
     - next-gen of JS
     - js의 고질적인 문제들을 해결
     - js의 다음 시대라고 불릴만큼 많은 혁식과 변화가 일어남
     - 이후로 공식 명칭은 버전이 아닌 출시년도를 기준으로 부름
     - 현재는 대부분 es6+가 표준
   - vanilla javascript
     - 크로스 브라우징, 간편한 활용 등을 위해 많은 라이브러리가 등장 ex) jQuery
     - 표준화가 된 이후로는 다양한 도구의 등장으로 라이브러리보다 순수 자바스크립트 활용의 증대
7. 결론
   1. 브라우저 전쟁으로 인한 파편화 및 표준화 과정
   2. 결과적으로 vanilla javascript로 오게 됐다.

## 문법

- 변수 선언 키워드(let, const)
  - 할당과 포인터는 다르다는 점을 항상 주의해서 사용하자
  - let
    - 재할당 할 수 있는 변수 선언시 사용
    - 변수 재선언 불가능
    - 블록 스코프{}
  - const
    - 재할당 할 수 없는 변수
    - 변수 재선언 불가능
    - 블록 스코프{}
  - var
    - legacy코드로 es6 이전에는 다 이걸로 선언했다.
    - 재선언 및 재할당이 모두 가능
    - 호이스팅되는 특성으로 인해 예기치 못한 문제 발생
      - 호이스팅이란 선언 이전에 참조해버릴 수 있는 현상으로 선언 이전에 접근 시 undefined 반환
      - 따라서 const와 let을 쓰기를 권장
    - 함수 스코프
  
- 제어문

  - 반복문

    - while : c의 while문과 유사하다.

    - for : 문법 자체는 c에서의 문법과 같다. 다만 주의해야할 점은 아래의 객체 중 실시간으로 반영이 되는 객체가 있기 때문에 이를 고려하고 돌리지 않으면 원하는 결과를 얻지 못할 수 있다. js에서만 되는 것이 있는데

      ```javascript
      for (const number of numbers) {
          console.log(number)
      }
      ```

    - 이 코드인데 왜 const가 되냐면 포문은 블록레벨이 여러번 반복되는 것이기 때문이다. 즉 다른 블록에서 매번 같은 이름의 변수를 할당하는 것이다. python의 in과 비슷하게 작동한다. 다만 이 때는 number에 담기는 것이 index이기 때문에 array와 같은 구조에서만 쓴다

    - Object처럼 key를 받아와야하는 경우에는 of 대신 in을 쓴다.(Object의 구조는 파이썬의 dict와 비슷하다.)

  - 조건문

    - if, else, else if : c와 유사하다.
    - switch, case : c와 유사하다.

- 함수 
  - 자바스크립트의 함수는 에러를 잘 내지 않는다. 인자가 적어도 인자가 많아도 일단 실행은 되는 편이다. 이 때 많을 경우에는 뒤에 들어온 인자를 무시한다.
  
  - `function funcname(arg) { return value}` : 기본적으로 함수를 생성하는 법, 선언식
  
  - `const funcname = function(arg) { return value}` : 함수를 생성해 함수이름에 할당한 것, 표현식 
  
  - 위와 같은 방법으로 할 시에 로직과 그를 담는 그릇을 따로 두는 것이다. 고로 로직만 필요할 시에 로직만 만들어 작성하고 할당없이 바로 쓰는 것도 가능하다.
  
  - 다만 이렇게 할경우 함수를 담을 그릇을 const라는 제한을 가진 그릇으로 만드는 것이라서 선언식으로 짠 것과는 조금 차이가 있다. var를 썼을 경우 호이스팅이 되기 때문에 아래에서 정의해놔도 위에서 쓸 수 있지만, const로 쓸 경우 이런 것은 불가능하다. 하지만 원래도 함수는 위에서 먼저 적어두는 것이 관습이기 때문에 그냥 관습을 지키면 신경쓸 필요 없는 문제다.
  
  - 이 표현식조차 줄일 수 있는데 그게 arrow function이라고 불리는 것으로 코드는 아래와 같다. 다만 이렇게 쓸 경우 위의 표현들과는 좀 차이가 생긴다. 이 차이가 생기는 경우가 내부에 this라는 예약어가 있는 경우다.
  
    ```javascript
    const mul = (n1, n2) => n1 * n2
    ```
  
  - 만드는 방법은 표현식에서 function을 지우고, () 와 {} 사이에 =>를 넣는 것이다. 추가적으로 인자가 1개라면 ()를 생략할 수 있고, 블록 안에 return문만 있다면 {}와 return도 생략 가능하다.
  
  - 종류
    - `.trim()` : 문자열의 좌우 여백을 없앤 문자열을 반환하는 함수
    - `Boolean()` : 들어온 객체가 참인지 거짓인지 알려주는 함수, 이 때 객체의 참 거짓 분별이 다른 언어와 다를 수 있으니 주의해가며 사용해야한다.
    - `.length` : 객체의 속성값으로 잡혀있으며, 길이를 알려준다.
    - `localStorage.setItem('data', data_to_save)` : 사용자의 저장소에 자바스크립트에서 쓴 내용을 'data'라는 키값으로저장하는 함수이다
    - `localStorage.getItem('data')` : 사용자의 저장소에서 'data'라는 키값으로 저장된 데이터를 불러오는 일이다.  
    - `JSON.stringify(obj)` : object를 JSON형태를 갖춘 string으로 만들어준다.
    - `JSON.parse(str)` : JSON형태를 갖춘 str이 들어오면 obj로 만들어 준다.
    - `new` : 인스턴스를 만들 때 쓰는 명령어라고 생각하면 된다.

## 객체 타입

기본 타입

1. number
   - 일반적으로 숫자라 생각되는 모든 것
   - Infinity라는 예약어로 무한대를 표현하게 되어있다.
   - NaN은 Not a Number라는 뜻으로 숫자연산을 다른 객체로 하려했을 시 나오는 number다.
2. string
   - 일반적인 문자열과 같다.
   - 다른 연산자는 안 되지만 + 연산자만은 concatonation용으로 허용한다. 한쪽이 문자열일 경우에는 다른 한쪽이 숫자여도 숫자도 문자열로 바꿔서 붙여버린다.
   - string ${another string} 이런 형식으로 ` 안에 넣으면 파이썬의 f string처럼 작동한다.   
3. empty
   - null 과 undefined 정도가 있다.
   - null은 의도적으로 비워놓은 것을 의미하고, undefined는 실제로 어떤 값이 들어있는지 알 수가 없을 때 나온다.
4. boolean
   - true, false가 기본적이며, 0과 ''정도만 false으로 인식하고 나머지는 다 true로 인식한다.

추가 타입

 1. Object

    - 파이썬의 dict와 비슷한 구조이다.

    - key값에 어차피 string이 들어가기 때문에 ''을 생략해도 잘 들어간다. 다만 띄어쓰기가 있을 경우는 ''을 생략해서는 안 된다. 다만 key도 일종의 변수명이기 때문에 변수명처럼 짓기를 추천한다.

    - 내부 요소에 접근할 때 []뿐만 아니라 .을 통해서도 접근가능하다.

    - 다른 hashmap 구조처럼 Object 구조안에 value로 array이나 Object를 넣을 수 있다.

    - es6+ 축약 문법

      - 

        ```javascript
        // old
        // object 내부 구성
        var books = ['learningjs', 'eloquentjs']
        var megazines = ['gq','esquire']
        var bookshop = {
            books : books
            megazines : books
        }
        // object 내부 함수 정의
        var dooly = {
            name: 'dooly'
        	greeting: function () {
             console.log('어서 오고')   
            }
        }
        
        // object destructuring(비구조화)
        var userInfo = {
            name : '김싸피',
            email: 'kim@ssafy.com',
            phone: '0101234567',
        }
        var name = userInfo.name
        var email = userInfo.email
        var phone = userInfo.phone
        ```

      - 

        ```javascript
        //new
        
        const bookshop = {
            books,
            megazines,
        }
        
        const dooly = {
        	name: 'dooly',
            // arrow
            greeting1: () => console.log('도우너,'),
            // function 키워드 대체용
            greeting2 () {
                console.log('어서오고')
            }
        }
        // computed property name
        const key = 'resgions'
        const value = array
        const saffy = {
            [key] : value,
        }
        saffy.regions
        
        // object destructuring(비구조화)
        const userInfo = {
            name : '김싸피',
            email: 'kim@ssafy.com',
            phone: '0101234567',
        }
        const { name } =  userInfo 
        const { email } =  userInfo 
        const { phone } =  userInfo
        const {name, email, phone } = userInfo
        const function printInfo({name,email,phone}){
            console.log(`${name} ${email} ${phone}`)
        }
        printInfo(userInfo)
        
        
        ```

	2. array

    - 음수 인덱싱 불가능

    - 슬라이싱 불가능

    - 사실 array아님 동적 연결 리스트임

    - 속성 및 매서드

      - `.length` : 길이
      - `.reverse()` : 원본을 뒤집는다. 뒤집힌 원본도 밥환한다.
      - `.push(ele)` : 원소를 추가한다. 원소가 들어가서 변한 원본의 length를 반환한다.
      - `.pop()` : 마지막 원소를 제거하고 그 원소를 반환한다.
      - `.unshift(ele)` : 앞에 원소를 추가하며 변한 원본의 length를 반환
      - `.shift()` : 처음 원소를 제거하고 그 원소를 반환
      - `.includes(ele)` : 원본에 ele이 있는 지에 대해서 boolean값 반환
      - `.indexOf(ele)` :  원본에 ele이 있는지 찾아서 있으면 그 index를 없으면 -1 반환
      - `.join('str')` : 원본을 문자열로 취급하면서, str를 사이에 끼운 하나의 문자열로 만들어 반환
      - `.forEach(func)` : 원본의 원소 하나하나를 func에 넣었을 때의 결과로 바꿔줌

    - arreyhelpermethods

    - 종류

      - `.forEach()` : 리턴 되는 값이 없고, 콜백 함수에 return도 있을 필요가 없다. for of 문과 용도가 비슷하다. array를 돌면서 함수에 해당하는 일을 실행한다.
      - `.map()` : 콜백함수로 만든 array를 반환한다.
      - `.filter()` : 콜백함수의 리턴값이 true인 요소만 모아서 배열로 반환
      - `.find()` : filter와 비슷한 기능이지만 콜백함수의 리턴값이 true인 첫번째 요소만 반환한다.

      - `.reduce()` :  유일하게 인자가 두개 필요한 매서드이다. cb, init이라고 부르는데 cb은 콜백함수를 init은 초기 값을 말한다. 추가적으로 콜백함수도 acc, ele 두개의 인자가 필요하다. 초회에는 init을 acc로 콜백함수에 넣고 그 후로는 인자마다 콜백함수를 돌리면서 반환된 값들을 acc자리에 넣어 계속 돌리다, 인자가 떨어지면 마지막 반환된 acc를 반환한다.

    - how to use

      - when .forEach, .map, .filter
        - arr.helpermethod(callbackfunction)
        - arr.helperMethod(function (arg) {return}) : arg is element of array
      - .reduce()
        - arr.reduce(callbackfunction(acc, arg), initValue)

## 연산자

- 항의 갯수로 구분
  1. unary(단항 연산자)
     - -1, +1과 같은 항 하나에 작용해서 값을 내보내는 연산자를 의미한다.
     - typeof 도 뒤에 항 하나를 받아와 값을 내보내는 연산자이다. 다만 다른 연산자와 달리 꼭 띄워서 써야된다.
     - ++,--, !
  2. binary(이항 연산자)
  	- -, +, / , * , +=, -=, /=,>,>=,<=,<
  	- &&,|| 단축평가도 똑같이 일어난다.
  3. ternary(삼항 연산자)
  	- (boolean)? (true case) : (false case) 
  
- 기능에 따라 구분

  1. 산술연산자
  2. 수 비교연산자
  3. 동등/일치 연산자
     - 동등 ==
     - 일치 ===
     - 인데 ==을 안 쓰고 ===만 쓴다. ==가 하자가 좀 있는 연산자이기 때문
  4. 논리연산자



