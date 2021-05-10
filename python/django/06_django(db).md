# django

## db
relationship fields

모델 간 관계를 나타내는 필드

- 1:n
- m:n

데이터 베이스 무결성

- 참조 무결성 : 참조할 때 혼선이 생기지 않게 되어 있는지
- 데이터 무결성 : 데이터 자체가 정확하게 의미있는 값만을 가지고 있는지

장고에서 db에 접속하는 방법

`python manage.py dbshell` : 을 이용하면 db에 직접적으로 접속하여 sqlite3을 쓸 떄처럼 sql문으로 db를 조작할 수 있다.

json의 경우 db에 접속할 필요 없이 그냥 loaddata 명령이 데이터를 읽어줬지만 csv같은 경우 형식이 맞지 않아 이는 불가능하다.

고로 dbshell까지 들어가서 db에 데이터 넣듯이 넣는 수밖에 없다.

그리고 db와 장고는 결국 별개의 프로그램 간의 통신으로 이루어져 있으므로 이 둘 간의 통신을 최대한 적게 하고, 그 과정에서 드는 메모리 또한 적게 하는 것이 좋다. 다만 이 것만 신경쓰다가 정작 중요한 구조를 놓치게 된다면 본말전도이므로, 최적화를 한다면 좋아진다는 확신이 들었을 때 하는 것이 가장 좋다.

- LAZY
  - db가 쿼리와 관련해서 일을 잘 안 한다는 의미에서 나온 말이다.
  - 장고의 쿼리셋 api가 똑똑해서 실제로 값을 가져와야만 하는 경우가 아니면 쿼리를 db에 날리지를 않는다는 뜻이다.
  - 실제로 db가 일하는 시점은 쿼리를 평가할 때 쿼리를 db로 날리게 되고 이 때 받아온 쿼리셋을 캐시에 저장한다. 이를 잘 활용하면 쿼리를 덜 날리고 더 효율적으로 프로그램을 짤 수 있다. 다만 특정 요소에 접근하는 경우에는 캐시에 저장되지 않을수도 있다.
  - 이 평가라는 것은 쿼리를 db에서 실행해 실제로 데이터를 꺼내오는 시점을 얘기하며 몇 가지 상황이 있다.
  - 반복을 시작할 때와 참,거짓 평가될 때 쿼리가 실제로 db에 날아가므로 이를 고려해서 짜면 더 빠른 웹 서비스를 만들 수 있다.

### many to one relationship(1:n)

- foreign key(외래 키)
  - 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
  - 참조하는 테이블에서 1개의 키에 해당, 참조되는 측의 pk를 의미
  - 하나의 테이블에 여러 개의 외래 키를 가질 수 있고, 각각의 외래 테이블과 연결될 수 있다.
  - 참조하는 테이블의 행 여러개가 참조되는 테이블의 동일한 행을 가르키고 있을 수도 있다.
  - 참조하는 테이블과 참조되는 테이블이 동일할 수도 있다(재귀적 외래키)
  - 참조가 되는 쪽이 target model, 참조를 한 쪽을 source model이라고도 부른다.
- 특징
  - 참조 무결성을 지켜야한다(유일한 값만을 사용)
  - 외래 키의 값이 꼭 기본 키일 필요는 없지만 참조 무결성은 지켜져야한다.
  - 기본적으로 1:n 관계에서 n에다가 foreignkey 작업을 해주게 된다.
