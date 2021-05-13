# vue

## vuex

- statement management pattern, library for vue.js
- 상태를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리
  - 싱테기 예측 가능한 방식으로만 변경될 수 있도록 보장하는 규칙 설정
- vue의 공식 devtools와 통합되어 기타 고급 기능을 제공
- 중앙 저장소에서 state를 모아놓고 관리
- 규모가 큰 프로젝트에 매우 편리
- 이를 공유하는 다른 컴포넌트는 알아서 동기화
- 단방향 통신의 단점인 트리가 깊어지거나 동일한요소를 여러 곳에서 사용할 경우 불필요하게 코드가 길어지는 것을 해결
- 다만 이를 쓴다는 것이 기존의 단방향 통신을 쓰지 않는다는 것이 아닌, 중앙관리용 저장소를 따로 만들어 필요할 때 쓴다는 것에 가깝다. 두 가지 방식 모두 적절히 혼용하는 것이 필요하다.

### vuex의 활용

- 공유된 상태 관리를 처리하는데는 유용하지만, 앱이 단순하거나 하면 낭비일 수 있음

- 하지만 spa의 규모가 커진다면 vuex가 필요해질 수 있음

- 결론적으로 필요한 순간에 적절히 쓰는 것을 추천

### vuex의 용어

- state
  - 데이터 및 어플리케이션의 핵심이 되는 요소
  - 각 콤포넌트에서 관리
  - 이에 반응하여 dom을 렌더링

- view
  - 선언적 매핑
- action
  - 사용자 입력에 밥능해 상태를 바꾸는 방법

### vuex의 상태 관리 패턴

- 컴포넌트의 공유된 상태를 추출하고 이를 저녕ㄱ에서 관리 하도록 함
- 컴포넌트는 커다란 뷰가 되며 모든 컴포넌트는 트리에 상관없이 작동가능
- 코드의 구조와 유지 관리 기능 향상

### vuex의 구성 요소

vuex는 flux 디자인 패턴을 적용한 라이브러리로 flux 디자인 패턴이란 데이터를 각기 다른 컴포넌트와 모델간의 통신으로 이루지 않고, 아래와같은 단방향 순환과정을 통해 중앙에서 데이터를 관리하는 것을 말한다. 더 자세한 내용은 문서를 참조하기 바란다.

1. state

   - 중앙관리중인 데이터
   - mutation에 정의된 메서드에 의해 변경
   - state가 변화하면 해당 state를 공유하는 컴포넌트의 dom은 알아서 렌더링
   - vuex store에서 데이터를 가져와 사용
   - dispatch()를 사용하여 actions 내부의 메서드를 호출 

2. actions

   - 컴포넌트에서 dispatch() 메서드에 의해 호출
   - 백엔드 api와 통신하여 data fetching 작업 수행
   - 항상 context가 인자로 넘어옴
     - store.js 파일 내에 자유롭게 접근 및 호출 가능
     - 다만 state를 직접 변경하지는 않는다.
   - mutations에 정의된 메서드를 commit 메서드로 호출
   - state는 오로지 mutations 메서드를 통해서만 조작
     - 올바른 역할 분담을 위해

3. mutations

   - actions에서 commit()으로 호출
   - 비동기적으로 동작하면 state가 변화하는 시점이 달라질 수 있기 때문에 동기적인 코드만 작성
   - mutations에 정의하는 메서드의 첫 번째 인자로 state가 넘어옴
   - 무조건 동기적인 메서드만을 사용해야한다. vuex가 비동기적인 메서드가 끝나는 시점을 추적할 수 없기 때문에 적절히 반영할 수 없기 때문이다.

4. getters

   - state를 변경하진 않고 활용하여 계산을 수행(computed)

     - 실제 계산된 값은 저장소의 상태를 기준으로 계산
   - 처음부터 생성되어 있지는 않다.
   - state를 쓰기 때문에 첫번째 인자로 state가 들어온다.
   - 가져올 때는 `this.$store.getters.gettername`으로 가져와 사용하게 된다.



### vuex의 명령어

- component helper method
  - vuex를 쓰면 중앙과 지역이 따로 움직이기 때문에 이를 매번 연결하거나 하는 컴포넌트 간의 해야할 일이 생기게 되는데, 이를 도와주는 메서드들이다.
  - 기본적으로 내장이 되어 있지 않으므로 가져와야 한다. 위치는 'vuex'에 있다.
  - 공통적으로 사용은 매핑할 객체 안에 객체안에 `...componenthelpermethod([objectname,])` 라고 써주면 `objectnames:object`로 매핑이 된다.  
  - `...mapState()` : computed와 state를 매핑, 다만 이 함수는 computed에서 쓰는 편이다. data는 대체로 로컬 데이터를 의미할 떄가 많기 때문
  - `...mapGetters()` : computed와 getters매핑
  - `...mapActions()` : action과 methods를 매핑, actions의 경우 payload가 추가로 들어가므로 호출할 때 `@event=actionname()`이런 식으로 인자를 넘겨주면 정상적으로 작동한다. 원래 들어가야할 event객체를 넘겨주고 싶다면 넘겨줄 때 `$event`를 넣어주면 함수 내부에서 event를 잡을 수 있다. 


### 실제 사용

- 설치
  - `vue add vuex` : vuex를 추가, store dir과 그 안의 index.js가 생성된다.
- 중앙 데이터 가져오기
  - store/index.js에 state에 데이터 등록
  - `$store.state`로 접근해 필요한 요소를 가져온다.
  - 직접 끌어다 쓸 수도 있긴 하지만 computed와 같은 속성을 이용해 캐싱해 사용하는 것을 권장한다.
- 중앙 데이터 변경하기
  - mutations에 함수를 작성할 때는 상수값 쓰듯이 쓰게된다. 데이터 변경용 함수라는 것을 명시하기 위해서 이렇게 쓴다.
  - mutations를 호출할 때는 `this.$store.commit('mutationname', payload)`으로 호출하게 된다.
  - 이렇게 호출하고 데이터를 넘겨주면 mutations에 있는 함수가 작동해 중앙 데이터를 바꾸게 된다.
  - 다만 mutations을 직접 호출하는 방법은 권장하지 않으며, mutations에 연결된 actions을 만드는 방법을 권장한다.
  - `this.$store.dispatch('actionname',payload)`로 액션을 호출한다.
  - 그 후 action에서  `context.commit('mutationname',payload)`로 mutation을 호출해 중앙 데이터를 변경한다.

### 추가 플러그인

플러그인 을 쓰려면 index.js에서 `plugins: []`라는 속성을 새로주고, 넣으려는 플러그인을 저 안에 넣어서 실행해줘야한다.

- vuex-persistedstate
  - 설치는 `npm i vuex-persistedstate`
  - 지역저장소를 사용할 경우 이를 편리하게 해주는 플러그인
  - 사용하게 될 시 state의 내용들이 변경될 때마다 지역 저장소에 저장되어 브라우저를 새로고침해도 정보가 날아가지 않는다.

