# django

## forms

`from django import forms`

앱마다 만들어줘야되는 파일로 이름은 forms.py로 고정하길 권장한다.

models와 문법이 비슷하지만 구성은 완전히 다르기 때문에 이를 주의하며 작성해야한다.

폼을 쓰는 이유는 크게 2가지로 아래와 같다.

1. data validation

   1. 1차로 html form에서 제한이 걸려있기 때문에 이를 브라우저가 눈치채고 제한대로 만들어지지 않으면 경고를 준다.( 다만 이는 브라우저에서 개발자도구로 얼마든지 변경할 수 있기 때문에 사용자가 권고사항을 따르지 않을 경우를 대비해야한다.)

   2. 2차로 post형식으로 돌아왔을 때 forms의 클래스에 해당하는 인스턴스를 만들때 내용을 미리 채워서 인스턴스를 생성해본다.

      ex) `contact_form = ContactForm(request.POST)`

   3. 그리고 매서드 중 forms.Forms가 가지고 있는 매서드인 `.is_vaild()`라는 매서드를 통해 실제로 유효한지 아닌지를 확인해볼 수 있다.

   4. 더 구체적인 내용을 알고 싶을경우 `.errors`라는 매서드를 통해 뭐가 잘못됐는지도 받아볼 수 있다.

2. html easy(입력받기 위한 폼태그를 일일히 html에 칠 필요 없이 클래스만 만들어두면 언제나 쓸 수 있게 된다.)

forms의 작성방법은 models의 방식과 거의 흡사하다.

- forms.Form( ):이걸 쓸 경우 내가 직접 확인할 때나 개별로 쓸 때는 쓸 수 있지만 model과 연동이 되는 것은 아니다. 그러므로 특정 모델과 연결된게 아닌 데이터의 유효성만을 확인할 때 쓰게 되며 cleaned_data라는 딕셔너리를 만들게 된다. 유효성 검사 이후는 form에서 하는게 아니라 cleaned_data의 데이터를 이용하여 조작하게 된다.

  - forms 에서는 forms.Form를 상속받은 클래스를 만들게 되는데 이후에 views.py에서 이를 끌고와 쓰게된다.

  - 이걸 가져와 context에 넣어놓으면 내가 만들어놓은 forms대로 html의 form 구문을 채워주게 된다.

  - context를 통해 variable을 넣듯이 form 또한 넣으면 된다.

  - form을 html에 어떤 형식으로 넣을 것이냐할 때, html에 form 입력부에 .as_p로 하면 문단 형식으로 .as_table로 하면 표형식으로 주고 as_ul로 하면 리스트의 항목으로 표현한다.

  - 그 외에도 폼에 해당하는 타입을 줄때 widget이라는 속성을 줘서 html폼 내부에서 어떤 인풋을 써서 받아올지, 인풋에 어떤 클래스를 적용할지 등등을 결정해줄 수 있다. 대부분은 form클래스의 widget에 attr라는 속성에서 조정할 수 있다. 다만 기능은 거기까지로 유효성검사나 그런 것까지 widget형식에 맞춰주진 않는다. 

  - ```python
    from django import forms
    
    class ArticleForm(forms.Form):
        title = forms.CharField(max_length=10)
        content = forms.CharField(widget='Textarea')
    ```

- forms.ModelForm(): 이걸 상속받은 클래스의 서브클래스 Meta에 model이 뭔지와 어떤 fields와 연결할 지를 설정해주면 그 model과 연동이 된다. model을 통해 form class를 만들 수 있는 helper라고도 한다. 모델이 이미 정의되어 있을 때 사용하며, 이 구조자체가 db와 연동되어 있기 때문에 이를 바로 form에서 활용할 수 있다. 이 때 Meta클래스에 model과 fields에 아무것도 쓰지 않으면 장고가 웹페이지 유효성 검사를 할 때 거절을 당한다. 고로 fields를 직접 적어주던가 exclude로 제외할 fields를 적어주던가 둘 중 하나는 꼭 해야한다.

  - 양 쪽 모두 data는 request 안에 들어 있는 것을 말한다.

    객체를 넘기고 싶을 경우 instance 속성에 할당을 해서 넘겨줘야한다.

    데이터와 instance를 동시에 넘길 수 있으며 이렇게 넘겨주면 article의 정보를 request.POST의 데이터로 섞어서 적용해준다.

  - ```python 
    from .models import Article
    class ArticleForm(forms.ModelForm):
        password = None #이렇게 하면 원래 있는 필드라도 안 나오게 만들 수 있다. override한 것이다.
        	class meta:
                model = Article
                fields = '__all__' # 이 경우 모델에 있는 모든 필드를 폼에 적용하겠다는 뜻이다.
                # fields = ['username','email'] 처럼 하나씩 적어줘도 된다. 튜플과 리스트 모두 가능
    ```
  ```
    
  - 연동이 되기 때문에 model에서 하는 일을 실제로 form에서도 할 수 있다. 물론 이 일을 폼이 하는 건 아니고 연결된 모델에 정보를 넘겨줘서 실행하는 것이다. ex) `form.save()`(실제로 form에 폼클래스를 적용하여 하면 모델에서 하듯이 save()가 가능하다.)
  
  - 단순히 연동이 아닌 유효성검사등의 기능을 구현하고 싶다면 forms.Form을 쓸 때처럼 일일히 써줘야한다.
  
    그리고 받을 때도 model처럼 쓸 수가 있는데 이 때 알아서 form에 값에 할당을 해준다.( ex)article.title = 'hi' 면 html로 렌더링할 때 title자리에 value='hi'까지 해준다는 뜻이다.) 이 부분이 어떻게 돌아가는 지에 대해 더 자세히 알고 싶다면 form태그가 어떻게 구성되어 있는지 내부구조를 들여다보길 바란다.
  ```

- html에 적용할 때

```html
{% block "content" %}
  <form action=''>
      {{ form.as_p}}
      <input type="submit">
  </form>
{% blockend "content" %}
```

추가적으로 forms의 디자인에 관해서도 부트스트랩에서 해놓은 게 있기 때문에 이를 활용하면 더 쉽게 만들 수 있다. 추가적으로 django bootstrap이라고 장고의 경우 부트스트랩의 클래스를 적용하기 어려운 편이기 때문에 이를 한번에 일괄적으로 적용할 수 있게 만들어주는 3rd party app이 있다.

1. `pip install django_bootstrap5`
2. `settings.py` 에 `bootstrap5`에 등록
3. 쓰고자 하는 html의 `{% extend 'base.html' %}`아래에 없다면 최상단에  `{% load bootstrap5 %}` 를 작성
4. 이후 django bootstrap5의 가이드에 따라 작성