- Foreignkey arguments
  - on_delete: 참조되고 있던 테이블의 값이 사라졌을 때 어떻게 처리할 지를 정의, 데이터 무결성을 유지하기 위해 중요함
    - CASCADE: 참조되던 테이블이 삭제되면 참조하던 객체도 삭제
    - PROTECT: 참조되던 테이블이 삭제되면 오류 발생
    - SET_NULL: 참조되던 테이블이 삭제되면 Null값 입력(속성 중 not Null이 있으면 불가)
    - SET_DEFAULT: 참조되던 테이블이 삭제되면 Default값 입력
    - 등등 나머지는 공식 문서 참조
  - related_name: 장고가 만들어주는 modelname_set이라는 manager의 이름을 설정할 때 사용, 기본값이 들어있으므로 굳이 수정할 필요는 없지만, 정해놓은 이름이 있을 경우 써주면 된다. ex)related_name = comments, article.comments.all()
    - 1:n관계에서는 써도 그만 안 써도 그만인데 m:n관계가 되면 반드시 이름을 관리해줘야할 필요가 생긴다.
    - 같은 테이블을 두고도 여러 번 참조될 일이 생기는데 이러면 같은 이름의 relatedmanager가 생겨버리기 때문이다.
    - 그 외에도 관계가 복잡해짐에 따라 가독성을 높이기 위해 이름을 잘 설정해줘야하는 필요성이 생긴다.
    - djagno2에서까지만 해도 related_name이 symmetrical 옵션을 함께 가지고 있는 셈이였다. 왜냐하면 자기자신과의 관계를 만들고 있는데 역참조가 필요하다는 사실이 관계의 방향성이 있다고 말하는 것이기 때문이다. 그렇기 때문에 django2에서는 symmetrical 옵션이 아닌 related_name이란 옵션을 사용해 방향성을 통제한다고 생각해도 된다. symmetrical 옵션이 django3에 오면서 생겼기 때문에 이런 차이가 좀 생겼을 뿐이다.
    - 다만 이게 좀 이상한게 django3 식으로 'self'로 모델을 지정하게 될 시 symmetrical옵션을 줘야만 비대칭적 관계로 작동한다. 2방식으로 하거나 아예 3방식으로 맞추거나 둘 중 하나만 해야된다.
- actual use
  - 아래의 코드의 순서를 바꾼다 하더라도 db에 저장될 때, 속성과 외래 키 순으로 붙기 때문에 외래 키는 db에서 오른편에 모여있게 된다.
  - 그리고 db에 저장될 때는 article이 아닌 article_id라는 항목으로 만들어지게 된다.

models.py

```python 
from django.conf import settings #models.py에서 유저 모델을 참조할 때는 이렇게 사용한다. 이렇게 되면 class객체가 아닌 스트링을 가져오게 되는데, 이는 장고의 구동원리와 관련이 있다. 장고가 runserver를 할 때 installed_app에 있는 app을 순서대로 구동하게 되는데, 이 때 순서가 잘못되면 활성화되지 않은 상태로 내가 지정해놓은 customuser가 아닌 auth.user를 가져올 가능성이 생긴다. 따라서 항상 같은 경로를 지정하고 있을 settings.AUTH_USER_MODEL을 가져와서 이런 순서에 따른 오류를 미리 방지하는 것이다.
class Comment(models.Model):
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(atuo_now=True)
    article = model.ForeignKey(Article, on_delete=models.CASCADE) #이름은 뭘로 지어도 괜찮으나 참조 테이블과의 관련성을 명확히 보여주기 위해 이런식으로 작성하는 편이다. foreignkey는 첫 번째 인자로 스트링과 클래스 모두 받을 수 있다.
    user = model.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    
    def __str__(self):
        return self.content
```

views.py

```python
# 단순 댓글 추가
comment = Comment()
comment.content = 'str'
article = Article.objects.get(pk=article_pk)
comment.article = article #즉 foreignkey항목을 작성할 때는 해당하는 클래스의 인스턴스를 넣어줘야한다.

# form이 추가된 경우
def detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    comments = article.comment_set.all()
    comment_form = CommentForm()
    context = {
        'article' : article,
        'comments' : comments,
        'comment_form':comment_form
    }
    return render(request,'detail.html',context)

@require_POST
def comment_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid(): # form을 만들 때 content에만 유효성검사를 하게 만들어뒀기 때문에 content만 멀쩡하면 통과는 한다. 다만 저장은 할 수 없다. article 요소가 없기 때문
        comment = Comment_form.save(commit=False) # 그래서 commit이라는 실제 db에 저장할거냐는 옵션을 False로 준다.
        comment.article = article
        # comment.article_id = article_pk 라고 해도 상관없이 저장은 되나 권장하지는 않는다.
        comment.save()
        return redirect('article:detail', article_pk)
    context = {
        'articles' : articles,
        'comment_form':comment_form
    }
    # 이렇게 해야 유효성 검사를 통과하지 못했을 경우 뭐가 틀렸는지 쓰여있는 페이지를 보내줄 수 있기 때문
    return render(requset, 'articles/detail.html', context)
'''
@require_POST
def comment_delete(request, comment_pk): #혹은 처음부터 인자를 article_pk를 받아서 짤 수도 있다.
    comment = get_object_or_404(Comment, pk=comment_pk)
    comment.article.pk = article_pk
    comment.delete()
    return redirect('articles:detail', article_pk)
'''
# 위 방법도 가능은 하나 아래의 경우가 restful_api를 만드는 방식에 더 가깝다.
@require_POST
def comment_delete(request, article_pk, comment_pk):
	comment = get_object_or_404(Comment, pk=comment_pk)
	comment.delete()
	return redirect('articles:detail', article_pk)
```

