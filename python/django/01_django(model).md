# django

## model

- 단일한 데이터에 대한 정보를 가짐
- 저장된 데이터베이스의 구조(모델 != 데이터베이스)
- 어플리케이션의 데이터를 구조화하고 조작하기 위한 도구
- 장고는 기본적으로 sqlite라는 db프로그램과 연결되어있으며 확장프로그램을 쓰면 이 sqlite의 db를 직접 볼 수도 있다.
- 장고로 프로젝트를 시작할 시에 db에 대한 구상을 마치고 시작하기 때문에 어떻게 보면 model이 가장 먼저 작성된다고 볼 수도 있다.

### 작성법

1. models.py 
   - model에 변경사항 기재
2. migration 작성
3. migrate


### database

체계화된 데이터의 모임

- schema(엑셀에서 1행에 적는 그 열에 적힐 데이터의 의미와 형식등을 모아놓은 것이다.)
  
  - 데이터베이스의 구조(자료의 구조, 표현방법, 관계 등)
- table(엑셀의 한 시트에 해당한다.)
  - field/column/속성(이 중 각 자료들을 구분하기 위한 고유값을 primary key라고 부른다. 이 pk는 반드시 필요하다)
  - record/row/tuple(엑셀의 한 행에 해당한다.)

장고는 디폴트 데이터베이스로 sqlite를 사용하고 있는데 vscode에서 확장프로그램을 쓰면 db를 직접 볼 수도 있다.

#### database API

- db를 조작하기 위한 도구
- 장고의 경우 기본적으로 orm으로 db를 구성하므로 db를 장고에서 조작할 수 있도록 돕는 기능 또한 가지고 있음.
- 이를 database api라고 하고 기본적으로 classname.manager.querysetAPI() 형식을 띄고 있다.
  - querysetAPI가 다양하므로 외울 수 있는 건 외우고 나머지는 문서를 참조하는 것이 좋다.
  - 걔중 몇개는 django.db.models에 미리 저장되어 있으므로 가져와서 쓰면 된다.
  - 이 중 하나인 .query는 이 명령이 sql로 어떤 명령인지를 출력해주는 속성이다.
  - EX)article.objects.all()
  - 다만 이를 파이썬의 shell에서는 바로 보기가 어렵기 때문에 이를 확인하면서 개발을 하기가 쉽지 않다 고로 이것을 보면서 하기 위해서 ipython이나 django-extensions를 추가하여 보면서 개발할 수 있게 해준다. 명령어는 `python manage.py shell_plus`이고 종료는 `exit`으로 하거나 강제로 종료해서 할 수 있다.
  - 이 때 옵션으로 --print--sql을 붙여서 실행하면 db에 가는 sql문도 함께 나오게 된다. 다만 이런 sql은 orm측에서 조작을 하기 때문에 limit가 걸려서 나오는등 조금 수정이 되어있다.
  - 이외에도 --notebook을 붙이면 주피터 노트북 환경에서 장고를 돌릴 수 있다.
- manager
  - 장고가 데이터베이스 쿼리작업에 제공되는 인터페이스
  - 기본적으로 모든 장고 모델 클래스에 objects라는 manager 추가됨
- queryset
  - 데이터베이스로부터 전달받은 객체 목록, 0,1, 여러개일 수 있다.
  - 데이터베이스로부터 조회, 필터, 정렬 등을 수행할 수 있다.
##### CRUD
create, read, update, delete의 가장 기본적인 데이터 처리를 묶어서 하는 말이다.

이 중 cud는 r과 달리 직접적으로 db에 손을 대고 다시 되돌릴 수 없는 행위들이기 때문에 보안성이 필요하다.

그래서 GET같은 오픈된 방법이 아닌 POST와 같이 숨겨서 보내는 방식을 쓴다.

이 때 내가 제공한 html폼태그에서 작성한 것이 맞는지를 확인하기 위해서, 폼태그 안에 `{% csrf_token %}`이란 것을 넣어서 준 후에 사용자가 제출을 할 때 내가 발행한 토큰이 맞는지를 기준으로 구분한다.

이게 인증이 안 될 경우 redirect를 통해 다른 곳으로 옮겨줄 수도 있다.

