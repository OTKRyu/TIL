# aiot

사물인터넷인 iot에 ai를 추가

asic 기반의 ai computing

기본적으로 training은 서버에서 하고 만들어진 가중치들을 디바이스에 실어서 inference만을 디바이스에서 실행했으나, tpu(npu)가 나오면서 training까지 디바이스에서 하기 시작했다.

edge computing이란 중앙화된 서버에 의존하는 것이 아닌 말단에 해당하는 edge device에서 여러가지 연산이 처리되는 것을 말한다.

edge device inferencing의 장점

- real-time에 가까운 빠른 응답 처리
- 상대적으로 높은 보안을 보장
- cloud gpu 서버 비용과 시간 감소
- 고도로 지능화된 제품 개발이 용이

임베디드 cpu 기반 ai 영상처리 단점

- 저사양 cpu로 저속의 object recognition을 수행
- 느린 속도로 인해 실시간 처리 불가능
- cpu부하로 자율주행에도 영향을 줄 수 있음

google coral usb acc

- tpu를 작게 만들어서 개발한 장치이다.
- 모든 모델을 다 지원하는 건 아니고, 지원되는 모델에 한해서는 좋은 성능을 보인다.

open edge computing products

- ndivia jetson nano
- google coral(asic)
- intel 

closed edge computing products

- tesla
  - ai day, 자동차 회사 뿐만 아니라 ai 회사다
  - d1 chip

