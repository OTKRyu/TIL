# algorithm

## 배열(array)

일정한 자료형의 변수들을 하나의 이름으로 열거하여 사용하는 자료구조

- 필요성
  - 여러 개의 변수에 접근할 때마다 새 이름을 짓는 것이 비효율적이기 떄문
- 선언
  - 변수의 처음 할당할 때 생성 혹은 미리 생성
- 접근
  - 인덱스로 접근

## array 2

### 2차원 배열의 선언

- 배열들을 모아놓은 배열
- 차원에 따라 2개의 인덱스를 사용
- 파이썬에서는 별도의 작업없이도 그저 값을 대응시키는 것으로 사용 가능
- 입력 받는 방법 3가지
  - 각자 변수명을 정해서 따로따로 받아서 리스트에 넣기
  - 리스트를 미리 만들어놓고 반복문으로 한 리스트씩 리스트안에 넣기
  - list comprehension으로 작성

#### 배열의 순회

- 행 우선 순회
- 열 우선 순회
- 지그재그 순회
  - 행 우선 조회이나 한번은 정방향 한번은 역방향으로 조회하는 것을 말함

```python 
arr # 2차원 배열
n, m # 행, 열의 길이
for i in range(len(arr)):
    for j in range(len(arr[i])):
        arr[i][j+(m-1-2*j*(i%2))]
#another answer
for i in range(len(arr)):
    if i % 2 == 0:
        for j in range(len(arr[i])):
            arr[i][j]
    else:
        for j in range(len(arr[i])-1, -1, -1):
            arr[i][j]
```

- 델타를 이용한 2차 배열 탐색
  - 단위 이동에 해당하는 연산을 정의해놓은 뒤 이를 이용해 순회하는 방법

```python
dr = [ -1, 1, 0, 0]
dc = [0, 0, -1, 1]

for i in range(4):
    nr = r + dr[i]
    nc = c + dc[i]
# 이런 식으로 인덱스에 직접 더하고 뺄 것을 미리 정해놓고 시작하는 것
# 다만 파이썬에서는 음수인덱스를 지원하니까 에러가 없지만, 다른 언어에서는 에러가 날 수도 있다.
# 다른 언어에서 쓰려면 if문 등을 이용해 에러가 날 경우를 미리 방지해야한다.
```

- 전치 행렬(transpose)
  - n by n행렬일때 대각선을 기준으로 대칭적으로 요소를 바꾼 행렬