이 후 javascript에서 이를 활용하고 싶을 때는 document.querySeletor('[name=csrfmiddlewaretoken])로 잡는다.

- create
  - 만들어놓은 클래스를 인스턴스에 할당을 하고 인스턴스에 실제 값들을 넣는다.(이 때 생성하고 값들을 넣어줄수도 있고, 생성하면서 바로 넣어줄 수도 있다.)
  - `instancename.save()`를 하면 그제서야 db에 전송되어 쿼리로 등록이 된다.
  - 혹은 `create(title='third',content='django')`와 같이 데이터를 바로 넘겨서 만드는 방법도 있다.
  - sql에서의 동작 원리와 같이 장고에서 만들 때도 받은 정보가 모델의 항목을 제대로 가지고 있는지 아닌지를 확인한다. 이 때 액세스에서 유각 항목의 유무을 직접 설정하던 것과 달리 여기서는 default값이 있으면 항목의 유무에 어긋나더라도 일단 default값을 넣지만, default 설정조차 없을 경우 에러가 난다.
  - 여러 개의 더미 데이터를 만들 때 이런 코드를 쓴다.
  
  ```python
  from faker import Faker # 더미데이터를 만들기 위해 가상 데이터를 만들어주는 라이브러리다.
  
  class Person(model.Model):
      @classmethod
      def dummy(cls,n):
          f = Faker()
          for _ in range(n):
              cls.objects.create(name=f.name())
  ```
  
  - `get_or_create()` : 데이터를 받아 이와 일치하는 데이터가 있으면 가져오고 없으면 만들어준다. 이 때 반환값이 (객체, bool)형식이기 때문에 객체만 받아보려면 `[0]`를 해줘야한다. bool에는 가져왔다면 False를 만들었다면 True값이 반환된다.
  
- read
  - `all()` : 존재하는 모든 쿼리셋을 가져온다.
  - `get()` : ()내부에 조건을 넣으면 조건에 맞는 레코드 하나를 가져온다.  없다면 존재하지 않는다는 에러가, 2개 이상이면 값이 여러개가 있다는 에러가 난다. 고로 `get()`은 딱 하나 있는 값을 찾을 때 쓰는 편이며, 이를 항상 보장하는 것은 pk뿐이기때문에 대체로 pk로 조회할 때 쓴다.
  - `filter()` : ()내부에 조건을 넣으면 조건에 맞는 쿼리셋을 가져온다.
    - 조건을 넣을 때 and 조건은 ,로 연결 
    - or 조건의 경우 Q를 모든 조건 앞에  각각 넣어주고 |로 연결해서 사용
  - `order_by()` : db에 저장된 모든 자료를 주는 조건에 맞춰서 정렬시켜서 가져온다. ex) -pk의 경우 pk의 역순이라고 생각해 `all()`로 가져온것과 같다. 이는 pk말고 다른 column에서도 쓸 수 있다. 한 가지 조건 이상으로 정렬시킬 수도 있으며, 순서는 sql에서 쓰던 것과 같다.
  - `annotate(full_name=Concat('first_name','last_name'))` : sql상의 GROUP BY를 구현하기 위한 기능으로  db상의 가상의 column을 하나 더 만드는 기능이다. 다만 이를 만들 때 쿼리셋 하나에 대한 정보를 이용해 만드는 것일 때 쓴다.
    - `from django.db.models.functions import Concat` : db상에서 두 개의 str 컬럼을 붙여낼 때 쓰는 기능이다.
    - `from django.db.models import Count(columnname)` : columnname에 해당하는게 몇개인지를 세어주는 명령어로 연결관계가 존재할 경우 하나의 column에 여러 개의 연결이 있을 수 있게 된다. 이를 세기 위해서 쓰는 함수이다. 이렇게 하면 본래 db에 쿼리를  2~3번 날리는 일을 한번에 처리할 수 있게 된다.
  - `aggregate()` : 쿼리셋 하나가 아닌 테이블 전체나 일부분에 대한 연산을 수행해주는 명령어 이다. 다른 함수들과 조합하여 쓰인다.
  - `.iterator()` : 쿼리문이 요청한 자료 중에서도 필요한 내용만을 캐시에 올렸다가 쓰고 바로 내릴 수 있게 만드는 옵션이다.
  - options
    - `.values('columnname',)` : sql에서 SELECT 바로 뒤에 뭘 불러올 건지에 대해서 지정하는 곳이다. 안 적으면 기본값으로 전부 가져온다.
    - `.count()` : sql의 count와 같다 db에서 숫자를 세어서 장고로 숫자만 가져온다.
    - `[a:b]`: 파이썬에서 리스트를 자르는 부분인데 이를 쿼리셋 api에 붙여쓰면 가져온 뒤에 자르는게 아니라 이를 인지해서 OFFSET=a, LIMIT = b 옵션을 sql문에 붙여준다.
  - field lookups
    - 조회시 특정 조건을 여러 개 적용시키기 위해 사용
    - `filter`, `get`, `exclude` 에 넣을 때 쓴다.
    - `fieldname__lookuptype=value` 형식으로 쓴다. 지원하는 것이 꽤나 많으니 문서를 확인하여 사용하는 것을 권장
    - sql에서 본 것들이 조금씩 다른 이름으로 되어있는 경우가 많다.
    - `__gt`, `__gte` : greater than, greater than equal
    -  `__lt`, `__lte` : lower than, lower than equal
    - `__startswith`, `__endswith` : 시작할 때나 끝날 때 이 값이 맞는 지를 검사한다.
    - sql에서 쓰던 와일드 카드 `_` ,`%` 중 장고 orm에서는 `%` 만 사용가능하다. 그 이상을 하려면 정규표현식(regex)를 사용한다.
    - regex란 패턴을 어떻게 표현할 지에 대해 정해놓은 기준으로 python의 re 라이브러리가 이에 대한 기능을 가지고 있다.
  
