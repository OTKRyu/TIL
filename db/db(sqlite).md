# db

## sqlite

### 설치법

1. sqlite 사이트 접속
2. 자기 환경에 맞는 sqlite와 sqlite tools 다운로드
3. c:\sqlite를 생성후 그 안에 sqlite와 sqlite tools의 내용물 저장
4. 환경 변수 설정에 c:\sqlite 추가

### 기본 설정

- PRIMARY KEY 속성을 넣어주지 않을 채로 테이블을 만들게 되면 SQLite에서 알아서 rowid라는 컬럼을 추가적으로 만들어준다.
- 이 rowid는 pk가 끊어지지 않게 삭제로 날아간 부분을 재활용해서 채워주는 편이다.
- 이런 속성 때문에 될 수 있으면 sqlite만 쓸 떄는 프라이머리 키를 직접 설정하기 보다는 rowid를 쓰기를 권장한다.
- 다만 장고에서 sqlite를 끌어올 때는 삭제된 데이터도 의미가 있다고 생각하기 때문에 django에서는 rowdi를 쓰지 않고 직접 만들어준다.

### 사용법

* db 생성

  * sqlite3 filename.sqlite3

* db 주석

  * -- 주석 내용 --  : 이런식으로 쓰면 주석처럼 쓸 수 있다.

* .으로 시작하는 명령어 : sql문이 아닌 sqlite 고유의 기능으로 sql문법을 지킬 필요가 없다.

  * .mode csv  : csv를 읽을 수 있게 모드를 바꾸는 명령어
  * .mode columns : 컬럼 모드로 바꾸는 것으로 출력할 때 표처럼 꾸며서 보여준다.
  * .import db.csv tablename : table을 tablename이란 이름으로 생성하면서 db.csv로 그 내용을 채운다. 이 때 데이터타입을 지정하지 않은 채로 넣게되면 일단 기본값인 text로 넣게 된다. db는 뭔가 문제가 생겨도 에러메세지를 띄워주긴 하지만 진행은 한다. 다른 언어처럼 멈춰버리지 않는다.
  * .headers on : 헤더라는 옵션을 킨다는 뜻으로, 이를 키면 어떤 column의 어떤 값인지를 같이 보여준다.
  * .read path : path위치에 해당하는 파일을 가져와서 읽으라는 것인데 이게 sql문(.sql확장자)이라면 이를 실행해준다.
  * .tables : 현재 테이블의 이름을 알려준다.
  * .schema tablename: tablename에 해당하는 schema를 보여준다.

