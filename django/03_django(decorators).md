# django

## view decorators

어떤 함수에 기능을 추가하고 싶을 때 해당 함수는 수정하지 않고 기능을 연장 해주는 함수, 장고는 다양한 기능을 제공

함수위에 어떤 decorator를 적용할지를 `@decorator` 형식으로 써주면 된다.

기본적으로 데코레이터들도 함수고 원래라면 서버측에서 설정해야하는 것들을 데코레이터들이 미리 가지고 있으므로 이를 사용하면 view함수의 내용을 간단하게 하면서도 틀렸을 경우 에러내용를 보낸다든지 하는 것들을 자동으로 해준다.

- allowed http method(`from django.views.decorators.http import `)
  - `@require_http_method(['GET','POST'])`: 내부에 저장된 리스트에 있는 값으로 들어온 method가 아니면 데코레이터가 붙어있는 함수가 동작하지 않는다
  - `@require_GET` : method가 GET이 아니면 작동 안함
  - `@require_POST` : method가 POST가 아니면 작동 안함
  - `@require_safe` : method가 GET이거나 HEAD로 온 경우만 받음. 이런 요청방식은 대체로 권한을 많이 안 주는 method들이기 때문에 안전하다고 생각해서 이런 이름이 붙음
  - 실제로 작동하는 지 시험해보기 위해서는 get과 post외에도 다른 method로 보내볼 필요가 있는데 브라우저로는 이런 기능을 할 수 없기 때문에, postman과 같은 api를 이용하여 시험해볼 수 있다.