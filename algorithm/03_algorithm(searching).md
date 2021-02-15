# algorithm

## 검색

- 저장되어 있는 자료 중 원하는 항목을 찾는 작업
- 키를 가지고 검색할 수도 있다.

### 검색 방법

- 순차 검색(sequential search)

  - 일렬로 되어 있는 자료를 순서대로 검색
  - 구현이 간단한 만큼 검색 대상 수가 많아질수록 급격히 비효율적으로 변함
  - 정렬이 됐을 때와 아닐 때 찾는 방법에 약간의 차이가 있으며, 정렬이 됐을 때 조금 더 효율적으로 사용할 수 있다.

- 이진 검색(binary search)
  - 일단 정렬이 되어있어야만 함.
  - 중앙 원소를 고르고 목표 값과 비교한 후, 목표값이 크면 오른쪽을 작으면 왼쪽을 대상으로 다시 이 과정을 반복한다.
  - 재귀함수를 통해서도 구현할 수 있다.

```python
def binary_serch(a, key):
    start = 0
    end = len(a) - 1
    while start <= end:
        middle = (start + end) // 2
        if a[middle] == key:
            return True
        elif a[middle] < key:
            start = middle + 1
        else:
            end = middle - 1
    return False
```

## selection algorithm

저장되어 있는 자료로부터 k번째로 큰 혹은 작은 원소를 찾는 방법

실행방법 : 정렬 알고리즘을 통해 정렬 후 k번째 원소 찾기