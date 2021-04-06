# algorithm

## 배열(array)

일정한 자료형의 변수들을 하나의 이름으로 열거하여 사용하는 자료구조

- 필요성
  - 여러 개의 변수에 접근할 때마다 새 이름을 짓는 것이 비효율적이기 떄문
- 선언
  - 변수의 처음 할당할 때 생성 혹은 미리 생성
- 접근
  - 인덱스로 접근

## 정렬(sort)

2개 이상의 자료를 특정 기준에 의해 작은 값부터 큰 값 혹은 그 반대의 순서로 재배열하는 것

이를 정하는 것을 키라고 부른다.

- bubble
  - 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막 자리까지 이동
  - 한 단계가 끝나면 마지막 원소만 정렬됨
  - 한개씩 물 위로 거품이 올라오는 정렬이라고 하여 bubble이라고 이름 붙음
  - 복잡도 n^2
  - 코딩이 쉽다는 장점이 있다.

```python
def bubble_sort(a):
    for i in range(len(a)-1,0,-1):
        for j in range(0,i):
            if a[j]>a[j+1]:
                a[j],a[j+1] = a[j+1],a[j]
```

- counting

  - 정수나 정수로 표현가능한 자료에만 적용가능하며 미리 할당을 해야하는 경우 최대 정수를 알아야 할당해놓을 수 있다.
  - 집합에 항목들이 몇 개씩 있는 작업후에 한번에 정렬하여 효율적인 알고리즘이다.
  - 복잡도 n+k(n: 리스트 길이, k: 정수 최대값)
  - 동일한 값이 있을 때 그 순서를 유지하는 안정정렬로 쓰기 위해서 순서를 뒤부터 배치한다.
  - k가 커지면 메모리할당측면에서 곤란해져서 비교적 작을 때 의미가 있다.
  - 코드를 읽어보면 알겠지만, 인덱스와 내용물을 뒤바꿔가면서 쓰고 있기 때문에 정수처럼 인덱싱과 겹쳐져도 문제가 없는 경우에만 쓸 수 있다.

  ```python
  def counting_sort(a, k):
      b = [0]*(len(a))
      c = [0]*(k+1)
      for i in range(0,len(b)):
          c[a[i]] += 1
      for i in range(1, len(c)):
          c[i] += c[i-1]
      for i in range(len(b)-1, -1, -1):
          b[c[a[i]]-1] = a[i]
          c[a[i]] -= 1
      return b
  ```

- selection

  - 가장 작은 값의 원소부터 차례대로 선택하여 맨 앞의 원소와 교환하고 다음번에는 맨 앞의 원소를 제외하고 진행하여 완성
  - 복잡도 n^2
  - 교환의 횟수가 적다.

```python
def selection_sort(a):
    for i in range(len(a) - 1):
        min = i
        for j in range(i+1, len(a)):
            if a[min] > a[j]:
                min = j
        a[i], a[min] = a[min], a[i]
```

- quick
  - 주어진 배열을 두 개로 분할한 뒤 각각을 정렬
  - 분할할 때 기준 아이템(pivot item)을 기준으로 작은 것은 왼편, 큰 것은 오른편에 위치
  - 평균 시간 복잡도 nlogn 최악의 경우 n^2

```python
def quick_sort(a, begin, end):
    if begin < end:
        p = partition(a, begin, end)
        quick_sort(a, begin, p-1)
        quickSort(a, p+1, end)
def partition(a, begin, end): # 호어 파티션
    pivet = (bigin + end) //2
    l = begin
    r = end
    while l<r:
        while(a[l]<a[pivot] and l<r) : l+=1
        while(a[r]>=a[pivot] and l<r) : r -= 1
        if l<r:
            if l==pivot: pivot = r
                a[l],a[r] = a[r], a[l]
    a[pivot], a[r] = a[r], a[pivot]
    return r
```

- insetion
- merge

### 순열

생성이 가면갈수록 많은 연산을 요구하기 때문에 재귀로 짜는 편이다.

### 부분합

- 반복문 두개로 모든 요소를 훑어가면서 계산을 해도 괜찮음
- 하지만 부분합 자체만을 구할 거라면 인덱스를 중복해서 돌리는 곳이 많음
- 이를 줄이기 위해 쓰는 기법 중 슬라이딩 기법이 있다.
  - 그래서 처음에만 부분합을 계산하고 그 뒤부터는 부분합의 첫번째 요소를 빼고 마지막 요소를 더해 다음 부분합을 구함
  - 이렇게 하면 연산량을 줄일 수 있음