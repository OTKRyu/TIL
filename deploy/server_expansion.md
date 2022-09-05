# 서버 확장

1대의 서버로 시작해 여러 서버로 늘려나가는 과정

시작은 1대로 하지만 사용자가 늘어난다면 1대로는 트래픽을 감당할 수 없게 된다.

이 때 봐야할 것이 cpu bound와 io bound이다.

- cpu bound
  - 연산량이 많아서 cpu의 가동률이 높은 경우
  - ex) 블록체인의 해시연산등
- io bound
  - 읽고 쓰기 혹은 데이터베이스 부하가 많은 경우

## 확장 방법

- scale-up
  - 말그대로 서버 한 대의 성능을 높여서 해결하는 방법
  - cpu bound의 경우 고성능의 cpu로 대체 혹은 추가 cpu 투입
  - io bound의 경우 ssd 등을 이용해 성능 향상
- scale-out
  - 기능별로 분리해서 여러 대의 서버를 운용하는 것
  - 서버가 여러 대이므로 리버스 프록시 기능이 필요, 대표적으로 로드 밸런서
  - 로드 밸런서
    - l4/l7 스위치
      - HAproxy, nginx
    - 분산 방식
      - round robin
      - hash
      - 부하가 적은 쪽 우선
    - health check
      - 서버가 분산된 요청을 제대로 받을 수 있는 상태를 수시로 체크하는 것을 말한다. 
- auto scaling
  - 클라우드 서비스가 제공하는 서비스로 조건에 따라 자동적으로 서버를 자동적으로 올렸다내렸다 관리해주는 기능

### db가 늘어날 경우

- master-slave
  - 실시간 복제
  - write는 master만
  - read는 가까운 곳, 가깝다는 것은 대체로 권역을 말한다.(ex) 아시아, 아메리카, 유럽 등)
- sharding
  - 인덱스별로 db를 나누는 방법
  - 자료 탐색면에서 문제가 생길 수가 있다.
  - join시의 문제가 발생할 수 있다.
- proxy 추가
  - HAproxy
  - nginx
- 캐시 layer 추가
  - 일반적으로 db 아페
  - read는 대체로 캐싱의 효율이 좋다
  - write back의 주기를 적절히 설정하는 것이 중요하다.
  - memcached, redis
  - content delivery network(cdn)

## 분산 시스템

- 주어진 작업을 여러 노드에서 나누어 수행
- consensus protocol(합의)
- 한 두개의 노드에서 장애는 일상
- 모든 과정에서 자동화는 필수
- 시스템의 가시성 확보
- scale-out 보다는 elastic