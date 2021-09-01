# ai란

- 감지하고 있는 것에 반응하여, 환경을 인지하고, 생각하며, 배우고, 행동을 취할 수 있는 컴퓨터 시스템
  - automated intelligence: 수동적으로 진행한 업무 또는 인지 등의 자동화
  - assisted intelligence: 업무를 수행하는 것을 돕는 역할
  - augmented intelligence: 의사결정을 도와주는 역할
  - autonomous intelligence: 인간의 개입이 없이 판단을 하는 수준
- 학습과정에 필요한 것
  - 학습 자료
    - 정제되지 않은 데이터를 정제해 학습 데이터로 가공
      - 무의미한 데이터 제거
      - 학습을 위한 형태로 변경
      - 학습을 위한 labeling
  - 학습 방법
    - 교재에 맞는 교육법, 교육법에 맞는 교재
      - 영상 인식, 분류 방법
      - sentence 분류 방법
      - 시계열 예측 방법
  - 자동화/관리
    - 스스로 학습할 수 있도록 하는 것
      - 학습 데이터 수동/자동 새성
      - 학습 스케쥴링에 의한 자동 학습
      - 학습 모델 생애주기 관리
- 인공지능에 대한 기대
  - 시간 효율성
  - 비용 효율성
  - 객관성 유지
  - 진화
- 가르치는 관점
  - 학습하는 과정을 이해
  - 적은 양의 교재, 짧은 학습 시간
  - 학습 효율을 높일 수 있는 효과적인 방법
  - 학습법을 다른 영역에 재활용 또는 효과적으로 활용할 수 있는 영역
  - 개입을 최소화
  - 학습법을 개발하기 쉬운 틀
  - 스스로 학습하여 발전할 수 있는 방법

# convolutional neural network

- convoulution filter
- pooling
- relu
- fully connected classfication
- softmax
- 적용 예
  - 서류 분류 작업
  - 기재 및 체크 여부 판단

# bidirectional encoder representations from transformer(bert)

- 구조
  - pre-tranining
    - masked language model(mlm): 지워져있는 내용에 대한 정답을 찾는 문제
    - next sentence classification(nsp): 두 개의 문장이 연속된 문장인지 판별하는 문제
  - fine tuning
    - sentence pair classification task
    - single sentence classification task
    - question answering task
    - single sentence tagging task
- bert 모델 architecture
  - multi-layer biirectional transformer encoder
  - embedding
    - token: 단어 자체의 의미
    - segment: 문장의 앞 뒤를 구분
    - position: 토큰의 위치 정보