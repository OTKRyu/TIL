# db

## design

1. 목적
   - 정보 요구사항에 대한 정확한 이해
   - 원활한 의사소통 수단
   - 데이터 중심의 분석 방법
   - 신규 시스템 개발의 기초 제공
2. 요구사항 분석
   - 요구사항을 수집 분석하여 명세서 작성
3. 개념적 설계
   - 명세서에서 데이터베이스를 구성하는데 필요한 개체, 속성, 관계를 추출해 erd 생성
     1. 개체와 속성을 추출
        - 개체와 속성 모두 명사지만, 구별하여 추출한다.
     2. 관계를 추출
        - 관계는 여러가지로 분류해서 정의 된다.
     3. erd 생성
4. 논리적 설계
   - 개체는 테이블로 변환, 속성은 테이블의 속성
   - n대m은 테이블로 변환
   - 1대 n은 외래키로 표현
   - 1대 1도 외래키로 표현
   - 다중값 속서은 독립 릴레이션으로
5. 물리적 스키마 및 구현
   - erd를 실제 테이블로 생성

## 반정규화

1. 반정규화란
   1. 성능향상, 개발과 운영의 단수화를 위해 모델을 통합하는 프로세스
      - 정규화 : 데이터 타입을 반드시 한 개씩만 존재하도록 하여 데이터가 변질되지 않도록 하는것, 테이블이 많아지므로 sql 작성이 어렵고 테이블 조인때문에 성능저하
      - 반정규화 : 정규화의 반대로 데이터의 중복을 어느정도 허용하여, 개발 속도 및 성능 향상
   2. 대표적 반 정규화
      - 컬럼 반정규화
        - 중복컬럼 추가
        - 파생 컬럼 추가
        - pk에 의한 컬럼 추가
        - 응용시스템 오작동을 위한 컬럼 추가
      - 관계 반정규화
        - 중복 관계 추가로 경로 단축
   3. 요약
      - 정규화 반정규화 때에 따라 다르다. 