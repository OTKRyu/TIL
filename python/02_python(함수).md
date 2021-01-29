# python

## function 

특정 기능을 하는 코드의 묶음

단순히 로직을 담아놓은 container라고 생각해도 된다.

실제로도 그렇게 동작한다.(로직을 =으로 할당하는 것 등등이 가능)

- 높은 가독성

- 재사용성

- 유지보수

  ```python
  def func(arg):
      function_work1
      function_work2
      return (return_value)
  
  func(a)
  ```
### 입출력
- 기본적으로 단 하나의 객체만 반환 가능
- 아무것도 안 쓸시 `None` 값 반환
- 여러 개의 값 반환시 `tuple`로 자동 반환

#### 입력
- 인자 사용시 기본적으로 위치로 인자 판단
- 기본값 지정시 기본 판단 매커니즘이 바뀜, 기본값이 지정된 인자를 뒤로
- `*`를 이용해 받는 인자 수를 임의로 지정 가능
- `*`사용시 명시를 해야함 기존 인자와 구분이 안 되기 때문
- 여러 개의 값 입력시에도 `tuple`로 자동 반환
- `**`를 이용하면 키와 밸류의 쌍을 가변적으로 딕셔너리형태로 받아올 수 있음
- 일반적으로 `kwargs`라고 이름을 붙임
- 이렇게 지정해 입력하면 입력을 `dict`타입으로 받음

### 재귀 함수
함수의 정의 안에 자기 자신을 내부함수로 사용하는 함수
```python
def fact(n):
    if (조건):
        return (값)
    else:
        return (값) + fact(n-1)
```
다음과 같은 경우에 사용한다.

- 재귀함수로 정의하는 게 자연스러운 경우
- 변수 사용을 적게하는게 필요한 경우

최대재귀깊이가 있어 제어를 벗어날 경우 자동으로 종료한다

### 정렬 

- `a.sort()`
  - 원본에 접근해 정렬후 저장, 반환값 없음
- `sorted()`
  - 원본은 그대로 둔 채 정렬된 개체를 반환x

### 지역, 전역 스코프 및 변수

변수들을 모아놓는 공간을 스코프라고 부르고 함수 내부에서만 쓰면 지역 그 밖에서도 전역이라고 구분한다.

#### 이름 검색 규칙(LEGB Rule)
위에서부터 아래로
- Local scope(함수내)
  - 함수가 호출될때 생성, 함수가 종료될 때까지 유지
- Enclosed scope(상위 함수내)
- Global scope(함수 밖의 변수, 혹은 import된 모듈)
  - 모듈이 호출된 시점, 혹은 이름 선언된 이후부터 인터프리터가 끝날 때까지
- Built-in scope(파이썬 내장)
  - 파이썬이 실행된 이후부터 계속 유지

### error and exception
- `SyntaxError` 문법 안 지킨경우
- `EOLError` 문장을 종결을 시키지 않은 경우
- `EOFError` 괄호 안 닫는 경우
- `ZeroDivisionError` 0으로 나누려 한 경우
- `NameError` 이름이 잘못된 경우
- `TypeError` 타입이 안 맞는 경우
- `ValueError` 넣어야하는 값이 잘못된 경우
- `IndexError` 없는 인덱스를 조회한 경우
- `KeyError` 없는 키를 조회한 경우
- `ModuleNotFoundError` 없는 모듈을 찾은 경우
- `ImportError` 모듈이 있으나 가져오는 과정이 잘못된 경우
- `KeyboardInterrupt` 동작 중에 키보드로 인한 방해명령이 들어온 경우

#### exception handling
`try` and `except` method
- 'try'문 안의 내용을 일단 실행하고 문제가 생기면 그에 해당하는 'except'문으로 이동
-'as'명령어를 이용해 어떤 오류인지를 변수로 받을 수 있다.
- 'else' 명령어는 모든 except가 에러없이 잘 지나갔을 경우 수행
- 'finally' 에러 유무와 상관없이 'try'문을 떠날때 꼭 해야하는 것을 써 놓는다
'raise' 명령어를 통해 강제로 에러를 만들수 있다.
'assert'문을 이용해 상태를 검증할 수 있다.
