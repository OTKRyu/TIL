# algorithm

## 검색

- 저장되어 있는 자료 중 원하는 항목을 찾는 작업
- 키를 가지고 검색할 수도 있다.

### 숫자 검색 방법

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

- 선택 검색(seletion searching)
  - k번째로 작은 수를 구하는 방법
  - 정렬후 k번재 원소를 구한다.

### string searching
- 고지식한 알고리즘(brute force)
  - 처음부터 끝까지 차례대로 순회하면서 일일히 비교
  - 찾고자 하는 부분과 전체가 있을건데 이때 양쪽 인덱스를 함께 돌면서 부분의 첫번째와 전체의 첫번째가 같으면 같이 넘어가되 틀리면 부분은 맨 처음으로 돌아가지만 전체는 순회를 시작한 첫부분보다 한 칸 더 앞으로 가서 탐색을 진행하는 방식이다.
  - 시간복잡도 : 부분의 길이 * 전체의 길이
- kmp 알고리즘
  - 매칭이 실패했을 때 돌아갈 곳을 미리 정해놓고 그 부분에서만 연산을 처리하고 넘어가는 방법이다
  - 돌아갈 곳을 정할 때, 부분의 접두사와 접미사가 일치하는 경우를 찾아 그 때 접두사와 접미사의 길이의 최대치만큼을 뽑아내서 그 자리에 넣어 탐색할 곳을 찾는다. 이렇게 해서 표를 만들어두면 부분과 전체가 일치하지 않는 곳을 찾았을때 전체의 뒤로 돌아가서 찾는것이 아니라 앞부분의 맞는부분까지는 연산하지 않고 뒤의 알수없는 부분만 새로 계산하는 것이다.
  - 시간복잡도 : 부분의 길이 + 전체의 길이
- 보이어-무어 알고리즘
  - 오른쪽에서 왼쪽으로 비교
  - 대부분의 상용 소프트웨어가 채택하고 있다.
  - 불일치시, 불일치한 전체의 원소와 일치하는 부분의 원소가 있는경우 그 둘을 일치시킬때로 이동하여 진행한다. 이 때 일치하는 원소가 여럿이면 만약을 위해 가장 앞의 인덱스에 해당하는 원소와 맞춰 이동
  - 불일치한 전체의 원소와 일치하는 부분의 원소가 없는경우 부분의 길이만큼 이동하여 진행한다.
  - 시간 복잡도는 케이스에 따라 다르지만 평균적으로는 부분의길이 + 전체의 길이만큼 걸리고, 최악의 경우 부분의 길이 * 전체의 길이만큼 걸린다. 
- 카프 라빈 알고리즘
- 비교
  - brute force와 kmp는 전체를 한번씩은 일주하지만, 보이어무어는 그냥 건너뛰는 경우도 있다.
  - 보이어무어는 읽는 순서가 오른쪽부터다.

## 그래프 탐색

그래프를 탐색할 때 필요한 것이 현재 위치를 기준으로 어떤게 인접한지를 아는 것인데 이를 처리하는 방법이 인접행렬과 인접리스트 이렇게 두 가지가 있다.

- 인접행렬
  -  정점의 개수가 n일 경우 n by n짜리 행렬에 연결이 되어있다면 1을 아니라면 0을 넣어서 연결관계를 나타내는 것이다.
- 인접리스트
  - 정점의 개수가 n일 경우 n짜리 리스트를 만들어 n번째 점의 경우 n개의 연결관계중 연결이 된 점들의 번호로 표현하는 것이다.

그래프가 비선형 구조일 때는 모든 자료를 다 검색하는 것 자체가 간단하지 않기 때문에 모든 곳을 도는 것 자체도 중요해진다.

이 때 깊이를 우선하면 dfs(depth first search), 너비를 우선하면 bfs(breadth first search)라고 한다.

두 가지 방법 모두 제대로 구현하면 그래프 상의 모든 점을 탐색할 수 있으며, 해결해야하는 문제에 따라서 사용하는 것이다.

### dfs(깊이 우선 탐색)

- 시작 정점에서 한 방향으로 갈 수 있는 곳까지 갔다가 더 이상 갈 곳이 없게 되면, 마지막 갈림길로 돌아와 다른 방향으로 탐색을 계속하여 모든 정점을 방문하는 순회방법
- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가야하므로 후입선출 구조의 스택 사용

#### 실제 사용 방법

1. 시작 정점 v를 결정하여 방문
2. v의 인접한 정점 중에
   1. 방문하지 않은 정점 w가 있으면 정점 v를 스택에 push하고 w를 v로 하여 2.반복
   2. 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2)를 반복
3. 스택이 공백이 될 때까지 2.반복

```python
# psudo code
visited = [], stack = []
dfs(v):
	v 방문;
	visited[v] = True;
	do {
		if (v의 인접 정점 중 방문 안한 w 찾기)
			push(v):
		while (w){
		w 방문;
		visieted[w] = True;
		push(w);
		if 방문하지 않은 w의 접점이 있다면:
			w = 방문하지 않은 현 w의 새로운 접점;
		else:
			stack.pop();
			w = stack.peek();
		if stack.isEmpty == True:
			break
		}
	}
```

```python
# psudo code by recursion
def dfs_recursive(g, v):
    visited[v] = True
	for w in adjacency(g, v)
		if visited[w] != True:
			dfs_recursive(g, w)
```

