# pjt 4

db 자체를 git으로 공유하지는 않지만, 데이터 자체를 json 형식으로 공유할 수 있다.(db자체를 공유하지 않는 것은 db에 저장할 때의 방식이 다를 가능성도 크고, 만약에 충돌이 나더라도 단순한 data를 바꾸는 것과 db안의 데이터를 바꾸는 것은 난이도의 차이가 크기 때문이다.)

이런 식으로 더미데이터를 공유하는 방식을 fixture라고 한다.(django fixture 참고)

이 때 `djanog-admin dumpdata appname.modelname ` 혹은 `python manage.py dumpdata appname.modelname`

`dumpdata`도 옵션이 몇 개 있는데 이 때 `--indent num`란 옵션은 몇칸을 들여쓰기한 데이터로 만들 것인지를 지정해준다. 없다면 그냥 붙여서 출력

이를 `>`를 이용해 원하는 형식의 파일에 저장하면 된다. 주로 json에 저장한다.

반대로 이를 받기 위해서는 `python manage.py loaddata path_to_file`를 하면 더미데이터를 db에 저장해 준다. 이 때 db에 저장할 때 pk를 기준으로 덮어씌워버리기 때문에 주의해야한다.

이 때 데이터가 db에 반영되려면 db에 맞는 자료여야하기 때문에 이를 확인하려면 db에 이미 그 구조가 있어야한다. 고로 db에 schema까지는 적용해놔야한다.

pair programming으로 진행

# pjt 5

resize 관련 django-imagekit 사용법은 공식문서 참조

processedImageField의 upload_to 속성을 사용할 때 결국 경로를 정해줘야하는데, 이를 쓸 때 년월일로 쓰고 싶다거나 하면 image/%Y/%m/%d 와 같은 형식으로 만들어 줄 수 있다.

form.html을 미리 만들어두면 현재까지 진행한 바로는 new, edit이나 같은 형식에 다른 값만 보내는 것이므로 한번의 작업으로 2가지 일을 같이 할 수 있다. 이 때 값이 다른 부분을 분기하기 위해서 if문과 `request.resolver_match.url_name`이라는 값을 사용한다.

이 값은 이 요청이 어디로부터 왔는지에 대한 정보중 그 경로의 이름을 저장하고 있는 변수로, 이를 통해 이 요청이 어느 경로로 왔는 지를 알 수 있다. django로 생각해보면 어떤 view함수로 들어왔는지를 저장하는 것이다.

django.shortcuts 에서 get_object_or_404이란 함수를 가져올 수 있는데 쓸 떄는 `get_object_or_404(modelname, pk=model_pk)`로 쓸 수 있다. get과 마찬가지로 pk에 해당하는 model 하나를 반환한다.

# pjt 6

깃 관련 내용이라 딱히 여기다 적지 않았다.

# pjt 7

소셜 로그인 구현

django-allauth 라이브러리 사용

기본적으로 저 라이브러리 사용법을 그대로 따라가면 된다.

masterapp.urls.py의 include는 해당되기만 하면 include안 쪽의 경로를 모두 다 가져온다. 고로 같은 이름으로 여러개의 경로를 짜도 멀쩡히 작동한다.

추가적으로 큰 규모의 플랫폼의 정보를 끌어와야되기 때문에 api활용이 필요한데 이에 대한 설정은 강의 참조

이 때 접근해야되는 api이름은 OAuth인데 인증을 다른 사이트에 맡기고 이를 통해 본사이트에서의 인증을 처리하는 방법이라고 생각할 수 있다.

pagination 관련

말 그대로 page를 만들어주는 기능인데, 한 페이지에 몇 개의 데이터를 보여줄지를 이를 통해 간편하게 구현할 수 있다.

관련 내용은 django paginator 항목 참조

이 또한 꾸미기 위한 라이브러리가 존재한다. django bootstrap pagination 검색하여 이를 활용하면 된다.