* sql(사실 FROM뒤에 오는게 tablename일 필요는 없다. dataset이기만 하면 된다. 고로 여러번 걸러서 탐색을 하는것도 가능하다.)

  * ```sqlite
    CREATE TABLE tablename(
    	id INTEGER PRIMARY KEY AUTOINCREMENT, --이 경우 id를 pk로 선정하고 데이터가 들어올 때마다 자동으로 1씩 올려준다. --
    	name TEXT NOT NULL --sql은 이 줄이 끝났는지 아닌지를 ,로 판단하기 때문에 마지막에 쉼표를 꼭 생략해야한다.--
    );
    ```

    이렇게 쓰면 table을 만들 수 있으며 여기서 대문자로 쓴 것들은 sql내부에서 정해져있는 것들이고 소문자가 있는 것들은 내가 정할 수 있는 것이다. 사실 이것도 그냥 쓸 수 있긴 한데 convention이다.

    * 여기서 옵션으로 PRIMARY KEY AUTOINCREMENT나 NOT NULL등 컬럼의 속성을 줄 수 있다.
    * 만약 column 이름만 쓰고 속성이나 이런 것들을 안 주면 기본값으로 들어가게 되는데 그 경우 TEXT, NULL로 들어가게 된다.

  * `DROP TABLE tablename;` : tablename을 삭제한다.

  * `INSERT INTO tablename (columnnames,) VALUES (values,)` : values에 해당하는 데이터를 tablename에 넣는다. 이 때 columnnames에 해당하는 부분을 쓰지 않고 하면 tablename에 해당하는 모든 인자를 받는다고 생각하고 그냥 순서대로 넣어버린다. (values,) 에 해당하는 부분을 여러번 쓰면 한번에 여러개를 넣을 수도 있다.

  * `SELECT DISTINCT COUNT(columnnames,) FROM tablename LIMIT  num OFFSET num WHERE condition`;

    * tablename에 있는 모든 자료의 columnames에 해당하는 부분을 가져온다. *를 쓰면 다 가져온다. 
    * LIMIT을 붙이면 num개에 해당하는 자료만 가져온다. 순서는 저장이 빠른 순서부터다.
    * 이 떄 OFFSET을 쓰면 자료조회의 시작점을 지정해줄 수 있다. 기본값은 0이다.
    * SELECT 구문을 쓸 때 주는 옵션의 순서는 정해져있으므로, 순서대로 쳐야만 제대로 작동한다.
    * DISTINCT 구문을 준다면 중복을 제거하여 가져온다.
    * COUNT 구문을 추가해주면 해당하는 컬럼에 NULL값이 아닌 값의 갯수를 세준다.
    * ORDER BY column DESC : 이 구문을 추가하면 데이터들을 컬럼에 해당하는 기준에 맞춰서 정렬시킨뒤 내가 원하는 작업을 할 수 있다. 기본값은 ASC으로 오름차순이고, 내림차순으로 해주고 싶다면 DESC를 명시를 해줘야한다. 이 떄 column을 여러 개 써줄 수도 있는데 이 때 앞쪽에 온 순서대로 정렬하고, 그 후 같은 것들 중에서 뒤의 조건으로 정렬한다. 이 각각에 대해서도 ASC, DESC 조건을 줄 수도 있다.
    * GROUP By column1, column2 : column별로 그룹을 지어준다. column이 여러 개면 각 조건에 맞게 그룹을 나눠준다. 이렇게 그룹을 나눌 때 정렬순서대로 그룹을 만든다.(ex)가, 과, 강 , 나, 노 텍스트라면 이런 순으로 진행...), 그리고 이렇게 나눠준 후에 조회를 반복하기 때문에 그 그룹의 특성을 뽑아올 때 쓸 수 있다.
      * 추가로 AS 문을 사용하여 컬럼 명을 바꿔줄 수 있다. ex)COUNT(*) AS name_count
    * AVG, SUM, MIN, MAX등의 명령어도 함수처럼 쓸 수 있는데, 이는 INTEGER필드에서만 작동한다. 따라서 컬럼을 지정해줄때 컬럼을 쓸 수 있는 컬럼을 설정해야한다. 이런 함수는 columnname을 지정해줄 때만 쓸 수 있으므로 여기서 쓰면 WHERE에서 쓴 것과 같은 효과를 낼 수 있다.
    * WHERE구문을 추가해준다면, 조회할 때 이 조건에 맞는 데이터들만 가져온다. 이 때 여러 조건이 들어갈 수 있는데 이 조건들은 and나 or같은 결합자로 연결해줘서 더 어려운 조건으로 검색할 수 있다.
      * 특히나 WHERE 구문은 내부에서도 조건 식을 여러 모로 적을 수가 있기 때문에 쓸 것이 많은 편이다
      * LIKE 라는 명령어를 쓰면 찾고자하는 조건에 얼추 맞는 데이터들을 찾아올 수 있다. 이 때 와일드 카드를 써서 조건을 줄 수 있는데, 와일드 카드의 경우 %와 _ 두 가지가 있는데 _의 경우 자리에 어떤 값이 꼭 존재해야하고 %의 경우 있어도 없어도 상관이 없다. 

  * `DELETE FROM tablename WHERE condition` : tablename에 해당하는 테이블의 condition에 해당하는 얘들만 삭제. WHERE을 안 준다면 조건이 없으므로 전부 삭제가 된다.

  * `UPDATE tablename SET column1=value1, column2=value2 ... WHERE condition` : tablename에 해당하는 테이블의 condition에 해당하는 얘들의 데이터를 SET에 써져있는 걸로 변경 

  * 테이블 이름을 바꾼다든지 column을 더 추가한다든지 하는 명령어가 추가로 있으나 sqlite에서는 잘 쓰지 않는 편이기도 해서 필요하다면 자료를 찾아보길 바란다.