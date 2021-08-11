# os kernel and db model

## infra

- os
- application
- internet
- 전기(한전)
- idc
  - os
  - 등등

## os kernel

- 하드웨어와 어플리케이션 사이의 중재역할
- 종류
  - 모놀리식 커널(단일형)
    - 한 개의 커널에 모든 기능을 가지고 다 처리하는 방법
    - 확장성이 좋지 않다
    - unix, linux, ms-dos, window 9번대 계열
  - 마이크로 커널
    - 커널에 핵심기능만 남기고 나머지기능은 다른 곳에 두는 방식
    - 리얼타임성 시스템에 감함
    - 통신은 메세지 전달에 위해서만 발생해서 전반적인 퍼포먼스는 저하
    - 확장 용이
    - minix
  - 하이브리드 커널
    - 중요한 ipc는 커널에 두고 파일시스템은 유저모드에 디자인
    - window nt, mac

## db model

- 의미 있게 구조화하여 저장된 정보
- model
  - relational
    - atomicity, consistency, isolation, durability
    - normalizaion
    - scalability(only scale up)
    - ansi sql문법, join 기능
    - mysql, oracle, postgresql, sql server
  - nosql
    - auto balancing
    - integrated caching
    - lack of schema
    - 종류
      - document store
        - 검색에 용이
        - json이나 xml로 저장
        - mongoDb 등등
      - key-value store
        - key와 value로 구성
        - 접근 속도가 빠름
        - redis 등등
      - wide column store
        - row별로 저장하고 각각 column을 따로 설정할 수 있게 만든 저장소
        - 큰 데이터를 저장할 때 좋은 db
        - cassandra 등등
      - graph database
        - neo4j
        - 그래프를 노드로 저장, 관계를 노드간의 연결로 표현
  - newsql
    - partitioning/sharding
    - concurrency control
    - replication
    - crash recovery
    - nuoDB, cockroachDB
- 결국 내게 필요한 요구사항을 잘 이해하고 선택해야한다.