```python
# psudo code by repeat
stack = []
visited = []
def dfs(v):
    stack.append(v)
	while stack:
		v = stack.pop()
		if not visited[v]:
			visit(v)  #v에서 해야할 일
			for w in adjacency(v)
				if not visited[w]
					s.push(w)
```

###  bfs(너비 우선 탐색)

- 인접한 정점들은 먼저 모두 차례로 방문한 뒤 방문했던 정점을 시작점으로 다시 인접한 정점들을 차례로 방문하는 방식

- 인접한 정점들을 탐색한 후 차례로 다시 너비우선탐색을 진행해야하므로 선입선출 형태의 자료구조인 큐를 활용
- 인접한 정점들을 거리에 따라 순서대로 계산하기 때문에 최단거리와 같이 거리가 얼마일때를 구해야하는 문제들에 활용하기 좋다.

```python
# 여러 가지 방법이 있지만 될 수 있으면 중복을 피하기 위해 점을 찾았을 때 방문처리를 하는 것이 좋고, 거리에 따라 다른 구분을 해야하는 경우도 반복 자체에 뭔가를 하는 것보다는 표시할 수 있는 자료구조를 만들어 따로 저장하는 편이 좋아보인다.
def bfs(g,v): # 될 수 있는 한 큐 한개짜리 쓰는게 좋은 것 같다 경험상 뭔가 문제가 잘 안 터진다.
    visited = [0]*n # n: 정점의 개수
    queue = []
    queue.append(v)
    visited[v] = True
    while queue:
        t = queue.pop(0)
        something(t) # t에서 해야할 뭔가의 일은 한다.
        for i in g[t]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
                
def BFS_with_distance(queue): # 거리에 따라 다른 연산을 해줘야 할경우 이런식으로 짠 후 조정하면 된다.
    while 1:
        new_queue = []
        while queue:
            v = queue.pop(0)
            visited[v] = 1
            for i in g[v]:
                if not visited[i]:
                    new_queue.append(i)
                    visited[i] = 1
        if new_queue:
            queue = new_queue
        else:
            break
```

```python
'''
단순 BFS는 하나의 큐에 모든 정점을 넣어가면서 탐색을 하지만, 이렇게 짤 경우 거리가 같은 것들의 탐색이 끝날때마다 심도가 깊어지면서 거리가 같은 것들에게만 어떤 연산을 할 경우에 쓸 수가 있다.
'''
def BFS_recursion_with_depth(g, queue):
    possibles = []
    while queue:
        v = queue.pop(0)
        visited[v] = 1
        for i in g[v]:
            if not visited[i]:
                possibles.append(i)
                visiited[i] = 1
    if possibles:
        BFS_recursion_with_depth(g, possibles)
    else:
        return
```

### 최단 경로

- 정의 
  
  - 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
  
- 하나의 시작 정점에서 끝 정점까지의 최단경로
  - dijkstra algorithm(greedy)
    - 음의 가중치 허용하지 않음
    - 시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식
    - s~t까지의 최단 경로에 정점 x가 존재
    - 이 때 최단 경로는 s~x~t로의 최단경로로 구성   
    - 실제 방법
      - 시작점을 정해놓고 시작점과 인접해 있는 점들 중 비용이 작은 정점을 찾는다.
      - 이 점에서 인접한 점들을 가지고 와서, 시작점에서 인접한 점으로 갈 때 직접가는 비용과 경유해 가는 비용을 찾아 최소치를 뽑아낸다.
      - 이 과정을 반복하면 시작점에서 다른 점까지 가는데에 필요한 최소비용들을 계속 뽑아낼 수 있다.
      - 그러다 모든 정점을 뽑아보면, 끝 정점으로 가는 최소비용도 뽑아낼 수 있게 된다. 
      - 매번 최솟값을 찾아다녀야되는 연산이기 때문에 정점마다 최소 힙을 만들어두면 조금 더 빠르게 진행할 수 있다.
  
  ```python
  minimums = [inf for i in range(n)] 
  # minimums는 최소 거리를 모아놓은 집합, ds는 그래프간 연결관계와 각 정점끼리의 거리를 모아놓은 인접행렬
  while 1:
      minimum = inf
      for i in range(n):
          if minimum > minimums[i] and check[i] == 0:
              idx = i
              minimum = minimums[i]
      if idx == e:
          break
      else:
          check[idx] = 1
          for i in range(n):
              if ds[idx][i] != inf and check[i] == 0:
                  minimums[i] = min(minimums[i], minimums[idx]+ds[idx][i])
          short = inf
          for i in range(n):
              if short > ds[idx][i] and check[i] == 0:
                  s_idx = i
                  short = ds[idx][i]
          minimums[s_idx] = min(minimums[s_idx],minimums[idx] + short)
          for i in range(n):
              if ds[s_idx][i] != inf and check[i] == 0:
                  minimums[i] = min(minimums[i], min(minimums[idx] + ds[idx][s_idx] + ds[s_idx][i], minimums[idx]+ds[idx][i]))
  ```
  
  - bellman-ford
    - 음의 가중치 허용
  
- 모든 정점들에 대한 최단 경로
  - floyd-warshall 
    - 음의 가중치 허용
    - 한점에서 다른 한점으로 갈 때 경유해서 갈 수 있는 모든 경우의 수를 고려해 그중 최솟값을 선택하는 방법이다.