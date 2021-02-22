# algorithm

## stack

- 물건 쌓듯이 자료를 쌓은 자료구조
- 선형 구조
- 자료 삽입 및 자료 출력이 가능하다.
- LIFO(last in first out)

### stack을 구현하기 위한 자료구조와 연산

- 자료구조 : 자료를 선형으로 저장할 수 있는 저장소
  - 저장소 자체를 스택으라고 부리기도 함
  - 마지막 삽입된 원소의 위치를 top이라고 함
- 연산
  - 삽입 : 저장소에 자료를 저장(push)
  - 삭제 : 저장소에서 자료를 삽입순서의 역순으로 삭제하며 반환한다.(pop)
    - 없을 경우도 있기 때문에 이를 잘 생각해서 짜야한다.
  - 스택이 공백인지 아닌지를 확인하는 연산 : isEmpty
  - 스택의 top에 있는 원소를 반환만하는 연산 : peek
    - 없을 경우도 있기 때문에 이를 잘 생각해서 짜야한다.

#### 구현 시 고려 사항

- 1차원 배열을 사용하면 구현이 용이하지만 크기를 변경하기 어렵다는 단점이 있다.
- 동적 연결을 이용하여 이를 해결할 수 있다.

### 재귀호출

자기 자신을 호출하여 순환 수행되는 것

상황에 따라 일반 호출방식보다 간단하게 작성할 수 있다.

이 때 같은 값을 구하기 위해 여러번 호출을 해야하는 비효율성 때문에 memoization이 나왔다.

단순히 재귀로 인해 호출이 많이 되는 값들을 미리 저장을 해놓고 그때그때  뽑아서 쓰는 것이다.

memoization은 dp(dynamic programming)의 핵심이 되는 기술이다.

memorization과 비슷하게 생겼으나 구분해야한다.


### 응용

- 괄호 검사

- function call
