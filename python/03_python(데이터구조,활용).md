# python

## str.method()
### 검색
- `.find(x)` : x값이 처음 등장하는 인덱스를 반환, 없으면 -1 반환
- `.index(x)` : x값이 처음 등장하는 인덱스를 반환, 없으면 오류

### 조작
- `.replace(old, new[, count])` : old값을 찾아 new로 바꾼 값을 반환
- `.strip([chars])` : 선택된 문자들 주위의 공백 제거해 반환
	- lstrip : 왼쪽의 공백만 제거
	- rstrip: 오른쪽의 공백만 제거
- `.split()` : 받은 문자를 기준으로 스트링을 리스트로 끊어서 반환
	- 기본적으로 공백을 기준으로 끊어준다.
	- 붙어있을 경우 list()를 활용
- ` 'separator'.join(iterable)` : 받은 문자를 문자의 사이사이에 끼워넣어서 문자열로 반환
- `.capitalize()` : 앞글자를 대문자로 만들어 반환
- `.title()`: 어포스트로피나 공백 이후를 대문자로 바꿔 반환
- `.upper()`: 다 대문자로 변형해 반환
- `.lower()`: 다 소문자로 변형해 반환
- `.swapcase()` : 대소문자 서로 변경해 반환

### 확인
- `.isalpha(), .isdecimal(), .isdigit(), .isnumeric(), .isspace(), .isupper(), .istitle(), .islower()`
	- 각자 이름에 맞는 형식의 스트링인지를 판단해 boolean값을 반환

## list.method()
### 추가
- `.append(x)` : 리스트에 값을 추가
- `.extend(iterable)` : 리스트에 iterable 값을 concatonate

### 변경
- `.insert(i, x)` : 원본의 i자리의 문자를 x로 대체

### 제거
- `.remove(x)` : 원본에서 값이 x인 것을 삭제합니다.
- `.pop(i)` : 인덱스 i에 있는 값을 삭제 후 그 항목을 반환
	- i가 지정되지 않으면 마지막 항목을 삭제하고 되돌려줌
- `.clear()` : 원본 안의 모든 값 제거

### 검색 및 정렬
- `.index(x)` : x값을 찾아 그 원소의 인덱스값 반환
- `.count(x)` : x값이 리스트 안에 몇 번 등장하는 지를 int로 반환
- `.sort()` : 원본을 정렬
- `.reverse()` : 원본의 값 순서를 뒤집음

### 복사
#### 얕은 복사(shallow copy)
말 그대로 겉 모양만 복사해온 경우로 1차원까지는 주소를 다르게 배정해주지만, 2차원 이상부터는 같은 주소를 공유함
- 슬라이서 활용 `[:]` : 전체를 그대로 잘라내서 반환
- `list()` 활용 : 리스트를 새로 리스트로 형변환해서 저장

#### 깊은 복사(deep copy)
2차원 이상의 원소들도 다른 주소에 배정되도록 복사
- `copy.deepcopy(list)` : list를 deep copy하여 반환

### List Comprehension
- 리스트를 작성할 때 리스트안에 표현식을 넣어 리스트를 작성하는 방법
- 여러 줄의 코드를 보기 쉽게 한줄로 바꿀 수 있음
- ex)
``` python
[expression for 변수 in iterable if 조건식]
[expression if 조건식 else 식 for 변수 in iterable]
```

### built in func
아래 세 개는 generator속성을 지닌 내장함수로 이런 속성을 가진 것들은 반환 시에 변수에 할당을 한다해도 일회 사용하는 순간 내부 데이터가 메모리에서 날아가기 때문에 list나 다른 데이터형태로 만들어서 저장을 해놓아야 영구적으로 쓸 수 있다.
-  `map(function, iterable)` : 각각의 원소에 function을 적용한 개체를 map개체로 반환
	- 내장함수외에 개인이 만들어낸 함수도 사용가능
-  `filter(function, iterable)` : iterable에서 function의 반환된 결과가 True 인 것들만 구성하여 filter 개체로 반환
   - 내장함수외에 개인이 만들어낸 함수도 사용가능
-  `zip(*iterables)` : 복수의 iterable 객체를 모아서 튜플의 모음으로 구성된 zip 개체를 반환한다.
   - 내장함수외에 개인이 만들어낸 함수도 사용가능

## set.method
### 추가 및 삭제
- `.add(elem)` : 원본에 elem을 세트에 추가
- `.update(*others)`: 원본에 여러가지의 값을 추가, iterable 개체만 가능
- `.remove(elem)` : 원본의 elem을 세트에서 삭제하고, 없으면 KeyError가 발생
- `.discard(elem)` : 원본의 elem을 세트에서 삭제, 없어도 에러 안남
- `.pop()` : 원본의 임의의 원소를 제거

## dict.method
### 조회
- `.get(key[, default])` : key를 통해 value를 가져옴, 기본갑 None 에러 나지 않음

### 추가 및 삭제
- `.pop(key[, default])`
	- key를 제거하고 그 값을 돌려줌. 없을경우 default를 반환.
	- default가 없는 상태에서 딕셔너리에 없으면 KeyError가 발생
- `.update(key=value)` : 값을 제공하는 key, value로 덮어씁니다.

### 반복문 작동 원리
- 기본적으로 키값을 기준으로 list의 for문과 같이 돌릴 수 있음

### dict comprehension
```python
{키: 값 for 요소 in iterable}
{키: 값 if (조건) else 값 for 요소 in iterable}
```

## 데이터 타입 추가
- decimal : int값이나 십진수 기반의 부동소수점 실수
- digit : 숫자로 표현된 모든 값
- numeric : 숫자로 표현된 것 이상으로 수식이나 연산 기호까지도 포함

## 파이썬 함수 속성
- generator
	- 새로운 개체를 만들어내는 로직를 반환하는 것
- iterator
	- for 문에 돌리는 등에 계산이 들어가는 행위를 할 수 있는 것
- decorator
