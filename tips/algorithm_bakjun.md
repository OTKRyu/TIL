# 주의점

## 시간 초과

- 공통

- python
  - 여러가지 편의를 위해 다채로운 연산이 가능하지만, 그 연산들이 순식간에 이루어지는 것은 아니다.
  - 대부분의 연산들이 파이썬의 기능이 없다고 생각했을 때 이를 구현했을 때와 같은 정도의 시간이 걸리게 된다.
  - 이런 시간 복잡도를 제대로 알아야만 파이썬으로 시간 초과를 내지 않으면서 코드를 짤 수 있다.
  - (len(a)= O(1)이 의외다.)
- c++
  - 축약 연산과 아닌 것의 시간복잡도가 다르다
  - endl은 느리기 때문에 \n를 사용하는 편이 좋다.
- java
  - 문자열을 더할 때 O(n+k)가 걸리기 때문에 StringBuilder를 사용해야한다.
  - sort가 퀵 소트이기 때문에 정렬하기 전 한번 셔플해주는게 좋다.

## runtime error

- 공통

  - 리스트를 만들 때 필요한 만큼 만드는 경우 인덱스 에러가 나는 경우가 많다.
  - 이를 해결하기 위해 리스트를 좀 더 크게 만들어놓거나, 예외 처리를 해야한다.
  - 경계값을 항상 넣어봐라
  - 전제가 거짓이면 그 명제는 항상 참이다.
  - 가장 큰 값과 가장 작은 값을 항상 확인해보자
  - 컴파일러의 워닝을 항상 주의깊게 봐라

- python
  - 리스트로 만든 리스트에 max를 쓰면 사전순으로 가장 뒤의 list를 찾게 됨으로 사용자가 원하는 이중 리스트의 최댓값과는 다른 결과가 나온다.
  - is 와 == 은 내부 작동 원리가 다르다. 고로 혼용해선 안 된다.
- c++
  - 변수의 저장에 까다롭기 떄문에 항상 초기화에 주의해야한다.
  - printf로 NULL값을 출력하면 틀릴 수 있다.
  - return값이 있는 지 확인
- java
  - 자료형에 담을 수 있는 정수인지 확인

## overflow

- 타입을 신경써서 해야하는 언어일 경우 연산에 의해서 그 범위를 벗어나는지 아닌지를 잘 생각해줘야한다.

## dp

- 공통
  - 마지막의 한 스텝이 이전과 비교해 어떻게 진행되는 지를 살핀다.
  - memoization을 이용해 될 수 있는 한 빠른 속도로 풀게 만든다.
  - memoization을 할 때 한 연산을 다시 하지 않도록 저장을 해야한다.
  - 연속합과 같은 경우에는 앞에서 뒤로 나아가면서 최대치를 찾아가게 만드는 dp문제와 같다.

## copy

- python
  - 얕은 복사가 기본값
- java
  - 얕은 복사가 기본값
- c++
  - 깊은 복사가 기본값

## 공통

- 실수 되도록 쓰지 마라
- 테스트 케이스를 반대로 쓰는 것도 때로는 좋은 테스트케이스를 발견하는 방법일 수 있다.
- 인덱스에 넣기 전에 범위 검사부터 해라
- 변수명이나 인덱스 순서 등 일관성을 가지고 작성해라.

# 문제 유형

## simulation

- 브루트 포스
  - 소풍
    - 빼는 연산을 할 때마다 목표로 하는 인덱스가 어떻게 변하는 지를 추적해 목표로 하는 인덱스가 제거되는 순간을 구하는 방법을 쓴다.
    - 왜 이렇게 하냐면 원래대로 그냥 주구장창 돌려가면서 실제로 언제 빠지는 지를 계산하면 nk짜리 연산이 되는데, 저런식으로 하나만 추적하면서 돌리면 n짜리 문제가 되기 때문이다.
    - 이 때 돌릴 때마다 어떤 상황이 변하는 지를 n,k,m,start란 변수로 추적한다. 여기서 start는 지워진 사람의 자리로 여기서부터 움직인다.
    - 혹은 제거된 사람 다음 사람의 번호를 1로 고정해서 돌려가면서 하는 방법도 있다. 이렇게 하면 항상 제거되는 사람의 번호는 고정이기 때문에 진행이 쉬워진다. 다만 목표로 하는 사람의 번호가 달라지므로 이를 추적해주면 위의 방법과 크게 다르지 않다.
  - 골드바흐 파티션 문제
    - 소수를 구하는 방법(에라토스테네스의 체 혹은 빠르게 구하는 법)을 알면 구현할 수 있다.
- bfs
  - 간선의 가중치가 동일할 경우에 최단 거리를 구할 때
  - 정점과 간선의 개수가 시간 내에 찾을 만큼 적은 경우
  - 숨바꼭질 5
    - 극단적 경우인 시작지점이 같을 경우를 고려하자
    - 한번 나온점들은 2회후에 반드시 재방문이 가능함으로 이를 미리 넣어주고 계산을 생략한다면 시간을 아낄 수 있다.
    - 이를 이용해 홀수와 짝수일 때 갈 수 있는 가장 빠른 시간을 알 수가 있다.
    - 이를 구해놓고 동생의 움직임을 따라가면서 되는지를 확인해 최소시간을 출력한다.