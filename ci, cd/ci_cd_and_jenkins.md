# jenkins

## 왜 ci/cd인가?

- 워터폴프로세스서는 딜리버리가: one time
- 애자일 프로세스에서는: many times
  - 매 스프린트마다딜리버리를하려면 요구사항 분석,설계,개발,qa를 해야함
  - 일의 양이 만만치 않음
  - 이에 대한부담을 줄이려면 자동호과 필수 곧 ci/c의 탄생

## jenkins

- java로 작성된 자동화 서버
- 다양한 플러그인을 사용하여 ci/cd 파이프라인을 만들어 자동화 작업을 완성
- mit licence

### 설치

- 베어 인스톨
  - 인스톨러를 사용해 pc나 서버에 설치
  - jre 8,11만 지원
- 컨테이너
  - docker pull jenkins/jenkins
  - 컨테이너에 jre가 포함
  - docker 컨테이너에 대한 이해 필요