forms.py

```python
class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ('content')
        # fields = exclude('article')
```

- 역참조: 참조되고 있는 쪽에서 참조하는 쪽을 찾아보는 것을 말한다.

  - 다만 장고에서는 model들을 정의할 때 참조하는 쪽에만 정보를 가지고 있기 때문에 평범하게는 이 관계를 찾을 수가 없다.

  - 고로 이를 위해선 특수한 manager인 modelname_set을 써야한다. 혹은 미리 정해준 이름을 쓰거나.

  - 이 때 modelname은 다 소문자화된다.
  
  - 이 명령어 DTL에서도 알아듣는다.
  
  - ```python
  article.comment_set.all()
    ```

### many to many relationship(m:n)

1대 n의 경우 하나의 댓글이 하나의 게시글에만 있을 경우처럼, 하나에 대해서 가지치는 형태로밖에 만들지를 못한다. 이런 한계를 극복하기 위해서 m:n 모델을 고민했다. 그 결과 나온 것이 중개모델로 두 개의 모델 사이의 관계성을 아예 새로운 테이블을 만들어 저장해놓는 것이다. 이 중개모델은 사실 양쪽의 모델과 1:n관계를 가진 테이블로 1:n 두개를 이어붙여 m:n으로 만든 것이다.

다만 이 m:n용 중개테이블을 만들 때 하나의 참조 필드를 정해서 만들기 때문에 둘 다 역참조를 하든가, 혹은 하나만 역참조를 하든가 해야한다. 이게 중개모델을 직접 참조해가면서 자료를 끌어오는 방법과 그냥 통과해서 가져오는 2가지 방법이다. 참조필드에 해당하는 쪽에서 역참조를 하지 않고 바로 데이터를 가져올 때 쓰는 것이 through option이다.

```python
# Doctor와 Patient의 중계 모델(테이블)
class Reservation(models.Mode):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    
# 예약생성
reservation.objects.create(doctor=doctor1,patient=patient1)
# 예약조회
doctor.reservation_set.all()
patient.reservation_set.all()
# 예약삭제 혹은 둘 사이의 관계를 끊음
reservation = Reservation.objects.filter(doctor=doctor1,patient=patient1)
reservation.delete()
```

```python
# manytomanyfield를 활용한 경우(어느쪽에 연결해놔도 상관이 없다.)
class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor,, related_name='patients') # 다대다 관계이므로 복수형을 써서 구분하는 편이다. 이렇게 만든 테이블은 appname_patient_doctors라는 이름으로 중계 테이블이 만들어지게 된다. 그리고 원리는 중계 테이블을 만드는 것과 똑같기 때문에 이 항목은 필수가 아니다. 그리고 중계 테이블을 직접 만든 경우에 through=tablename 옵션을 주면 새로 만드는게 아니라 중계테이블과 연결을 해주게 된다. related_name은 역 참조용 매니저의 이름을 지정하는 명령어로 이렇게 하면 어느 테이블에서 참조하더라도 규칙에 맞는 명령어를 쓸 수가 있게 된다.
    
# 예약생성
patient.doctors.add(doctor1)
# 예약조회
patient.doctors.all()
doctor.patient_set.all()
# 예약삭제 혹은 둘 사이의 관계를 끊음
doctor1.patient_set.remove(patient1)
patient1.doctors.remove(doctor1)
```

