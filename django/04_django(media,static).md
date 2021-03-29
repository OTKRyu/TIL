# django

## static(내가 미리 정해놓은 거)

응답용 html을 보낼 때 html의 경로를 온라인 경로가 아니라면, 서버가 제대로 찾아주지 못한다.

그렇기 때문에 내 컴퓨터에 있는 자료를 올리고 싶다면 장고가 찾을 수 있는 곳에 자료들을 넣어놔야한다.

그 것이 `static/`으로 시작하는 url 요청이다. 이는 setting.py 안의 STATIC_URL에서 확인할 수 있다.

static 또한 templates 처럼 장고에서 기본적으로 찾는 파일들이 있는데 이게 appname/static이라는 폴더들이다

만약 그 외에도 찾고 싶다면 `STATICFILES_DIRS = [PATH/]`를 등록해 줘야한다.

이 때 URL을 직접 쓸 수도 있지만 배운 것처럼 `{% url 'path' %}`으로 경로를 찾거나 혹은 `{% static 'appname/filename' %}`식으로 찾을 수도 있다. 다만 static경로를 사용할 경우 이는 기본 설정이 아니기 때문에 `{% load static %}`을 해줘야만 똑바로 작동한다.

static도 다른 파일들처럼 name spacing을 할 수 있다.



## media(사용자가 내게 보낸 거)

pillow 필요 (`pip install pillow`)

데이터베이스에 저장할 때는 img경로만 잘 저장하면 되기 때문에 ImageField를 쓰긴 하지만 이거 어차피 charfield다.

결국 진짜로 데이터를 받는 url의 부분은 files부분이다.

그래서 forms.modelForm으로 넘겨줄때 두번째 인자인 files자리에 request.FILES을 넘겨주면 저장이 되게 된다.

이렇게 들어오게 된 파일의 경우 아무 설정이 없을 경우 project root에 기본적으로 저장이 되게 된다.

그래서 MEDIA_ROOT를 지정해줘서 이쪽으로 실제 데이터를 보내게 만든다.

그리고 사용자가 자기가 보낸 파일을 조회하고 싶을 수 있는데 이 때는 MEDIA_URL를 따로 만들어 경로를 지정하게 된다.

그리고 이를 찾게 만들어주기 위해서는 masterapp의 urls.py에 아래를 해줘야한다.

```python
from django.conf.urls.static import static # 이 옵션은 개발 중일 때만 쓴다. 출시하면 이거 말고 다른 방식으로 한다.
from django.conf import settings

urlpatterns + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # 이부분은 debug=True일 때만 작동하니까 걱정할 필요없다.
```

html에서 이를 불러올 때는 아래의 코드를 이용해 파일을 넣을 곳에 대신 넣어주면 된다.

```html
{{ instance.image.url }}, {{ instance.image }}
```

이 때 이미지 파일이 없다면 이로 인해 에러가 날 수 있기 때문에 이를 DTL의 if를 이용해 있을 때만 보여주도록 유도를 해야한다.

서버의 용량이 무한대인 것이 아니기 때문에 이를 관리할 수 있도록 받는 폴더의 용량을 조정하거나 아니면 경로별로 보내 정리해야하는데 이를 django-imagekit이라는 패키지로 조절할 수 있다.