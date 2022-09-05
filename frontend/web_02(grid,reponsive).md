# web

## layout

요소들을 취합하고 배치하는 것

### float(다른 레이아웃 요소들이 많이 생기면서 본래의 목적으로 돌아간 편)

원래는 기사의 인라인 부분중에 사진이나 그림의 레이아웃을 잡기 위해 도입, 점점 발전해 전체적인 레이아웃을 만드는데까지 발전

두 개 이상의 요소가 같은 쪽에 띄우게 되면 같은 줄에 순차적으로 쌓이게 됨

말 그대로 내가 보는 입장에서 띄우는 것이기때문에 레이아웃이 무너질 수 있음.

- none 기본값
- left: 요소를 왼쪽으로 띄움
- right: 요소를 오른쪽으로 띄움

- float clear : 부모요소에게 `clearfix::after`를 이용해 뜬 요소 뒤에 float가 해제된 블록레벨의 빈 요소를 만들어 레이아웃이 붕괴되는 것을 막는다.

``` css
<!--이름은 clearfix로 짓는 것이 관례다-->
.clearfix::after{
    content: "";
    display: block;
    clear: both;
}
```

### flexbox

정식명칭은css flexible box layout

요소간 공간 배분과 정렬 기능이 있는 1차원 단방향 레이아웃(한 줄에서 배분과 정렬을 조정한다는 뜻)

단반향이기 때문에 flex-item이 많아지면 테두리를 그냥 넘어가게 된다.(기본값이 nowrap이기 때문) 이를 해결하기 위해서는 flex-wrap 속성을 조작해 나가지 못하게 해야하며 이 경우 상하에 차례대로 쌓을 수 있다.(wrap은 아래로 wrap-reverse는 위로 쌓아나간다.)

기본적으로 좌에서 우로 쌓여나감, 하지만 축을 바꾸는 기능을 이용해 세로로도 작성할 수 있음

- 요소

  - flex container(부모)

    - ```css
      .flex-container{
          display: flex;
          <!-- flex items를 강제로 일렬로 정렬시킬 것인지를 결정 -->
          display: inline-flex; 
      }
      ```

  - flex item(자식)

- 축

  - main-axis
  - cross-axis

- 속성(self속성뺴고는 전부 flex-container에서 조작)

  - `flex-flow` : flex-direction과 flex-wrap의 shortened
  - 배치 방향 설정(`flex-direction`) : 기본값은 좌우, 축을 변경해 세로로도 가능(메인축을 바꾸는 것)

  방향도 바꿀 수 있다.(row,row-reverse,column,column-reverse)

  - 메인축 방향 정렬(`justify-content`)
    - `flex-start` : 시작방향쪽 정렬
    - `flex-end` : 끝방향쪽 정렬
    - `center` : 중앙 정렬
    - `space-around` : 아이템 외각에 균일하게 공간을 준 후 정렬한 것
    - `space-between` : 아이템부터 시작해 아이템으로 끝남, 아이템간의 사이 공간을 균일하게 맞춘 것
    - `space-evenly` : 여백부터 시작해 여백으로 끝남, 여백과 아이템 간격을 모두 균일하게 맞춘 것
  - 교차축 방향 정렬(`align-items`,`align-self`,`align-content`)
    - `content` : 여러 줄 조작(여러 줄이 되려면 `wrap` 속성이 wrap이여야만 한다.)
    - `items` : 한 줄(stretch라고 칸에 맞춰 늘리는 것이 기본값)
      - `baseline` : 기준선에서 폰트 크기에 맞춰 크기를 조절해 정렬
    - `self` (self속성): flex item 개별 요소
      - 개별 요소에 적용하는 것이기 때문에 다른 명령어들과는 다르게 flex-item에 직접 클래스를 적용해 줘야한다.
  - 순서 조작(`order`, self속성) : 작은 숫자일 수록 시작방향으로 이동(기본값 0)
  - 크기 조작(`flex-grow`,self속성) : 메인 축에서 남는 부분을 얼마씩 가져갈지를 결정해 크기를 조정하는 방법(기본값 0)

- 우선순위

  - line
  - item

### bootstrap

프론트엔드 오픈소스로 반응형 그리드 시스템중에서는 가장 유명하다고 한다.

내장으로 만들어져 있는 것들이 많이 있다.(responsive라는 뜻은 어떤 매체로 보든 그에 맞춰 조정된 페이지를 보여주는 것을 말한다.)

one source, multi use(하나의 코드로 여러개의 매체에 대응할 수 있게 짜는 것)

bootstrap의 기본기준이 브라우저의 것과 다르기 때문에 같은 코드를 실행해도 다른 결과로 보일 수 있다.