이렇게 두 가지 방법으로 m:n을 구현할 수 있는데, 중계테이블을 직접 만들어 쓰는 대표적인 예시는 중계테이블이 연결관계뿐만이 아니라 추가 컬럼을 가지고 있을 때이다.

- models.ManyToManyField(): m:n 관계를 만들기 위해 장고에서 미리 만들어둔 필드, 원리는 중계필드를 만들고 한쪽에 relatedmanager를 생성하는 것이다. 
  - related_name : foreignkey의 related_name과 동일
  - symmetircal 
    -  자기 자신을 참조하는 경우 역참조용 매니저가 생성되지 않는다.
    - 대신에 대칭적이라고 생각하여 한 쪽이 다른 한 쪽을 참조하게 되면, 동시에 다른 한 쪽도 한 쪽을 참조하는 것으로 간주해 생성해준다. 이 때 db에서는 양방향성을 유지해주기 위해 한번의 관계를 만들면 양방향에 해당하는 두 개의 관계를 db에 저장하게 된다.
    - 이를 원하지 않는다면 symmetrical=False를 해주야 하며 기본값은 True다.
    - 이렇게 재귀적 참조를 하면서 대칭관계를 끄게 되면, db에 만들어지는 이름이 from_model_id, to_model_id 이런식으로 바뀌게 된다.
  - through='tablename'
    - 다대다 관계의 경우 중개 테이블을 자동으로 생성한다.
    - 다만 중개 테이블에 여러가지 추가 컬럼이 있을 경우 직접 만들어야된다. 그 때 그 중개 테이블을 이어주기 위해 through 옵션을 쓴다.
    - 이 때 1:n 연결관계와 추가로 만들어놓을 필드를 모두 작성된 중개 테이블을 만들어야 된다.
    - 이렇게 만들게 되면 객체 입장에서 관계테이블을 직접 건드리지 않으면서, 관계테이블에 접근해 변경을 할 수 있게 된다.
- method
  - add()
    -  지정된 객체를 객체 집합에 추가, 이미 존재하는 관계를 하려하면 복제되지는 않는다
    -  한 번에 여러개를 ,로 연결해 넣으면 여러 개가 생성된다.
    -  `through_default = {'colname':'content'}` : 이런 옵션을 주면 추가할 때 들어가는 값들에 대해 through에 해당하는 필드의 디폴트를 잠깐동안 딕셔너리에 넣어놓은 내용으로 대체시켜 넣어줄 수 있다. 이 방법을 사용하면 중개 테이블의 추가 컬럼에 해당하는 내용을 넣어줄 수 있다.
  - remove()
    - 관련 객체 집합게엇 지정된 모델 객체를 제거
    - 한 번에 여러 개를 ,로 연결해 넣으면 여러 개가 제거된다.

#### actual use

- like 구현

models.py

```python
class Aricle(models.Model):
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
```

urls.py

```python
path('<int:article_pk>/likes/', views.likes, name='likes'),
```

views.py

```python
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)
        if article.like_users.filter(pk=request.user.pk).exists(): # 존재하는지 여부만을 알아오는 쿼리셋이다. 이렇게 하면 훨씬 빠르다. 왜냐하면 죄다 검색해서 가져오는 것도 아니고 있기만 하면 바로 True로 가져온다는 점과, 가져왔을 때 in으로 찾는 것 또한 n짜리 작업이기 때문이다. 추가적으로 exists는 cachy에 쿼리를 저장하지 않기 때문에 메모리면에서도 효율적이다.
            article.like_users.remove(request.user)
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('articles:index', article_pk)
```

- follow 구현

models.py

```python
class User(AbstractUser):
    followings = models.ManyToManyField('self',related_name='followers',symmetrical=False)
```

urls.py

```python
path('<int:user_pk>/follow/', views.follow, name='follow')
```

view.py

```python
@require_POST
def follow(request, user_pk):
    person = get_object_or_404(get_user_model(), pk=user_pk)
    if request.user.is_authenticated:
        if request.user != person:
            if person.followers.filter(pk=request.user.pk).exists:
                person.followers.add(request.user)
            else:
                person.followers.remove(request.user)
            return redirect('accounts:profile', person.username)
        else:
            return redirect('accounts:profile', person.username)
     else:
        redirect('accounts:login')
```

