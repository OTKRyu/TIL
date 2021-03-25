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

### a many to one relationship(1:n)

- foreign key(외래 키)
  - 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
  - 참조하는 테이블에서 1개의 키에 해당, 참조되는 측의 pk를 의미
  - 하나의 테이블에 여러 개의 외래 키를 가질 수 있고, 각각의 외래 테이블과 연결될 수 있다.
  - 참조하는 테이블의 행 여러개가 참조되는 테이블의 동일한 행을 가르키고 있을 수도 있다.
  - 참조하는 테이블과 참조되는 테이블이 동일할 수도 있다(재귀적 외래키)
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

### ORM

- `modelname.objects.count()` : sql의 count와 같다 db에서 숫자를 세어서 장고로 숫자만 가져온다.
- 