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
    - 말 그대로의 의미로 독선적일 수록 프레임워크가 해놓은 작업이 많고 써야하는 형식이 고정적이며, 관용적일 경우 프레임워크보다 자기가 직접 정해야할 것이 많으며 형식이 자유로울 수 있다.
    - 장고의 경우 다소 독선적인 편이다.
- 장점
  - 거의 모든 작업이 되어있기 때문에 그런 작업을 다시 할 필요없이 중요기능 구현에만 신경을 쓸 수 있게 해준다.
- 모델-뷰-컨트롤러 모델 패턴(model-view-controller,mvc)
  - 소프트웨어의 디자인 패턴의 하나이다.
  - 다만 장고는 model-template-view라고 소개하고 있다.(하고있는 역할은 같다.)
  - model = 데이터베이스 구조를 잡고, db와의 연결을 관리 장고의 경우 orm이 db에 해줘야할 일을 대신 해주다보니 이 model단이 db관련 조작을 총괄한다, view = 중간 컨트롤러, template  = 레이아웃(화면)

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
- `settings.py` : django의 모든 세팅이 들어있는 파일은 django.conf.settings.py에 있지만 개발자가 자주 바꿀 법한 것들은 접근이 편한데서 수정할 수 있도록 이곳에 모아놓았다. 실행을 한다면 django.conf.setting.py를 기반으로 masterapp/settings.py 를 override하여 작동한다.
    - internationalization : 국제화 관련 코드로 언어 및 국가별 다른 것들을 설정할 수 있다.
    - language_code : 언어 설정
    - time_zone : 기본값은 utc, 데이터베이스의 기준 시간을 정해준다. 이 때 대륙/도시이름 순으로 입력을 해주면 도시의 시간에 맞춰서 기준이 설정된다.
    - installed_apps : 설치된 앱들을 나타내는 리스트
      - 이 앱들을 탐색할 때 이 리스트의 순서대로 그냥 탐색해서 선착순으로 가져오기 때문에, 같은 이름을 가진 개체가 다른 앱에 있을 경우 꼬일 수 있다.
      - 이를 위해 name spacing이 필요하다.
      - name spacing이란 단순히 같은 이름을 가진 파일들이더라도 어디에 소속되어 있는지를 추가로 적어줌으로서 혼선을 벗어나는 일로서, 그 앱의 이름과 같은 폴더 안에 자료들을 넣어둠으로서 `appname/file` 식으로 찾게 만드는 것이다.
      - ex) `pages/useful_someting.html` in templates 
    - middleware : 구현해놓은 프로그램을 실제로 작동하기 앞서 보내준 요청을 한번 걸러주는 역할을 한다. 보안적인 면에서의 역할을 주로 한다.
    - debug : 이 항목을 true로 해놓고 서버를 돌리면 어떤 에러인지를 상세히 알려준다. 개발단계에서는 켜놓고 하는게 좋지만, 상품발매단계가 되면 필수적으로 꺼야한다. 보안 사항도 전부 보여주기 때문에.
    - MEDIA_ROOT : 여기에 내가 받을 파일의 경로를 저장해 놓으면, 서버에서 파일을 받아올 때 이걸 참고해 원하는 위치에 저장을 해준다.
    - templates: 프로젝트에 쓰이는 templates 관련 세팅을 적어놓는곳
      - DTL이 기본적으로 내장하고 있는 정보들이 context_processors에 적혀있다.  그 중 몇 가지가 request와 user다.
      - 이런 형식으로 바꿔주는 것은 middleware가 해주는 일이다.
    - SESSION_COOKIE_AGE: 이 항목의 경우 장고가 내장하고 있는 세션 발급 기능을 쓸 때 세션을 얼마나 길게 유지할 지를 설정해 줄 수 있다. 
- `urls.py` : 사용자의 요청을 받을 때 쓰는 파일로, url을 지정해준다.
  - urlpatterns라는 리스트가 url들을 가지고 있다. 기본적으로 관리자용 url은 지정되어있다.
  - 추가할 때는 형식에 맞춰 path(sitename, sitename에 맞는 view 함수를 호출할 주소)로 적는다. 기본적으로 view와 직접 연결을 하는 것과 가능하나, app이 많아질 경우 masterapp에서 이를 다 연결해놓는 것은 유지 보수 측면에서 좋지 않으므로 include함수를 이용해 각 앱의 ulrs.py로 옮기는 역할을 더 많이 한다.
- `templates` : 앱에 들어갈 base templates를 놓는 곳으로 다른 앱들에서 공통으로 쓸  templates를 가져다놓는다. 이 폴더는 앱과는 다르게 장고가 이를 인식하지 못한다. 즉 새로 경로를 설정해줘야만한다. 이 경로를 설정할 때 settings.py에서 `TEMPLATES`의 `DIRS`에 BASE_DIR/'firstpjt'/'templates' 처럼 추가를 해줘야한다. 이 `DIRS`는 그 아래에 있는 `APP_DIRS`가 찾을 templates폴더 외에 것을 찾을 목록에 추가하는 역할을 한다. 여기서 `BASE_DIR`은 프로젝트 생성시의 모든 걸 담고 있는 폴더를 의미하고, 절대 경로가 바뀌더라도 프로젝트 루트의 위치를 그때마다 잡아주는 역할을 한다. 관습적으로 마스터 앱이나 혹은 프로젝트 root에 만드는 편이다.