bootstrap 파일중 min이 붙은 것은 속도를 빠르게 하기 위해 양식을 조금 간략화한 것으로 일반적으로 min이 안 붙은 것을 쓴다.

추가적으로 startbootstrap.com같은 사이트에 가면 부트스크랩으로 이미 만들어진 템플릿이나 레이아웃을 가져올 수 있다.

#### CDN

content delivery network의 약자

컨텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 제공하는 시스템

개별 유저와 가까운 서버를 통해 빠르게 전달 가능하고 본인 서버에 저장을 안 하기 때문에 부하가 적다.
#### responsive web design
같은 페이지를 여러 화면으로 보게 된 상황으로 변했기 때문에 이를 하나의 페이지를 크기에 따라 다르게 볼 수 있도록 디자인의 개념이 변했는데, 이를 반응형 웹 디자인이라고 한다.

#### class(flex기능 그대로 사용 가능하나, 명령어가 조금 다르기 때문에 이를 찾아보고 써야함)

- m:  마진, p : 패딩
- t , b, s, e, x, y : 탑, 바텀, 레프트, 라이트, x축, y축
- d : display로 -inline -block 두 개로 속성을 조정할 수 있다.
  - 추가적으로 d-none과 d-block을 이용하여 가로축 브레이크포인트에 따라 보이고 보이지 않는것을 설정할 수 있다.
- xs, sm, md, lg, xl, xxl : small, meddile, large 등의 약자로 d와 조합하면 페이지 화면의 크기에 따라 나오게 할지 안 나올지 선택가능 이를 6개의 브레이크 포인트라고도 부른다. 이중 아무것도 안 쓴것이 xs라고 생각하면 된다.
- 0~5 : rem으로 분류된 단위 
- w: width로 w-100하면 `<br>`과 같은 역할을 클래스 내에서 해준다.
- auto : 그 auto다 정렬할 때 다른 것과 조합하면 어느쪽으로 정렬할지 사용가능
- text : 글자 관련에 앞에 붙고 뒤에 뭐가 붙냐에 따라 정해진다.
- fixed-top,fixed-bottom : 위나 아래 요소에 붙여서 안 움직이게 하는 방법이다.
- img-fluid : 화면크기에 맞춰 알아서 조정을 해준다. 자세히는 원본 크기보다 커지지 않게 하는 동시에 가로비율에 맞춰 세로길이를 조정해 주는 것이다.
- g: gutter의 약자로 좌우 패딩을 조정하는 역할을 한다.
- modal : 트리거와 모달 두 파트로 이루어져 있으면 이 두 가지 모두 써줘야 작동한다. 이 둘은 id로 엮여있다.


#### 사용법

1. bootstrap을 가져오면 자동으로 reboot.css(normalize, gentl)를 써서 브라우저에서 각기 가지고 있는 유저세팅을 백지로 돌린다.
2. 가져온 bootstrap의 클래스들을 이용하여 적용한다.



#### bootstrap grid system(!= css grid)

12개의 컬럼(12의 수의 약수가 많기 때문에)

6개의 break point

```html 
<div class="container">
    <div class="row">
        <div class="col">
            
        </div>
    </div>
</div>
```

- 이 때 col이 12칸중 몇칸을 차지하게 할 것인지를 -4와 같이 적어준다.

- 이 때 한 row안에 들어있는 col의 수가 12가 넘어가면 다음 줄로 넘겨버린다.

- 더 편해져서 개체에 col만 주고 row단에서 row-cols라는 이름을 통해 한단에 컬럼이 몇개씩 보일 건지를 설정할 수도 있다.

- 이 row-cols는 기본값이 자의적이므로 하면서 확인해봐서 조정이 필요하면 추가로 코드를 작성해야한다.

- nesting이란 한 컬럼 안에서 또 다시 분할을 하는 것을 의미한다. 원래 하던대로 col안에 row를 추가해주고 그 안에 col을 분할해서 추가해주면 된다.

- offset은 12개의 col을 다 쓰지 않고 비워두기 위해 쓰는 기법으로 col에 offset으로 몇칸을 줄 건지를 정해주면 앞쪽에서 그 수치만큼 띄고 나서 시작한다.  이 때 col과 offset이 다른 개체라고 생각할 수 있는데, 이는 붙어있는 하나의 개체이고 고로 하나의 개체를 기준으로 움직인다.

- 단순 container말고도 container-fluid를 쓰면 화면의 가로를 100%쓸 수 있다.

- 설정해준 구간이 없을 경우 기본값으로 들어가게 되는데 이 경우 col-12다.

  