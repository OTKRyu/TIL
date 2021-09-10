# 데이터베이스

- 정보를 모아놓은 곳
- 모아놓은 정보를 빠르게 찾을 수 있어야함

## 관계형 데이터베이스

- 데이터를 키, 밸류 형태로 식별이 가능하도록 함
- 테이블간 1:1,1:n,:n:m 등의 관계가 있음

## primary key

- 다른 항목과 중복되어 나타날 수 없는 단일 값
- 다중 컬럼을 pk로 설정할 수 있다
- 관계형을 완성하는데 가장 중요한 정보

### pk와 index

pk 생성, index 생성

pk 삭제, index 삭제

#### index?

- 테이블의 검색 속도를 향상시키기 위한 자료구조
  - 인덱스를 쓰는 이유이기도 하다.
  - 다만 인덱스를 많이 만들게 되면 성능이 저하될 수 있다.
- 인덱스 테이블이라고도 함, 테이블의 한 종류
- pk가 아닌 컬럼이라도 index를 생성할 수 있다.
- 인덱스의 동작
  - pointer
  - hash
  - b+ tree
- 인덱스를 활용하면 안 되는 경우
  - delete, update가 자주 일어나는 경우
- 인덱스의 장점
  - 테이블의 조회속도 및 성능 향상
  - 시스템의 부하를 줄일 수 있다.
  - 원하는 데이터를 빠르게 찾을 수 있다.
- 인덱스의 단점
  - db 저장 공간을 차지한다.(꽤나 큰 용량을 사용)
  - 인덱스를 관리하기 위한 작업이 필요하다
  - 잘못 사용할 경우 성능이 저하될 수 있다.
- 인덱스를 잘 쓰려면
  - where 조건 작성시 index를 적용할 수 있는 컬럼 조건을 상단에 작성한다.
  - index로 사용된 컬럼은 컬럼값 그대로 사용해야한다. 즉 조작해서 사용하려하면 안 된다.
  - between,like, <,> 등 범위 조건 뒤의 index들은 적용되지 않으니 가장 마지막에 활용한다.즉 인덱스가 걸린 조건을 가장 먼저 써야한다.

#### 데이터베이스 객체?

- 데이터베이스 내에 존재하는 논리적 저장 구조
- table, view, indexes, synonym, sequence 등등
- data define language(ddl)로 생성, 수정 및 삭제 가능

#### DDL?

- structured query language(sql)의 한 종류
- data manipulation language(dml)
- data define language(ddl)
- data control language(dcl)
- transaction control language(tcl)

### 쿼리 성능을 향상시키려면?

- table 생성시 pk, index등을 잘 구축한다.
- 같은 결과를 출력하더라도 최적화된 쿼리를 작성해야 한다.
- table join 순서도 중요하다. (driving, driven table)
- databae optimizer, query tuning, query plan