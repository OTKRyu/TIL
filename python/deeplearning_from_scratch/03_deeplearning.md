# 7 합성곱 신경망(cnn)

## 7.1 전체 구조

신경망 구조를 짤 때 학습 층과 출력 층을 어떻게 구성할 지 고민하게 되는데 출력 층은 대체로 affine-softmax로 고정이지만, 다른 학습 층에서는 구조를 affine-relu 혹은 다른 구조로 얼마든지 변경할 수 있다. 그 변경 방법 중 대표적인 것이 합성곱 신경망이다.

완전연결 계층 = affine 계층 : 층의 모든 노드가 다음 층의 모든 노드에 연결되어 전달되는 형식의 계층을 이른다.

## 7.2 합성곱 계층

완전연결 계층의 문제점 

- 데이터의 형상이 무시된다는 점

함성곱 계층

- 입력시의 형상과 출력시의 형상을 유지
- 필터 연산을 수행한다. 이 때 필터 연산이란 전체보다 작은 필터 행렬을 만들어 그 안에 가중치를 두고 이동해가며 필터 행렬의 크기만큼의 누적 가중치를 만들어 내는 것을 말한다. 자리별 곱셈으로 구현하면 단일 곱셈누산이 된다.
- 완전 연결 신경망에서 가중치에 해당하던 행렬은 cnn에서는 필터가 담당하게 되고 cnn에도 편향은 여전히 존재한다.
- 패딩은 계산을 용이하게 하기위해 본래의 데이터를 결과에 영향이 없는 선에서 변형시키는 것을 말한다. 주로 다음 층에 보낼 출력의 형상을 유지하기 위해 쓰인다.
- 스트라이드는 필터를 적용하는 간격을 얘기한다.
- 이 두 가지를 적용해서 2차원 행렬을 처리할 시에 나오는 결과의 형상을 구하는 식은 다음과 같다, 이 때 결과의 형상이 정수로 떨어지게 설계해야된다는 점을 주의하자
  - oh = ((h+2p-fh)/s) + 1
  - ow=((w+2p-fw)/s) + 1
  - 결과의 형상 (oh,ow), 입력의 형상(fh,fw), 패딩 p, 스트라이드 s
- 3차원으로 높여서 필터도 3차원으로 할 경우도 생각할 수 있으나 결과를 2차원으로 얻고 싶다면 차원은 같은 수준으로 맞춰줘야한다.
- 실제로 처리할 때는 필터가 여러개 적용되어 여러개의 결과가 나와 3차원데이터가 될 수도 있다.
- 3차원 학습도 배치처리를 통해 구현할 수 있다.

# 7.3 풀링 계층

행렬의 크기를 줄이는 계층으로서 대표적으로 최대 풀링이 있다. 풀링용 필터의 영역만큼에서 최대값을 뽑아내어 다음으로 이동시키는 것을 말한다.

풀링은 모든 값에 대한 연산을 진행하면서도 크기를 줄이는 것이 목적이기 때문에 중복을 줄이기 위해 스트라이드는 필터의 크기만큼씩 주어지는 것이 보통이다.

- 풀링 계층의 특징
  - 일방적으로 줄이는 계층이기 때문에 학습이 일어나지 않는다
  - z축은 그대로 유지된다.
  - 입력의 변화 중 대부분을 무시하거나 줄여서 받기 때문에 영향을 적게 받는다.

## 7.4 구현

데이터가 3차원이고 이런 데이터를 동시에 배치로 학습할 예정이니 필요한 것은 4차원 데이터이다.

이를 for문으로 돌면서 계산하는 것은 numpy의 특성상 바람직하지 않다, 이를 해결하기 위해 im2col이라는 함수를 이용한다.

im2col은 3차원 데이터를 2차원으로 변환시켜주는 함수로 이 함수를 쓰면 이미지를 한 줄로 변환시켜준다.

이를 사용하면 행렬연산때보다는 조금 더 복잡한 연산이 필요해 지지만, 기본적으로 컴퓨터는 복잡한 연산을 더 잘하는 편이기 때문에 효율이 낮아지는 것을 방지할 수 있고 심지어는 효율을 높일 수도 있다.

역전파를 할 때는 col2im을 사용하면 구현할 수 있으며, 상세 코드나 세세한 사용법은 책을 참조

## 7.5 전체 구조 구현

코드 위주이므로 책 참조

## 7.6 cnn 시각화

저층일수록 선이나 덩어리등 간단한 것들을 인식하고 고층으로 갈 수록 점점 추상적인 이미지를 뽑아내기 시작한다.

## 7.7 대표적인 cnn

대표적으로 두 가지가 있다.

- lenet
  - 활성화 함수로 시그모이드 함수 채택
  - 서브샘플링 하여 중간 데이터의 크기가 작아짐
- alexnet
  - 활성화 함수로 relu 함수 채택
  - 국소 정규화 계층 채용
  - 드롭아웃 사용

# 8 딥 러닝

왜 깊은 층을 가지는 것이 의미가 있는 지에 대한 설명과 딥 러닝을 이용해 구현할 수 있는 다른 주제들 및 남겨진 과제에 대한 내용들이 있다.

다만 정리하는 것이 의미를 가질만큼 의미가 있는 내용이라고 생각하진 않아서 따로 정리하진 않는다.