### app(이름은 꼭 '복수형'으로, 생성(startapp) 후에 등록할 것)

- `templates(dir)` : 템플릿을 모아놓는 곳으로서 이는 앱을 만들 때마다 직접 만들어줘야한다.  이를 다른 이름으로 저장하면 django가 이를 인식하지 못하기 때문에 이 이름은 강제가 된다. django가 templates를 찾을 때 settings.py에 있는 installed_apps에 있는 앱 안의 templates들을 찾기 때문에 이를 등록해줘야한다. 그렇게 templates라는 폴더를 찾으면, 이 안에서 어떤 파일을 찾을 지를 설정해주면 된다. 

- `admin.py` : 관리자 사이트를 만드는 파이썬 파일

- `apps.py` : 앱의 데이터가 들어있는 파일인데 수정할 일이 잘 없다.

- `models.py` : model에 해당하는 파일

- `tests.py` : 테스트용 코드를 작성하는 곳

- `urls.py` : 이 앱에서 쓸 view함수들을 모아놓은곳

- 추가적으로 \<str:name>같은 것을 url에 붙여서 보내온 경우 이를 views에 있는 함수에서 인자로 받을 수가 있는데, 이를 variable routing(주소자체를 변수처럼 쓰는것)이라고 한다.
    - 이렇게 하면 request에서 일일히 정보를 뽑아낼 필요없이 url에 적혀있는 정보를 바로 변수로 받아서 함수에다 쓸 수 있다. 
    - str이 기본값이므로 생략가능하다.
    
- `views.py` : view의 역할을 하는 파일, 작성하는 법은 크게 두 가지로 function_based와 class_based가 있다. function_based는 url로 연결한 주소로 요청이 들어올 시 그에 해당하는 함수를 작동하게 만든 것이고, class_based는 장고에서만 지원하는 고유한 작성방법으로 class와 그 안의 요소들로 작동하게 만드는 것이다. 이 방법을 쓰면 django측에서 해놓은 것이 많기 떄문에 훨씬 간편하고 빠르게 백엔드를 만들 수 있다.
    - views의 내부의 첫번재 함수의 인자는 반드시 request가 들어가야 한다.
      - 기본적으로 들어가있는 render라는 함수를 써서 return을 작성하는데 이 때도 render의 첫 번째 인자는 request이며, 두 번째 인자로 템플릿 경로를 쓴다. 그리고 세번째로 context라는 정보가 담긴 딕셔너리를 넣게 된다.
      - 추가로 항상 html만을 보내주는것이 아닌 다른 url로 보내는 것을 해야될 때도 있는데 이 경우 render와 같은 패키지에 있는 redirect라는 함수가 필요하다. 이 함수에 `'/practice/'`와 같이 직접 보낼 경로를 쓰거나 또는 urlname을 쓰면 그 쪽으로 이동시켜준다. 인자가 필요한 경우 `argumentname = real_argument`식으로 ,로 연결해 보내면 된다.
    - `from django.shortcuts import get_objects_or_404(class, kawrgs=value)` : 인자를 받아서 조회를 해보고 결과가 없으면 404페이지를 보내는 함수다. 
    - 오는 정보의 방법이 get이냐 post냐에 따라서 분기를 해서 같은 페이지에 요청이 오더라도 다른 일을 하도록 시킬 수도 있다. 물론 get이 올 때와 post가 올 때 완전히 다른 일을 시킬 때에만 이런식으로 짜야한다. 혼선이 있을 가능성이 없을 때만.
    - `from django.http import HttpResponse`를 통해 접근할 수 있으며 잘못된 접근이 일어났을 때 장고에서 미리 만들어둔 페이지를 보내줄 수 있다. status 요소에 몇 번 오류인지를 적어주면 알아서 만들어주는 함수다.

### template

- 데이터 표현을 위한 도구 및 로직

- django가 빌트 인으로 가지고 있는 것이 있음, 이를 DTL(django template language)라고 한다.

- DTL이 파이썬이랑 비슷하다, 다만 같은 것은 아니고 파이썬으로 실행되는 것도 아니다.

- 이렇게 출력할 때 단순한 데이터를 출력하는 것보다 사람들이 자주 쓰는 표현으로 쓰는 것을 선호할 수 있는데 이럴 때는 django.contrib.humaize, 혹은 django humanize 문서를 참조하길 바란다. 그리고 이는 장고의 기본 세팅에 들어있지 않으므로 사용하기 위해선 app에도 추가해주고 templates에도 load를 해줘야 한다.

