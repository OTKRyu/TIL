# 5 오차 역전파법

- 순전파: 계산식을 구성하는 순서대로 받은 값들로부터 출발해 계산결과값으로 나아가는 과정
- 역전파: 계산식을 구성하는 순서의 역순으로, 계산결과값에서 시작해 받았을 값으로 나아가는 과정

- 연쇄법칙: 미적분학 참조

## 실제 역전파

각종 기본 노드들에 대한 연산

- 덧셈: 그대로
- 곱셈: x,y의 곱셈인 경우 서로 반대인 y,x를 곱해서 전파

실제 구현 책 참조

- relu : 0인 부분에서는 후반에 경향끼치는 것이 없음, 그 외일때는 x이므로 미분값 1만큼 영향을 끼침 고로 0일때는 0을 곱해서, x일 때는 1만큼 곱해서 역전파
- sigmoid : 시그모이드는 앞에서부터 -1 곱하기, 지수함수, 더하기 1, 역수 취하기 순으로 합성된 함수로 역으로 이를 따라서 나가면서 역전파를 일으키면 된다. 시그모이드 함수의 역전파를 이 아래의 조합으로 구현해도 되고 그냥 통으로 구현해도 된다.
  - 역수 취하기는 -y^2배를 해서 역전파
  - 덧셈은 앞에서 봤듯이 그대로 역전파
  - 지수함수는 지수함수는 미분해도 자기자신과 미분값으로 이루어지므로 exp(-x)를 추가로 곱해서 역전파
  - -1 곱하기는 곱셈이므로 -1를 곱해서 역전파

## affine/softmax계층 구현

### affine 계층

아핀 계층은 순전파 때 수행하는 행렬의 내적을 기하학에서 어파인 변환이라고 부르기 때문에 이렇게 이름 붙였다.

위에서는 단일 노드를 대상으로 했지만 실제 기계학습은 행렬단위로 일어나게 된다.

이것을 행렬로 나타내면 XdotW+B = Y가 되는 식인데 dot은 dot product고 B는 편향 행렬, X는 입력값행렬, W는 가중치행렬이다.

이제 이 행렬식을 역전파 시켜야한다. 이를 앞에서 한것처럼 일일히 증명할 수 있으나 이 책에서는 생략했다.

행렬이라고 하더라도 덧셈의 경우 행렬의 모양의 변화도 없고 그대로 더해지므로 오차의 편미분도 그대로 전파가 되고 편향에도 똑같이 전파된다. 다만 dot product의 경우 단일 노드의 경우와 좀 다르게 가중치 행렬의 전치행렬만큼 곱해서 입력값행렬쪽으로, 입력값행렬의 전치행렬만큼 곱해서 가중치행렬쪽으로 역전파하게 된다.

### softmax 계층 구현

구현이 조금 복잡하니 책 참조

# 6 학습 관련 기술

## 6.1 매개변수 갱신

확률적 경사 하강법 SGD의 한계를 극복하기 위해 제시된 새로운 optimizer 

- momentum
  - 물리에서의 그 모멘텀 맞다. 그를 수식으로 표현해 적용한 것이다.
- adagrad
  - 학습이 많이 된 상태일 수록 새롭게 학습을 많이 하는 것보다는 미세한 조정이 들어가야한다.
  - 이를 구현하려면 학습률이 학습정도에 따라 줄여들어야 한다.
  - 이를 구현할 때 일괄적으로 학습률을 낮출 수도 있지만, 매개변수마다 학습이 진행된 상황이 다르기 때문에 이를 고려하여 진행한다면 더 효과적으로 학습을 진행시킬 수 있을 것이다.
  - 이를 구현한 것이 adagrad이다.
- adam
  - 기본적으로 위의 두 방법을 합쳐서 구현하려고 만든 방법이다.
  - 이론적으로 어려움이 있어 깊게 배우지 않는다.

이 3가지 모델 모두 sgd의 한계점을 극복하기 위해 만든 방법이지만 대체로 모든 것들이 그렇듯 모든 것 위에 서는 한 가지 방법은 잘 존재하지 않는다. 다만 대부분의 경우에서 sgd보다는 위 세 가지 방법이 최적화가 빠른 편이긴 하다.

고로 자신이 당면한 문제가 어떤 종류의 것인지 보고 4가지 방법 중 적절한 것을 사용해 구현하는 것이 중요하다.

## 6.2 가중치의 초깃값

