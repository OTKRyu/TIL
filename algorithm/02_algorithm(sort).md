# algorithm

## 정렬(sort)

2개 이상의 자료를 특정 기준에 의해 작은 값부터 큰 값 혹은 그 반대의 순서로 재배열하는 것

이를 정하는 것을 키라고 부른다.

### array 정렬

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
      for i in range(0,len(a)):
          c[a[i]] += 1
      for i in range(1, len(c)):
          c[i] += c[i-1]
      for i in range(len(a)-1, -1, -1):
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
  - 이 때 피봇을 어떤 값으로 정하냐에 따라 파티션이 변하는데 단순히 중간 인덱스에 해당하는 것을 고를수도, 중간, 끝, 시작 이렇게 세개의 값을 비교해 뽑을 것 등등 다양한 방법이 있다.
  - 대체로 좋은 성능을 보이나 특정 경우에는 버블정렬과 같은 결과가 되긴 한다.

```python
def quick_sort(a, begin, end):
    if begin < end:
        p = partition(a, begin, end)
        quick_sort(a, begin, p-1)
        quick_sort(a, p+1, end)
def hoare(array, l, r):
    pivot = l
    i = l + 1
    j = r
    while i <= j:
        if array[i] > array[pivot] and array[j] <= array[pivot]:
            array[i], array[j] = array[j], array[i]
            i += 1
            j -= 1
        elif array[i] > array[pivot]:
            j -= 1
        elif array[pivot] >= array[j]:
            i += 1
        else:
            i += 1
            j -= 1
    array[pivot], array[j] = array[j], array[pivot]
    return j
# 기본 아이디어는 왼쪽에서 오른쪽으로 진행하면서 피봇보다 작은 것은 그대로두다가 큰 것을 만나는 순간을 기준점으로 두고
# 이 기준점부터 피봇보다 작은 것을 왼쪽의 기준점과 교환해하고 기준점을 증가시킨다. 그러면 기준점 기준으로 왼쪽에는 전부
# 피봇보다 작게, 오른쪽은 전부 피봇보다 크게 만든다.
def lomuto(array, l, r): # 맨 오른쪽이 피봇
    x = array[r]
    i = l - 1
    for j in range(l, r):
        if array[j] <= x:
            i += 1
            array[i], array[j] = array[j], array[i]
    array[i + 1], array[r] = array[r], array[i + 1]
    return i + 1
```

- insetion
  - 도서관에서 책을 정렬할 때 쓰는 방법
  - 자료 배열의 모든 원소들을 앞에서부터 차례대로 이미 정렬된 부분과 비교하여, 자신의 위치를 찾아냄으로써 정렬을 완성한다.
  - O(n^2)의 시간 복잡도를 가진다. 다만 정렬이 되어있을 때 혹은 최선의 상황일 때는 n만에 끝나므로 정렬이 어느정도 되어 있다는 확신이 있을 때는 빠른 방법이다.
- merge(merge sort)
  - 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식
  - 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과를 얻어냄. 
  - top-donw방식
  - 항상 nlogn의 시간 복잡도를 가짐
  - 외부 정렬의 기본이 되는 정렬 알고리즘이며, 코어가 여러개인 프로세서에서 병렬화하기 쉬운 병합 정렬이 쓰인다.
  - 과정
    - 분할 단계 : 최소 크기의 부분집합이 될 때까지 분할
    - 병합 단계 : 2개의 부분집합을 정렬하면서 하나의 집합으로 병합, 이 때 합칠 때 하위 항목 두 개를 작은거부터 비교하면서 채워나간다.
  - 퀵과의 다른 점 : 피봇의 필요성 유무, 이 후 병합의 필요성 유무

```python
def merge_sort(nums):
    if len(nums) == 1:
        return nums
    middle = len(nums) // 2
    left = nums[0: middle]
    right = nums[middle:len(nums)]
    left = merge_sort(left)
    right = merge_sort(right)
    return merge(left, right)


def merge(left, right):
    global lcnt
    result = [-1] * (len(left) + len(right))
    if right[-1] < left[-1]:
        lcnt += 1
    l = 0
    r = 0
    idx = 0
    while 1:
        if l < len(left) and r < len(right):
            if left[l] <= right[r]:
                result[idx] = left[l]
                idx += 1
                l += 1
            else:
                result[idx] = right[r]
                idx += 1
                r += 1
        elif l < len(left):
            result[idx] = left[l]
            idx += 1
            l += 1
        elif r < len(right):
            result[idx] = right[r]
            idx += 1
            r += 1
        else:
            break
    return result

```

- heap sort
  - 최대 힙 혹은 최소 힙을 이용한 정렬이다
  - 모든 수를 힙에 넣은 후 하나씩 빼오면서 정렬을 시킨다.
  - nlogn이 걸리는 정렬이다.

### 위상 정렬(topological sorting)

- 순서가 있는 작업을 찰례로 진행해야할 때 순서를 결정하기 위해 사용하는 알고리즘
- 사이클이 없는 방향 그래프의 모든 노드를 주어진 방향성에 어긋나지 않게 순서를 나열하는 것
- 사전지식
  - dag
  - 진입 차수 : 특정 노드에 들어오는 간선의 개수
  - 진출 차수 : 특정 노드에서 나가는 간선의 개수
- 실제 구현 방법
  - 큐 사용(진입차수라는게 결국 필요한 조건이므로 이게 채워지면 0이 된다는 점을 이용)
    1. 진입 차수가 0인 노드들 큐에 삽입
    2. 큐가 공백상태가 될 때까지 반복
       1. 큐에서 원소를 꺼내 해당 노드에서 나가는 간선을 그래프에서 제거(연결된 노드의 진입 차수를 감소시킨다)
       2. 새롭게 진입 차수가 0이 된 노드를 큐에 삽입
  - 스택 사용
    1. 진입 차수가 0인 모든 노드에서 dfs시작
    2. dfs탐색
       1. 해당 노드를 방문 표시
       2. 인접하면서 방문하지 않은 노드가 있다면 dfs 재귀호출
       3. 함수 리턴 하기 전 stack에 현재 노드 저장
    3. 스택이 공백상태가 될 때까지 pop
- 위상 정렬 특징
  - 모든 정점을 방문하기 전에 큐가 공백상태가 되면 사이클이 존재( 사이클이 있다면 서로의 차수를 서로가 높여주기 때문에 0으로 만들 수 없음)
  - 고로 그래프의 유형이 dag일때만 유효
  - 여러가지 해답이 존재할 수 있다.(진입차수가 0일 때 어떤 순서로 처리하느냐에 따라)
  - 시간 복잡도 O(v+e)