- DTL
  1. variable(사실상 print다)
     - `{{ variable }}` 형식으로 써놓고 render함수에서 template파일을 받을 때 이를 삽입해줌, 중괄호와 변수명 사이를 스페이스로 좀 띄워놓는 것을 권장
     - 밑줄로는 시작 불가,  공백 혹은 구두점 또한 사용불가
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
     - `{% with followings=person.followings.all followers=person.followers.all%}`,`{% endwith %}`
       - 이 구문을 사용하면 이 사이에 있는 구문들에 대해서는 with안에 쓰인 변수 할당문이 작동하는 것처럼 쓸 수 있다.
     - `{% url 'name' %}` : name이라는 이름의 url을 자기가 찾아서 넣어주는 역할을 한다.
       - 만약 url에 해당하는 path가 추가 arguments를 필요로 한다면 `{% url 'name' argument argument %}` 식으로 넘겨주면 된다.
       - space로 구분하고 이 인자를 넘기는 것은 파이썬의 함수의 인자를 넘기는 것과 똑같이 기능하기 때문에 순서대로 넘기는 것도 가능하고 키 밸류로 넘기는 것도 가능하다.
     - template inheritance : 템플릿의 기능을 이어받는 기능으로 oop에서의 상속과 같은 개념이다.
       - `{% block content %}~{% endblock%}` : 부모 템플릿에서는 자식에서 바뀔만한 곳(override)을  block으로 지정하고, 자식 템플릿에서는 이 곳에 실제로 코드를 작성해 다르게 만들곳을 만든다.
       - endblock에서도 이름을 붙일 때가 있는데 여러 블록이 중첩되어 있을 경우 구분하기 위해 쓴다.
       - `{% extends 'base.html' %} ` : 받아올 부모의 것을 가져온다. 이 태그는 꼭 최상단에 존재해야한다.
       - 추가적으로 `{% include 'other.html' %}` 을 이용하여 다른 html의 구조를 가져올 수 있다. 이를 이용하여 코드를 다 분리시킬 수 있으며 유지보수를 더 쉽게 만들 수 있다. 작동원리를 보면 other.html의 문서내용을 그대로 붙여넣는 것으로 django html의 기능까지 모두 잘 작동한다. 다만 block의 경우 뚫어놓은 곳과 채울 곳이 쌍으로 작동해야하기 때문에 잘못 넣을 경우 작동하지 않는다.
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
- `rm -rf projectname/` : 장고 어드민을 통해 만들었던 프로젝트 파일을 그냥 지워서 없앤다. 이게 프로젝트를 없애는 방법이다.
- `python manage.py runserver` : python을 통해 manage.py라는 파일을 실행시키고 서버를 실행한다. 이 때 manage.py라는 파일이 있는 곳에서 실행해야만 올바르게 움직이게 할 수 있다.
- `python manage.py startapp appname` : python으로 manage.py라는 파일을 실행시키고 appname이라는 이름을 가진 app을 시작한다. 이렇게 하면 app을 만든 세팅이 어느정도 끝나지만, 프로젝트에서 이를 인식하기 위해선 프로젝트의 settings에 appname이라고 추가해줘야 한다. 다만 이 때 장고가 앱의 순서에 따라 어느정도까지만 읽을 때가 있으므로 local apps를 가장 먼저, 그 후 third party apps 그리고 django apps순으로 쓰는 편을 권장한다.
## django urls
- dispatcher로서의 url(uniformed resource locator)

- 웹 어플리케이션은 url을 통한 클라이언트의 요청으로부터 시작

- view함수가 많아지면 path 또한 많아지고 app 또한 많아지므로 프로젝트의 urls.py에서 일괄관리하는 것은 코드 유지보수에 좋지 않음, 그래서 기능들을 앱으로 옮기고 이 앱들에의 경로를 mapping을 한다

- 그래서 app에 각각 urls.py를 작성함 이 때 장고의 admin은 빼고 나머지를 전부 가져와서 app에 만든다.(admin은 프로젝트 단위에서만 쓰이기 때문에), 이 때 단순히 import만 해도 쓸 수 있지만 어디서 가져왔는지를 보다 명확히 보여주기 위해 from . 라는 표현을 붙여서 가져오는 것을 장고에서는 추천하는 편이다.(딱히 사용할 view가 없더라도 urlpatterns라는 리스트는 있어야한다. 없으면 에러난다.)

- 이렇게 되면 이제 프로젝트의 urls는 어떤 앱으로 연결할 지를 판단하여 보내주는 일을 해주면 되는데 이 때 필요한 것이 django.urls의 include 매서드이다. 이를 활용하면 다른 폴더에 있는 앱의 urls로 넘길 수 있도록 해준다.(ex) include('articles.urls'))

### naming url patterns

링크를 직접 url로 연결하는 것이 아니라 path()함수의 name인자를 정의해서 사용

이렇게 정의하면 url이 바뀔 때마다 요동치는 구조에서 이름을 통해서 찾아가는 구조로 변하기 때문에 구조가 조금 바뀐 정도로는 기능이 무너지지 않는다.

이 때도 templates와 마찬가지로 중복이 날 수 있기 때문에 똑같이 name spacing을 하는데 이 때는 폴더가 아니라 app_name 이란 변수를 urls.py에 추가해 설령 다른 앱에서 같은 이름을 쓰더라도 구분이 될 수 있도록 한다. 이를 실제로 끌어다 쓸때는 {% url 'app_name:url_name'}식으로 쓴다.
