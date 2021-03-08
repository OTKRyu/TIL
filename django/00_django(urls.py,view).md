# django

dynamic web application program

- dynamic
  - static web : 미리 저장된 정적파일을 제공
  - static과 다르게 사용자가 하는 요청에 따라 서버단에서 가공된 파일을 제공하는 것을 dynamic이라고 한다.

- web
  - protocol : request와 response로 이루어져 있으며 이 두 개가 한 사이클로 이루어진다.

- python web framework
  - 웹 페이지 개발하는 과정에서 겪는 어려움을 줄이기 위해 개발됨
  - 웹페이지를 위한 모든 과정을 하기보다는 이미 어느정도 되어있는 것들을 가져다 써서 쓸 수 있게 해준다.
    - 프레임워크의 성격
    - opinionated vs unopnionated , 독선전 vs 관용적
    - 장고의 경우 다소 독선적인 편이다.
- 장점
  - 거의 모든 작업이 되어있기 때문에 그런 작업을 다시 할 필요없이 중요기능 구현에만 신경을 쓸 수 있게 해준다.
- 모델-뷰-컨트롤러 모데 패턴(model-view-controller,mvc)
  - 소프트웨어의 디자인 패턴의 하나이다.
  - 다만 장고는 model-template-view라고 소개하고 있다.(하고있는 역할은 같다.)
  - model = 데이터베이스 관리, view = 중간 컨트롤러, template  = 레이아웃(화면)

- 어떻게 작동하는가
  - 요청
  - urls가 요청을 인식
  - view에서 model에 요청을 넘기고 데이터를 뽑아냄
  - 이를 view가 받고 template에 넘겨 반응할 결과물을 만들어냄
  - view에서 이를 요청자에게 반환
  - 이런 작동방식을 알고 이 순서대로 코드를 작성하게 되면 실수할 일이 줄어들고 조금 더 명확하게 볼 수 있다.

## django 프로젝트 구성

### project folder(혹은 master app)
- `__init__.py` : 이 파일이 이 프로젝트가 파이썬 패키지라는 것을 알려준다.

- `settings.py` : django의 모든 세팅이 들어있는 파일
  
  - internationalization : 국제화 관련 코드로 언어 및 국가별 다른 것들을 설정할 수 있다.
    - language_code : 언어 설정
    - time_zone : 기본값은 utc, 데이터베이스의 기준 시간을 정해준다. 이 때 대륙/도시이름 순으로 입력을 해주면 도시의 시간에 맞춰서 기준이 설정된다.
  
- `urls.py` : 사용자의 요청을 받을 때 쓰는 파일로, url을 지정해준다.
  
  - urlpatterns라는 리스트가 url들을 가지고 있다. 기본적으로 관리자용 url은 지정되어있다.
  - 추가할 때는 형식에 맞춰 path(sitename, sitename에 맞는 view 함수를 호출할 주소)로 적는다.
  
- `templates` : 앱에 들어갈 base templates를 놓는 곳으로 다른 앱들에서 공통으로 쓸  templates를 가져다놓는다. 이 폴더는 앱과는 다르게 장고가 이를 인식하지 못한다. 즉 새로 경로를 설정해줘야만한다. 이 경로를 설정할 때 settings.py에서 `TEMPLATES`의 `DIRS`에 BASE_DIR/'firstpjt'/'templates' 처럼 추가를 해줘야한다. 여기서 BASE_DIR은 프로젝트 생성시의 모든 걸 담고 있는 폴더를 의미한다.

### app(이름은 꼭 '복수형'으로, 생성(startapp) 후에 등록할 것)

- `templates(dir)` : 템플릿을 모아놓는 곳으로서 이는 앱을 만들 때마다 직접 만들어줘야한다.  이를 다른 이름으로 저장하면 django가 이를 인식하지 못하기 때문에 이 이름은 강제가 된다. 그리고 django가 app의 templates까지는 인식하므로, 이 안에서 어떤 파일을 찾을 지를 설정해주면 된다. 

- `admin.py` : 관리자 사이트를 만드는 파이썬 파일

- `apps.py` : 앱의 데이터가 들어있는 파일인데 수정할 일이 잘 없다.

- `models.py` : model에 해당하는 파일

- `tests.py` : 테스트용 코드를 작성하는 곳

- `urls.py` : 이 앱에서 쓸 view함수들을 모아놓은곳

    - 추가적으로 \<str:name>같은 것을 view를 붙여줄 수 있는데, 이를 variable routing(주소자체를 변수처럼 쓰는것)이라고 한다.
    - 이렇게 하면 request에서 일일히 정보를 뽑아낼 필요없이 url에 적혀있는 정보를 바로 변수로 받아서 함수에다 쓸 수 있다. 
    - str이 기본값이므로 생략가능하다.

- `views.py` : view의 역할을 하는 파일

  - views의 내부의 첫번재 함수의 인자는 반드시 request가 들어가야 한다.
  - 기본적으로 들어가있는 render라는 함수를 써서 return을 작성하는데 이 때도 render의 첫 번째 인자는 request이며, 두 번째 인자로 템플릿 경로를 쓴다.

### template

- 데이터 표현을 위한 도구 및 로직

- django가 빌트 인으로 가지고 있는 것이 있음 이를 DTL(django template language)라고 한다.

- DTL이 파이썬이랑 비슷하다, 다만 같은 것은 아니고 파이썬으로 실행되는 것도 아니다.

