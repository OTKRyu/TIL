# django

## authentication and authorization

authentication: 신원 확인

authorization : 권한 확인

## django authentication system

인증과 권한을 동시에 제공하며 이 두개가 결합되어 있으므로 authenticaition으로 부름

이를 구현하긴 위한 form들은 미리 built-in되어있음(UserCreationForm, AuthenticationForm 등등)

장고는 세션과 미들웨어를 사용하여 인증 시스템을 request객체에 연결

고로 request.user에서 이 정보를 받아볼 수 있음

로그인이 안 된 경우 AnonymousUser라는 클래스의 인스턴스로 설정되며, 아니면 User 클래스의 인스턴스로 설정됨.  단순히 데이터만 넣는게 아니라 python형식의 class의 인스턴스 자체를 이 안에 저장하는 것이다.

### login and logout conform

장고에서 유저와 관련된 기능을 구현할 때 관습적으로 accounts라는 앱을 만들어 사용하는 편이다.

models.py

```python
from django.contrib.auth.models import AbstractUser
from django import models

class User(AbstractUser):
    # username. password, is_active 등등은 이미 AbstractUser에 있는 값들이고 이 아래에 내가 추가하고 싶은 항목들을 추가한다.
```

forms.py

```python
from django.contirb import UserCreationForm
from .models import User

class UserForm(UserCreationForm):
    # customize할 내용들
    class Meta:
        model = User
```

- login()

  - Session을  create하는 로직과 같음
  - 현재 세션에 연결하려는 인증된 사용자가 있는 경우 login 함수로 로그인 진행
  - request 객체와 User 객체를 이용해 진행
  - 이 때 key값으로 username과 password를 사용한다.
- logout()

  - Session을  delete하는 로직과 같음
  - request 객체를 받아서 진행하며 return은 없음
  - db에 세션 데이터를 삭제하고 클라이언트 쿠키에서도 session id를 삭제
- Session

  - 장고의 경우 session id를 발급하고 이를 사용자에게 전송하고, 돌아올 때는 쿠키를 사용해 연결된 세션을 확인
- 장고 프로젝트를 생성할 때 이와 관련된 기능을 장고측에서 만들어준다.
- 이 때 로그인했는 지 아닌지에 따라 다른 화면이나 기능을 제공해야하는 데 이 때 접근 제한하는 방법이 두가지가 있다.
  - is_authenticated attribute: 
    - User클래스의 속성중 하나로 User에 대해서는 항상 True, AnonymousUser에 대해서만 항상 False
    - 다만 이는 클래스가 뭔지만 판단하는 거지, 활성화가 되어있는지 혹은 권한을 가지고 있는지 같은 상세한 부분은 검사하지 않는다.
    - 이를 이용해 DTL의 if로 html상의 분기를 줄 수 있다. 다만 이는 보여지는 부분만 바꾼것이기 때문에 강제로 접근하고자 하거나 하면 얼마든지 접근이 가능하다.
  - login required decorator
    - `from django.contrib.auth.decorators import login_required`
    - 사용자가 로그인 했는지 확인하는 데코레이터
    - 로그인 안 한 유저일 경우 settings.LOGIN_URL에 설정된 경로로 redirect
    - 이렇게 리다이렉트 되었을 경우, 로그인만 되면 들어갈려고 하던 웹페이지로 바로 다시 갈 수 있도록 `?next=`라고 url에 붙여준다. login_required에서 next를 붙여주지만 실제로 그쪽으로 이동시키는 건 개발자가 해야되리 일이다. next항목은 GET에 있기 때문에 request.GET.get('next')로 next가 있는 없는 지를 확인해 보내줄 수 있다. 이 때 `redirect( request.GEt.get('next') or 'articles:index')`로 분기해서 작성할 수 있다. 이렇게  next로 보내게 되는 request는 방식이 get일 수밖에 없기 때문에 만약 post로만 작동하는 view함수 일 경우 문제가 생길 수 있다. 이런 때는 attribute를 사용해서 처리해 줄 수도 있다.

####  actual use

- login 

```python
from django.contrib.auth.forms import AuthenticationForm # 얘는 폼이다 모델폼이 아니다. 즉 유효성검사에만 쓰이고 실제 저장할 때는 다른데 저장하기 때문이다.
from django.shortcuts import render, redirect
from django.contrib.auth import login as auth_login

def login(request):
    if request.method == 'POST':
        form = AuthenticationForm(request, data=request.POST) # 다른 폼과는 다르게 첫 번째 인자로 reqeust, 두 번째 인자로 데이터가 들어간다.
        if form.is_valid(): # 보통 폼과는 다르게 AutehnticaitonForm.is_valid()는 user db에 접근해 실제로 있는 유저인지를 확인까지 해준다.
            auth_login(request, form.get_user()) # 세션 create 하는 내장 함수, 첫 번째 인자로 request, 두 번재 인자로 user가 들어간다. 이 때의 session관련 정보는 db의 django.session에 저장된다. django가 설정하는 session은 기본적으로 permanent session이다.
            return redirect('articles:index')
    else:
        form = AuthenticationForm()
    context = {
        'form':form,
    }
    return render(request, 'accounts/login.html', context)
```

- logout

