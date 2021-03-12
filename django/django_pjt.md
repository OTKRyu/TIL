# pjt 4

db 자체를 git으로 공유하지는 않지만, 데이터 자체를 json 형식으로 공유할 수 있다.(db자체를 공유하지 않는 것은 db에 저장할 때의 방식이 다를 가능성도 크고, 만약에 충돌이 나더라도 단순한 data를 바꾸는 것과 db안의 데이터를 바꾸는 것은 난이도의 차이가 크기 때문이다.)

이런 식으로 더미데이터를 공유하는 방식을 fixture라고 한다.(django fixture 참고)

이 때 `djanog-admin dumpdata appname.modelname ` 혹은 `python manage.py dumpdata appname.modelname`

`dumpdata`도 옵션이 몇 개 있는데 이 때 `--indent num`란 옵션은 몇칸을 들여쓰기한 데이터로 만들 것인지를 지정해준다. 없다면 그냥 붙여서 출력

이를 `>`를 이용해 원하는 형식의 파일에 저장하면 된다. 주로 json에 저장한다.

반대로 이를 받기 위해서는 `python manage.py loaddata path_to_file`를 하면 더미데이터를 db에 저장해 준다. 이 때 db에 저장할 때 pk를 기준으로 덮어씌워버리기 때문에 주의해야한다.

이 때 데이터가 db에 반영되려면 db에 맞는 자료여야하기 때문에 이를 확인하려면 db에 이미 그 구조가 있어야한다. 고로 db에 schema까지는 적용해놔야한다.

pair programming으로 진행