- DTL
  1. variable
     - `{{ variable }}` 형식으로 써놓고 render함수에서 template파일을 받을 때 이를 삽입해줌, 중괄호와 변수명 사이를 스페이스로 좀 띄워놓는 것을 권장
     - 밑줄로는 시작 불가,  공백 구두점 또한 사용불가
     - dot(.)을 사용하여 변수 속성에 접근 가능(딕셔너리의 경우 key로, list의 경우 인덱스로 접근가능)(ex)`dict.key`, `list.0`)
     - render함수의 세번째 인자로 들어가며 변수명과 실제 값의 딕셔너리 형태로 전달하게 된다.
     - 이 때 일일히 적기보다는 context라는 이름의 딕셔너리 변수를 만들어 저장해놓고 이를 조정하면 유지보수가 편하다.
  2. filters
     - `{{ variable|filter }}`
     - 표시할 변수를 수정할 때 사용(ex) `{{name|lower}}`
     - 60개정도 필터가 존재
     - chained가 가능(즉 연결하여 사용 가능), ex) `{{variable|truncatewords:30}}`)
  3. tags
     - `{% tag %}`
     - 출력 텍스트 가공, 반복 논리를 수행하여 더 복잡한 일들을 수행
     - 일분 태그는 시작과 종료 태그가 필요( `{% tag %}...{% endtag%}`)
     - 24개 정도의 태그가 내장되어 있다.
     - `{% url 'name' %}` : name이라는 이름의 url을 자기가 찾아서 넣어주는 역할을 한다.
     - template inheritance : 템플릿의 기능을 이어받는 기능으로 oop에서의 상속과 같은 개념이다.
       - `{% block content %}~{% endblock%}` : 부모 템플릿에서는 자식에서 바뀔만한 곳(override)을  block으로 지정하고, 자식 템플릿에서는 이 곳에 실제로 코드를 작성해 다르게 만들곳을 만든다.
       - endblock에서도 이름을 붙일 때가 있는데 여러 블록이 중첩되어 있을 경우 구분하기 위해 쓴다.
       - `{% extends 'parents path' %} ` : 받아올 부모의 것을 가져온다. 이 태그는 꼭 최상단에 존재해야한다.
  4. comments
     - `{# lorem ipsum #}`
   - 여러 줄의 주석은 `{% comment %} 내용 {%endcomment %}` 활용
  
- 장고의 템플릿 설계 철학

    - 표현과 로직을 분리(표현은 템플릿, 로직은 view(python)에서)
    - 중복을 배제(상속을 통해)

## django 명령어

- `django-admin startproject firstpjt` : `django-damin`을 통해 firstpjt라는 이름으로 새로운 프로젝트를 시작한다.
  - 프로젝트 이름을 만들 때 파이썬 내장함수들과 외부 라이브러리들, 및 장고에서 쓰는 용어들과 겹치는 이름을 쓰면 혼선이 생길 수 있으므로 쓰지 않아야 한다.
  - 하이픈이나 쉼표도 쓸 수 없으므로 언더바를 이용해 써야한다.
  - 이 때 프로젝트 네임 뒤에 .를 찍으면 현재위치에 만들라는 뜻이 되어 필요없는 폴더를 만들 일을 줄여준다.
- `rm -rf intro/` : 장고 어드민을 통해 만들었던 프로젝트 파일을 그냥 지워서 없앤다.
- `python manage.py runserver` : python을 통해 manage.py라는 파일을 실행시키고 서버를 실행한다. 이 때 manage.py라는 파일이 있는 곳에서 실행해야만 올바르게 움직이게 할 수 있다.
- `python manage.py startapp appname` : python으로 manage.py라는 파일을 실행시키고 appname이라는 이름을 가진 app을 시작한다. 이렇게 하면 app을 만든 세팅이 어느정도 끝나지만, 프로젝트에서 이를 인식하기 위해선 프로젝트의 settings에 appname이라고 추가해주면 된다. 다만 이 때 장고가 앱의 순서에 따라 어느정도까지만 읽을 때가 있으므로 local apps를 가장 먼저, 그 후 third party apps 그리고 django apps순으로 쓰는 편을 권장한다.
## django urls
- dispatcher로서의 url

- 웹 어플리케이션은 url을 통한 클라이언트의 요청으로부터 시작

- view함수가 많아지면 path 또한 많아지고 app 또한 많아지므로 프로젝트의 urls.py에서 일괄관리하는 것은 코드 유지보수에 좋지 않음, 그래서 기능들을 앱으로 옮기고 이 앱들에의 경로를 mapping을 한다

- 그래서 app에 각각 urls.py를 작성함 이 때 장고의 admin은 빼고 나머지를 전부 가져와서 app에 만든다.(admin은 프로젝트 단위에서만 쓰이기 때문에), 이 때 단순히 import만 해도 쓸 수 있지만 어디서 가져왔는지를 보다 명확히 보여주기 위해 from . 라는 표현을 붙여서 가져오는 것을 장고에서는 추천하는 편이다.(딱히 사용할 view가 없더라도 urlpatterns라는 리스트는 있어야한다. 없으면 에러난다.)

- 이렇게 되면 이제 프로젝트의 urls는 어떤 앱으로 연결할 지를 판단하여 보내주는 일을 해주면 되는데 이 때 필요한 것이 django.urls의 include 매서드이다. 이를 활용하면 다른 폴더에 있는 앱의 urls로 넘길 수 있도록 해준다.(ex) include('articles.urls'))

### naming url patterns

링크를 직접 url로 연결하는 것이 아니라 path()함수의 name인자를 정의해서 사용

이렇게 정의하면 url이 바뀔 때마다 요동치는 구조에서 이름을 통해서 찾아가는 구조로 변하기 때문에 구조가 조금 바뀐 정도로는 기능이 무너지지 않는다.


