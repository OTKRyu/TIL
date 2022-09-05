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

## pjt 8

### cbv(class based view)

함수 기반이 아닌 클래스를 기반으로 작동하는 view를 작성하는 방법

내가 직접 짤 필요없이 장고측에서 이미 만들어준게 많기 때문에 이를 그냥 끌어와서 사용

미리 짜놓은 클래스들은 `django.views.generic`이란 곳에 있으며 여기서 필요한 클래스를 끌어와 상속시켜 내 클래스를 만들면 된다.

이를 구현하는 자세한 방식은 공식 문서를 참조

model의 get_absolute_url이라는 매서드는 model과 관련해서 수정이나 생성이 되었을 때 어디로 리다이렉트시킬지 정해주는 매서드이다.

유저의 로그인을 구현할 거라면 이 또한 미리 구현되어 있는게 많으니 문서를 참조하길 바란다.

tests.py에 관한 설명

내가 짠 코드가 제대로 동작을 하는지 외부에서 시험해볼 수 있는 파이썬 파일이다.

`python manage.py test` 로 동작시킬 수 있다. 이 때 앱마다의 tests.py의 함수들 중에 test_로 시작하는 매서드들을 실행시키면서 그 결과를 가져와 준다.

test.py를 짜는 방법은 class를 만들고 TestCase라는 클래스를 상속 받는다.

그 후 setUp이라는 매서드로 이 후 테스트에 필요한 공통조건을 불러오는 역할을 한다.

이후 다음에 test_ 로 시작하는 함수들을 짜 넣으면 검사를 할 수 있다.

Client()라는 클래스는 가상유저를 만드는 방법으로 이 클래스에서의 행동을 규정함으로서 특정 상황을 발생시킬 수 있다.

이 떄 정답을 정해놓고 assertEqual이라는 인스턴스 매서드를 이용해 특정상황에서 발생시킨 것과 내가 정한 정답이 맞는지를 비교하여 같다면 진행을, 아니라면 에러를 발생시킬 수 있다.

assertTemplateUsed 라는 매서드를 쓰면 실제로 쓰인 템플릿이 내가 정한 정답과 맞는지를 확인할 수도 있다.

assertContain 은 내가 준 html 문서에 실제로 이게 들어가 있는지를 확인해준다.

assertNotEqual은 말그대로 두 개의 요소를 받아 두 개가 다를 때에만 진행하고 아니면 에러를 낸다.

TDD에 관한 설명

test-driven development의 약자로 규모가 커질수록 실제로 검증해가면서 디버그를 하는게 쉽지 않으므로, 전체적으로 내 프로젝트가 잘 만들어졌는지 검증할 수 있는 test를 먼저 만들고, 실제 개발을 하면서 이 test를 충족시킬 수 있는지를 검증해나가는 방식을 말한다.

## pjt09

- query

  - db optimizaion에 관한 내용(1+n queryset problem)
  - 이를 좀 더 편하게 해주는 것이 django debug toolbar다
  - 페이지마다 쿼리문을 얼마나 보내는지 시간은 얼마나 걸렸는지 등을 시각화시켜준다.
  - 사용법은 공식문서 참조
  - sql join은 연결관계가 있는 테이블을 연결키 기준으로 합쳐서 테이블을 만드는 기능이다.
  - django orm에서 쓸 때는 select_related라는 명령어로 사용한다. 이 때 정참조일 때만 사용할 수 있다. sql에서 inner join 사용
  - m대m 정참조, fk 역참조일 때는 prefetch_related를 사용하여 sql join을 활용한다. 파이썬 내부의 join활용. `prefetch_related(Prefetch('comment_set',queryset=Comment.objects.select_related('user'))` 과 같이 사용할 수 있다.

- infinity scroll

  - 일단 한번의 로딩에 몇 개의 자료를 불러올 지 나눠놔야하므로 pegination을 해줘야한다.

  - ` from django.core.peginator import Paginator`

  - `paginator = Paginator(queryset, query num for each page)`

  - `movies = paginator.get_page(page_numbe)`

  - 그리고 ajax를 활용할 것이기 때문에 html을 보내는게 아니라 json데이터를 보내야한다.

  - 고로 이걸 직렬화해서 json으로 보내준다.

  - 그리고 이러면  html과 json 둘 중 하나만 보낼 수 있기 때문에 이 둘을 분기하기 위해서 `request.is_ajax()`를 활용해준다.

  - javascript에서 스크롤이 화면의 맨 아래까지 내려가면 요청을 보내게 만든다.

  - ```javascript
    document.addEventListener('scroll', (event) =>{
    // 아래의 documentElement는 document의 전반적인 정보를 가지고 있는 곳이다.
    // 이 안의 scrollHeight는 전체 html문서 중 어느정도에 위치했는지를 보여준다.
    // 내가 지금 보고 있는 화면에서의 높이가 clientHeight이다.
        const {scrollTop, clientHeight, scrollHeight} = document.documentElement
        let pageNum = 2
        if (scrollHeight-scrollTop === clientHeight){
            requestData = {
                method: 'get',
                url : `http://localhost:8000/movies/?page=${pageNum}`,
    			headers : {'X-Requested-With':'XMLHttpRequest'}
            }
            axios.get(requestData)
            	.then((res) =>{
                res.data.forEach((movie) => {
                    const movieDiv = document.createElement('div')
                    const movieList = document.querySelector('#movie_list')
                    const movieData = `<h1>${movie.fields.title}</h1><p>${movie.fields.overview}</p>`
                    movieDiv.innerHtml = movieData
                    movieList.append(movieDiv)
                    pageNum++
                })
            })
        }
    })
    ```
    
  - vue.js version
  
  - ```javascript
    const app = new Vue({
        el:'#movie-list',
        data: {
            movies: [],
            url: 'url',
            pageNum:1,
        },
        methods: {
            getMovies() {
                axios.get(this.url)
                .then((res) => {
                    this.movies.push(...res.data)
    				this.pageNum++
                })
            },
            checkBottom() {
                const {scrollTop, clientHeight, scrollHeight} = document.documentElement
        	if (scrollHeight-scrollTop === clientHeight){
                this.getMovies()
            }
        },
        // vue instance의 생애주기 중 탄생에 해당했을 때 실행할 일
        create: function () {
            this.getMovies()
            window.addEventListener('scroll', this.checkBottom)
    	}
    })
    ```
  
  - {{}}로 보간법을 하는게 장고와 vue가 같기 때문에 html과 json을 모두 보내느 방식으로 짜면 장고가 이를 실행해버릴 우려가 있다. 주의해야한다.
  
  - vue instance의 delimeter라는 항목을 조작해 보간법에 해당하는 것을 변경할 수도 있으나 추천하진 않는다.