```python
from django.contirb.auth import logout as auth_logout
from django.shortcuts import render, redirect
from django.views.decorators.http import require_POST

@require_POST
def logout(request):
    auth_logout(request) #쿠키와 db의 세션에 해당하는 id를 모두 지운다.
    return redirect('articles:index')
```

## user crud

장고가 기본적으로 유저와 관련된 db나 이런걸 만들어주기 때문에 어떤 user모델을 쓸 건지를 기본값으로 적어놓는다. 만약 이를 바꾸고 싶다면 setting.py에 AUTH_USER_MODEL에 적어서 경로를 바꿔주면 장고가 알아듣고 기본 유저모델을 자기가 만든 유저모델로 바꿔준다. 이 때 완전한 경로를 적는 것이 아니라 `'accounts.User'`정도로만 써줘도 장고가 알아서 경로를 잡는다. 장고 내부의 원리가  AUTH_USER_MODEL이 하나일 때를 기준으로 하고 있기 때문에 이에 해당하는 것은 꼭 하나만 해야한다.

이렇게 새로운 USER모델을 만들고 싶다면 accounts/models.py 에 추가해서 만들어주는 방법이 있다.

장고에서 미리 만들어놓은 AbstractUser라는 클래스가 있으므로 이를 상속받아와서 새로 만들어주면 내가 추가하고 싶은 것을 추가하고 덮어씌우고 싶은것은 덮어씌울 수 있다.

장고가 정해놓은 기본값 외에 쓸 일이 없다고 하더라도 이를 만들어 두는 것을 추천한다. 왜냐하면 나중에 이를 확장할 때 필요하기도 하거니와 이렇게 해놓으면 바로 AbstractUser 클래스를  뒤지기보단 먼저 어디를 참조해야할 지 알 수 있기도 하기 때문이다.

그리고 이렇게 만든 User의 정보에 해당하는 form도 forms.py에 넣어줘야한다.

- create

  - 이번에는 UserCreationForm을 사용
  - UserCreationForm은 기본적으로 장고의 forms.ModelForm을 상속받은 클래스이므로 첫 번째 인자로 data를 넣으면 된다.
  - 이렇게 해서 유효성 검사를 거치고 나면 save()를 해주게 되는데 이 때 UserCreationForm도 save시에 user객체를 반환하게 된다.
  - 나머지는 login과 동일하다.
  - 다만 단순히 등록만 하고 끝내는 게 아니라 로그인상태로 만들어주고 싶다면 로그인단에서 `auth_login(request, user)`를 추가해주면 된다.
- read

```python
from django.contrib.auth import get_user_model
User = get_user_model() #유저는 어차피 딱 한 개이기 때문에 여기저기서 끌어다 쓰기보다는 get_user_model을 써서 한 번만 신경쓰면 알아서 가져오게 하는 것이다.

def index(request):
    users = User.objects.all()
    context = {'users':users,}
    return render(request, 'accounts/index.html', context)
```



- update

  - UserChangeForm의 경우 admin이 일반 사용자들을 관리할 수 있는 것만큼의 권한을 제공한다. 즉 너무 많은 권한을 제공하기 때문에 이 폼 그대로는 쓸 수가 없다.
  - forms.py

  ```python
  from django.contrib.auth.forms import UserChangeForm
  from django.contrib.auth import get_user_model #user model은 여러군데에서 쓰는 장고 안의 파일이기 때문에 이를 직접 끌어다 쓰는 것은 권장하지 않는다. 만에 하나라도 user model 말고 유저 모델을 커스텀하게 되면 사용할 수 없게 되기 때문이다. 고로 활성화되어 있는 모델을 가져오는 함수를 써서 쓰게 된다.
  class CustomUserChangeForm(UserChangeForm):
      class Meta:
          model = get_user_model()
          fields = ('email', 'first_name', 'last_name',) # 변동하지 않는 항목이기 때문에 튜플로 자주 쓰는편이다. 그리고 비밀번호의 경우 암호화하여 저장하기 때문에 비밀번호는 특별 취급으로 내가 추가하지 않아도 알아서 작동한다. 
  ```

  - views.py에는 이 customUserChangeForm을 가져와 update에 사용하면 된다.
  - article의 업데이트와 달리 이번 인스턴스는 이미 request.user에 저장되어 있으므로 그대로 가져다 쓰면 된다.
  - 그 외에도 PasswordChangeForm같은 것도 있다.

- delete

  ```python
  @require_POST
  def delete(reqeust):
      if request.user.is_authenticated:
          # auth_logout(request) 를 추가하면 로그아웃을 진행한 후 삭제를 시키는 식으로도 가능하다. 물로 삭제하면 이 user에 있던 모든 정보가 의미가 없어지므로 굳이 추가하지 않아도 된다.
          request.user.delete()
          return redirect('article:index')
  ```

## User(model)

장고 인증 시스템의 핵심으로 하나의 User class만 존재하며 AbstractUser class의 상속을 받음

AbstractUser는 AbstractBaseUser와 PermissionMixin을 상속받아서 만들어져 있고, user model을 구현하기 위한 기능을 모두 갖추고 있는 클래스이다. AbstractBaseUser도 나름 기능을 갖추고 있지만 AbstractUser만큼 기능이 다양하지 않고 쓰기 위한 준비가 더 많기 때문에 기본적으로는 AbstractUser를 쓴다.

# vscode 단축키

특정 객체 더블 클릭후 ctrl+d를 하면 동시에 선택할 수 있다.