학습을 시작하기 전 가중치의 값을 정하는 것도 중요한 주제 중 하나이다.

실제로 가중치에 어떤 패턴이 있거나 하게 되면 학습이 일어나는 도중에도 그 영향에서 벗어나 원하는 대로의 결과가 나오기를 기대할 수 없기 때문이다. 그렇기에 대체로 작고, 정규적으로 분포한 무작위 값을 초깃값으로 많이 사용하는 편이다.

이 때 정규적으로 분포한 것을 표준편차로 통제하게 되는데 이를 너무 크게 하거나 너무 작게 하는 것도 학습이 제대로 진행되지 않게 하는 이유가 될 수 있다.

이 때 대표적인 것들로 한쪽으로 가중치들이 너무 양쪽 끝으로 분포하게 됨으로써 생기는 기울기 손실문제나, 아예 중앙으로 모여버리는 표현력 부족 문제같은 것들이 있다. 

이를 해결하기 위한 방안이 몇가지 연구되어 있다.

- 활성화 함수가 선형일 때
  - xavier 초깃값 : 앞 층의 노드 수가 n일 때, 1/sqrt(n)을 표준편차로 가지는 정규분포를 사용하는 방법이다.
- 활성화 함수가 ReLU함수일 떄
  - He 초깃값: 앞 층의 노드 수가 n일 때 , sqrt(2/n)짜리 표준편차를 가진 정규분포로 분포시키는 방법이다.

## 6.3 배치 정규화

배치 정규화란 각 층이 활성화를 적당히 퍼뜨리도록 강제하는 방법이다.

이를 통해서 얻을 수 있는 이득으로는,

- 학습 속도 개선
- 초깃값 의존성 제거
- 오버피팅 억제

등이 있다.

실제로 사용할 때는 affine계층과 활성화 계층 사이에 배치 정규화 계층을 배치해 활성화되기전에 값들을 정규화시키는 것이다.

이 때 데이터의 분포가 평균은 0, 분산은 1이 되도록 정규화 한다.

## 6.4 바른 학습을 위한 추가 주제

- 오버피팅 : 말 그대로 특정 데이터셋에만 지나치게 적응한 결과로 범용성이 떨어져 다른 데이터셋에서는 정답률이 낮아지는 현상
  - 가중치 감소: 말 그대로 가중치의 크기에 따라 지나치게 큰 가중치일 겨우 오히려 학습을 덜하도록 막는 방법이다. 람다라고 불리는 하이퍼 파라미터를 정의해 각 가중치마다 람다*(가중치^2)/2를 더해서 구현한다.
  - 드롭 아웃: 층이 여러개인 신경망의 경우 다음층으로 이동할 때 임의로 몇 가지 뉴런를 삭제해버리는 기법이다. 이를 활용하면 오버피팅을 방지하면서도 표현력을 높일 수 있다.

## 6.5 하이퍼파라미터

기계가 스스로 가중치를 최적화할 때 인간이 직접 정해줘야하는 수치들을 부르는 말로 이를 잘 설정하는 것이 기계학습을 성공시키는 주요 요소이기도 하다.

이 때 시험 데이터에 맞춰 하이퍼파라미터를 정하게 되면 오버피팅이 일어날 가능성이 있으므로, 시험 데이터를 사용하기 보다는 하이퍼파라미터 검증용 데이터를 따로 빼두는 편이 좋다.

하이퍼파라미터도 학습과 마찬가지로 처음부터 정답을 가지고 있지 않다. 그렇기 때문에 범위를 좁혀가면서 어떤 하이퍼파라미터가 좋은지를 알아내야 한다. 이를 하이퍼파라미터 최적화라고 부른다. 이 때 패턴을 가지고 탐색하는 것보단 무작위로 설정하는 편이 효과가 좋다. 대체로 0.001~1000정도 사이에서 찾는 편이다. 이 때 가중치 최적화보다도 오랜 기간을 소모할 가능성이 높기 때문에 나쁠 것 같은 값을 일찍감치 포기하는 것이 효율적이다. 따라서 배치학습을 시킬때 에폭을 작게 해 가능성을 자주 그리고 많이 쳐내는 편이 효율적이다.

이렇게 여러개의 하이퍼파라미터를 시험한 뒤에 가장 잘 움직이는 범위로 계속 줄여나가다가 최후에 학습시킬 모델에는 딱 하나의 하이퍼파라미터를 사용해서 구현하는 것이다.