- update
  
  - `save()`: pk값이 없는 값이라면 값들을 새로 추가해가면서 db에 추가하지만, pk값을 가지고 있는 객체를 빼와서 인스턴스에 할당하고 수정한 후 `save()`를 하는 경우 이 매서드가 있던 것을 수정한 것으로 인식하여 원래 값을 수정한다.
  - `update(columnname='value')` : sql문에서 update set에 해당하는 부분으로 어떤 컬럼의 내용을 뭘로 수정할 것인지를 적어줄 수 있다.
  
- delete
  
  - `delete()` : 레코드를 인스턴스에 할당한 후 이 매서드를 실행하면 이 인스턴스와 같은 리코드를 db에서 삭제한다.

### query

데이터를 조회하기 위한 명령어, 조건에 맞는 데이터를 추출 조작하는 명령어

### models.py

파이썬 클래스들을 적어놓는 곳으로 이를 orm을 통해 db와 연결한다.

클래스를 작성할 때는 app에서 사용하는 만큼 app의 복수형 이름에서 단수형으로 바꿔서 이름을 짓는 편이다.

이 때 단순 파이썬 클래스들은 orm을 통해 변환할 준비가 되어있지 않기 때문에, `from django.db import models`에서 `models.Model`이라는 부모 클래스를 상속받아와서 사용한다.

그 후에 클래스에 어떤 구조를 가질 것인지를 작성한다.

이 후에 이 구조에 요소에 해당되는 것들이 어떤 타입인지를 고려해 `models`의 안에서 적절한 타입을 찾아서 지정해준다.



이런 모델을 만들때는 thin control, fat model이라는 지침을 따르는 것이 좋은데, 말 그대로 컨트롤을 하는 부분에서는 쉽고 건드릴게 많지 않게 만들지만, 모델 면에서는 많은 정보량을 쥐고 있게 만드는 것이다.

orm은 models.py 내부의 class에서 class var만 확인하여 db의 column으로 만듬, 즉 그 외에 만드는 것들은 db에는 영향을 주는 것이 없다.

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True) #작성일 관련 정보는 관습적으로 이 이름을 쓴다.
    updated_at = models.DateTimeField(auto_now=True) #수정일 관련 정보는 관습적으로 이 이름을 쓴다.
    def __str__(self): # 출력시의 대표값을 정하는 메서드로, 이를 설정해놓으면 어드민이나 다른 곳에서 출력될 때도 적용된다.
        return self.title
