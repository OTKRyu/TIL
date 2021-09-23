# astar

최단경로를 찾는 알고리즘

- 조건

- 노드의 상태

  - empty: 안 가봄
  - block: 갈 수 없는 곳
  - open: 발견됨 가 봐야할 곳
  - close: 가본 곳

- step 

  1. 도착한 노드 close
  2. 주변 node 탐색, state에 따라 
     - empty => open
     - block => nothing
     - close => nothing
     - open
       - g 값이 작으면 부모 node 변경
       - g 값이 크면 nothing
       - g 값은 출발지에서 이동한 거리
  3. open 노드 중, 최소 f값으로 이동, f값은 g값과 도착지까지 남은(manhattan distance) 거리를 합한 것이다.
  
- case

  - open 노드가 더 이상 없는 경우 => fail
  - 현재 노드가 도착지인 경우 => success
  
  