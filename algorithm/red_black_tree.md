# red-black tree

## 왜 필요한가?

- java hashmap에서  key값 충돌이 일어날 때 이를 해결하기 위해서
- 리눅스 커널 스케줄러 - completelty fair scheduler

## red-black tree란?

- balanced search tree
  - 트리의 높이를 이진탐색트리의 최댓값으로 균형을 계속 맞추는 트리
  - avl tree
  - red-black tree
- red-black tree
  - node는 red 혹은 black 속성을 가진다
  - root와 leaf node는 black이다
  - black height: root로부터 아무 leaf node까지의 경로에서 거치는 black 노드의 갯수
  - node는 color를 저장하기 위한 추가 bit를 가진다
  - longest path는 shortest path의 2배를 넘지 않는다
    - shoortest path: 경로 상의 node가 모두 블랙
    - longest path: 블랙과 레드가 번갈아 나올 때

## red-black tree의 연산

- tree rotation
  - bst의 성질을 해치지 않고 subtree의 높이를 조정하는 연산
- insert operation
  - 새로운 원소를 삽입하는 연산
  - 처음에는 red 속성으로 초기화한다.
  - 그 이후 위치에 따라서 색깔을 변경한다.
    - 가짓수가 많으니 직접 찾아보길 바람
- delete operation
  - 기존 원소를 삭제하는 연산