```

- `models.CharField(max_length=)` : 문자를 받는 곳이긴 하지만, 최대치가 정해진 문자를 받을 때 쓰는 class이다. 그러므로 최대치를 필수인자로 가지고 있다. 다만 유효성검사를 이 구간에서 제대로 하진 않는다.
- `models.TextField()` : 문자를 받는 곳이며, CharField와는 다르게 최대치가 정해져 있지 않은 class이다.  
- `models.IntegerField()` : 정수를 받는 곳이다.
- 시간 관련 클래스
  - 다만 이 클래스들은 장고에서 가지고 있는 시간값을 넣어주는 것이기 때문에 실제 db에 저장된 시간을 가지고 오고 싶다면 sql구문을 사용하여 db의 시간을 가져와야만 한다.
  - `models.DateTimeField(auto_now_add=True)`  : 여기서는 장고가 가지고 있는 시간을 넣어준다.
  - `updated_at = models.DateTimeField(auto_now=True)` : 여기서도 장고가 가지고 있는 시간을 넣어준다.
- 공통 특성
  - `null=` : `True`로 할 시 빈 값을 받는 것을 허용해준다. 기본값은 false다.
  - `default=` : 뭔가의 데이터를 지정해놓을 경우, 값이 없을 때 이 디폴트값을 알아서 넣어준다.
  - `blank=True` : orm이 그냥 비어있는 것을 허용함. db에서는 null이라든지로 저장하지만 is_valid를 비어있을 때도 통과시켜준다는게 핵심
- subclass Meta
  - models.Model의 서브 클래스로 단순히 이 클래스가 가지는 특성외에 더 큰 단위에서 쓰이는 특성을 적는 곳이다.
  - abstract 옵션을 True로 줄 경우 models.Model이 가지고 있는 migration기능을 작동하지 않게 만들어줄 수 있다. 기본값은 False

### migrations

- 장고가 모델에 생긴 변화를 반영하는 방법
- 명령어(이 모든 명령어 또한 manage.py에 있다.즉 `python manage.py` 이후에 사용하면 된다.)
  - `makemigrations` : 모델을 변경한 것에 기반한 새로운 migration을 만들 때 사용, 이 때 원래 있던 migration파일에 수정을 하게 되면 이전의 자료들을 어떻게 변경할 것인지에 대해서 물어보게 된다. 이 때 1번은 직접 수정하는 것이고 2번은 이 과정을 일단 끝내고 models.py에 있는 class를 변경해 default값을 넣어달라는 뜻이다. 1번은 선택할 경우 데이터타입에 따라 장고가 기본적으로 가지고 있는 default값을 넣어줄 것인지를 물어본다. 이 때 그냥 enter누르면 장고가 알아서 해주긴 하는데 잘 보고 결정해야한다. 기본적으로 이 명령어를 쓰면 버전 관리를 하듯 migration을 버전별로 모두 저장을 해준다.
    - 추가로 `appname`을 쳐주면 이 앱에 대한 migration만을 만들어준다.
  - `migrate` : 생성된 migration을 db에 반영하기 위해 사용, 이를 통해 모델과 db의 동기화가 이루어짐
    - 추가로 `appname`을 쳐주면 이 앱에 대한 migration만을 진행한다.
  - `sqlmigrate appname num` : appname에 해당하는 num번째 마이그레이션에 대한 sql구문이 어떻게 되는지 보여준다.
  - `showmigrations ` :  마이그레이션파일들이 migrate가 됐는지 안 됐는지 여부를 확인할 때 쓴다. X가 되었다는 뜻이고 빈 것이 안 되었다는 뜻이다.
- 이 때 장고가 어디까지 migration이 진행이 되었는지를 db.sqlite3에 django_migrations에 저장해놓는다.

## orm

object relational mapping의 약자

객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템간에 데이터를 변환하는 프로그래밍 기술

프로그래밍 언어에서 사용할 수 있는 가상 객체 데이터베이스를 만들어 사용

즉 파이썬과 같은 프로그래밍 언어에서의 객체를 sql과 같은 데이터베이스언어와 서로 오갈 수 있게 만들어주는 변환기다.

- 장점

  - sql(Structured Query Language)을 잘 몰라도 db조작가능
  - sql의 절차적 접근이 아닌 객체 지향적 접근으로 인한 높은 생산성(현대 웹 프레임워크의 가장 중요한 점이 생산성이므로 이 점은 상당한 강점이다)

- 단점

  - orm만으로는 완전한 서비스를 구현하기 어려울 수도 있음

## 초기화
이 때 한번 구조가 틀어지면 이를 수정하는 것은 쉽지 않은데 장고의 경우 sqlite를 쓰기 때문에 정 고치지 못하겠고 내부데이터도 별게 없다면 그냥 db.sqlite파일 자체를 삭제해버리면 db를 초기화할 수 있다. 현업에서는 결코 해서는 안 되는 일이지만, 배우는 단계거나 개발단계에서는 이런 식으로 초기화할 수 있다는 것 정도는 알아두면 좋다.  이 때 따라야할 스텝은 다음과 같다.

1. db.sqlite 삭제
2. migrations 폴더 안의 내용물을 project를 시작했을 때와 동일하도록 새로 추가된 파일들 삭제
3. 새로운 구조로 migration 진행

이와 같은 과정을 거치면 project를 새로 시작해 구조를 처음 잡았을 때와 동일하게 된다. 데이터와 쌓여있던 변경점들은 모두 날아가니까 이 점은 참고하고 진행해야한다.

## admin
### automatic admin interface

- 서버 관리자가 사용하기 위한 페이지로

- admin.py에 등록하고 관리

- django.contrib.auth에서 제공하는 admin 페이지가 존재

### 사용법(django admin site)

- `admin.py`에 models를 import한다.
- `admin.site.register(Article)`이란 명령어로 models의 class를 등록한다. 이렇게 하면 admin권한으로 접속했을 때 이 앱의 db까지도 들여다볼 수 있게 된다.
  - 추가로 admin site의 구조를 바꿔주고 싶다면 class ArticleAdmin를 새로만들어 `admin.ModelAdmin`을 상속받게 한뒤 `list_display`라는 이름의 리스트에 띄울 요소들을 써준다.
  - 그 후에 `admin.site.register(Article, ArticleAdmin)` 으로 추가해주면 인식을 한다.
- `python manage.py createsuperuser` 를 통해 슈퍼유저를 만든다.
  - 이 때 required는 username과 password이며 나머지는 optional이다.
- 이 때 password는 원문으로 저장하지 않고 암호화과정을 거치기 때문에 알아볼 수 없다.

