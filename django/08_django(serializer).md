# django
## api server

프론트엔드와 백엔드를 구분짓기 시작하면 views.py에서 보내는 게 이젠 html이 아니라 json이 되게 되는데, 이 때 파이썬의 경우 dict로 만들어 json을 보내게 된다. 이 방법이 크게 3가지 정도 있다. 

1. 이 때 활용하는 함수가 JsonResponse로 `from django.http.response import JsonResponse` 에서 가져올 수 있다. JsonResponse에 필요한 요소로 자료와 `safe=`요소인데 딕셔너리 형태로 보낼때는 `safe=True`지만 딕셔너리가 아닌 자료를 보낼 때는 `safe=False`로 넘겨줘야한다.

2. 혹은 `from django.core import serializers`에서 `serialiezers.serialize`를 가져와 쿼리셋을 json으로 변환해준다. 이 때 사용법은 첫번째 요소로 어떤 타입으로 바꿀건지를 스트링으로 넣어주고 쿼리셋을 넣어주면 된다. 이 후 httpRespose를 통해 변환된 json파일을 그대로 보내줘도 된다. 이 때 추가 옵션을 줄게 있는데 `content_type=`인데 이것을 'application/json'으로 해줘야 정상적으로 작동한다. 이는 헤더에 들어가 어떤 타입의 데이터가 들어있는지를 명시해주는 기능을 한다.

   - serializtion(직렬화)는 데이터 구조나 객체 상태 다른 곳에서 재구성할 수 있는 포맷으로 변환하는 과정이다.
   - 예를 들어 쿼리셋같은 복잡한 데이터를 json이나 xml등으로 바꿔주는것이다.

3. 마지막으로는 이를 해주는 3rd party 라이브러리를 쓰는 것인데 djagno rest framework이다. 줄여서 drf라고 한다.

### rest_framework(drf)

회원가입과 로그인도 이런 프레임워크를 통해 만들거나 django_allauth같은 외부 라이브러리를 불러와서도 담당하게 할 수 있다.

#### serializer.py

   - drf의 serializer는 modelform과 유사하게 작동, serializers.py를 만들어 `from rest_framework import serializers`에서 serializers.modelserializer을 상속받는 클래스를 만들고 이후는 modelform을 활용하여 form을 만들 때처럼 Meta클래스를 써서 만들어주면 된다. 문법이나 구조가 modelform과 똑같으므로 자세한 활용은 그 쪽을 참고

   - 이 때 serializer의 항목을 적을 때 `read_only=True`옵션을 주면 한번 작성하고 나면 외부 접근으로는 변경할 수 없도록 고정시켜 높을 수가 있다.

   - 여기서의 serializer가 하는 것도 form과 비슷한데 데이터 검증과 보내줄 데이터를 특정 형식으로 만들어주는 일 2가지를 한다.

   - 여기에 연결관계가 추가되면서 복잡해 지는데, model에서 하던것처럼 serializer끼리도 참조관계를 가지기 시작해야한다.

```python
# 위쪽에 commnetserializer가 있다고 생각하고 보면, 이는 연결관계인 comment의 역참조용 매니저이름으로 commnetserializer를 가져와 같이 출력해주는 것이다. 근데 이 때 하나만 들어올게 아니기 때문에 many=True로 해준것이고, article측에서 comment를 수정할 수 없게 만들기 위해서 read_only_field에 추가해준다.
class ArticleSerailizer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True)
    # 단순히 다 가져오는 것이 아닌 커스텀해서 연결관계를 활용하고 싶다면 source에서 대상으로 하는 orm문을 어떻게 쓸지를 써주면 된다.
    comment_count = serializers.IntergerField(source='comment_set.count')
    class Meta:
        model = Article
        fields = '__all__'
        read_only_field = ('pk', 'comment_set', 'comment_count')
```

   - `serializer.save(article=article, user=request.user)`이렇게 되면 연결된 객체를 넣어주지 않으면 유효성검사를 통과할 수 없기 때문에 제외를 해주든 해야한다. 다만 제외를 하는 경우에는 데이터에서도 연결관계를 표시를 안 하게 되기 때문에 포함은 시켜주되, read_only_field를 따로 넣어서 제어를 다르게 해주는게 바람직하다.

   - 그리고 관계가 있는 객체를 실제로 json데이터에 동봉해서 보내주고 싶다면, serializer에 역참조용 매니저 이름으로 연결을 시켜줘야한다.

     

#### views.py

   - 이후 views에서 이를 불러와 쿼리셋을 변환해주면 되는데, 이 때 `many=`라는 요소를 설정해줘야한다. 단일 객체라면 False를 여러 객체를 동시에 변환한다면 True를 써야한다. 
   
     ```python
         serializer = ArticleSerializer(instance=None,data=request.data)
         articles = Article.objects.all()
         serializer = ArticleSerializer(articles, many=True)
     ```
   - `from rest_framework.response import Response` 에서 Response를 가져와 `return Response(serialized_data.data)`를 하면 된다.
- 검증 또한 폼처럼 `is_valide()`함수가 만들어져있으므로 이를 통해서 데이터 검증도 할 수 있다. 게다가 `raise_exception=True`라는 옵션을 주면 if,else로 분기할 필요없이 검증 실패시 자동으로 실패 이유를 보내준다.
   - drf에서 편의상 알아서 입력용 혹은 수정용 form도 만들어준다.

     - 데이터 검증에 쓸 때는 실패했을 때의 반응을 줘야하는데 위에서 자동으로 하는 방법 외에도 `Response(serialize.errors)`로도 할 수 있다. 다만 Response의 status 기본값은 200이므로 잘못된 요청을 받았을 경우에는 적절하지 않다. 고로 적절한 http status code를 찾아 status값을 넣어줘야한다. 그리고 errors도 form에서처럼 뭐가 틀렸는지를 알려준다.
   - 이전에는 `from rest_framework import status`해서 status값에 `status.http_status_code_400`과 같은 식으로 넣어줘야 했다. 다만 현재는 그냥 숫자만 넣어줘도 작동한다. 그 외에도 성공에도 이것저것 http status code가 따로 있으므로 이를 활용해주면 더 의미있게 만들 수 있다.
   - `from rest_framework.decoraters import api_view` 에서 데코레이터를 붙여줘야만 위 과정이 제대로 작동한다. 사용법은 @require_method와 같이 리스트로 만들어 넣어주면 되고 기본값은 GET만 허용이다. 이 외에도 api_view에서 drf가 만들어준 폼에서 보냈을 때, 지정한 형식과 맞는지도 검사해주는 기능을 가지고 있다.
   - request.POST는 사실 form태그에서 POST로 보냈을 때에만 저장되는 저장소로, 그 외의 형식에서는 저장이 되지 않는다. 그렇기 때문에 form태그를 쓰지 않고 보낸 방식에 따라서 정보를 얻으려면, request.data에서 정보를 얻어와야한다.
   - 연결관계가 생겼을 때 form에서 객체를 이어주기 위해서 db에 저장안하고 객체만 불러와 연결하던 것처럼 해줄 필요는 없고, `serializer.save(article=article, user=request.user)` 이렇게 저장과 동시에 객체를 넘겨주면 된다. 