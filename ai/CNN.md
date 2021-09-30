# CNN

이미지의 분류에 쓰이는 학습방법이다.

extract feature - classification의 두 단계로 이루어져있다.

## extract feature

- convolution layer
  - 더 작은 필터를 만들어 각 지역마다의 값들을 만들어낸다.
  - padding
  - 활성화함수
- pooling layer
  - 데이터의 크기를 줄이는 역할과 동시에 불필요한 특징을 제거하는 역할
- flatten layer
  - 데이터의 형태를 계산하기 편한 1차원형태로 변환하는 역할

## classification

- extract feature에서 나온 데이터를 기반으로 어떤 class에 넣을 것인지를 정하는 역할

