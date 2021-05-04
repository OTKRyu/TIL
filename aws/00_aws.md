# aws

## ch1 클라우드 컴퓨팅

뭐 여러 개념도 있고 이에 대해서 정의한 것도 꽤나 오래되었지만, 결국 현대에 각광받고 있는 이유는 대규모로 클라우드 서비스를 시행할 경우 서비스하는 쪽에서도, 서비스를 쓰는 쪽에서도 경제적이기 때문이다.

## ch2 aws

1. 컴퓨팅 관련 서비스
   - elastic compute cloud : 보통 생각하는 클라우드 컴퓨팅, 손쉽게  용량을 줄이거나 늘릴 수 있다.
   - auto scaling : 사용자가 정의해둔 조건에 따라 알아서 용량이 조절되도록 하는 서비스
   - elastic mapreduce : 대용량 데이터 특화 서비스
2. 스토리지 관련 서비스
   - s3 : 온라인 상의 저장소
   - amazon glacier : 저렴한 저장소로 빈번히 업데이트하기에는 부적절하지만 백업용으로 좋다.
   - import/export : 실제로 저장용기를 가지고 가서 업데이트하는 서비스로 네트워크비용등이 절감되나 우리나라는 데이터센터가 없어서 굳이 쓸 일이 없다.
   - storage gateway : 물리적 it환경처럼 저장공간을 쓰는 서비스. 자주 쓰는 데이터는 캐시에 저장하듯 캐싱데이터 전용공간과 영구적으로 저장할 데이터를 나누어 따로 저장하여 효율을 높이고 비용을 낮춘다.
3. 네트워크 관련 서비스
   - elastic Ip : ec2 instance에 공인 ip 제공, 변동되지 않는 서비스일 경우에만 적합
   - elastic load balancing : 수신되는 트래픽을 자동으로 배분
   - route 53 : domain name service로 외부 도메인과의 연결도 해준다.
   - virtual private cloud : 사설망 구축용, 클라우드 내부에서만 사설망을 쓰도록 만들 수도 있고, vpn 연결을 이용하여 물리적 네트워크와도 연결가능
   - direct connect : amazon 데이터센터와 직접 연결망 구축, 우리나라는 데이터센터 없어서 쓸일이 없다.
4. db관련 서비스
   - 관계형 db : amazon relational database service(rds) 범용 db와 비슷한 엔진이 제공되고 db관련 잡다한 일을 대신 해준다.
   - 비관계형 db : dynamodb, nosql 기반 데이터베이스로 ssd사용
   - amazon elasticache라는 캐싱 서비스를 제공함으로서 좀 더 빠른 검색을 지원한다.
   - 추가로 redshift라는 신규 서비스는 데이터 웨어하우스 구축을 지원한다.
5. 어플리케이션 관련 서비스
   - cloudsearch : 검색 엔진을 손쉽게 추가할 수 있는 기능, 다만 한글 미지원
   - simple workflow service(swf) : 분산 어플리케이션 간 작업을 work flow라는 형태로 조정 가능하게 하는 서비스
   - simple queue service : 컴퓨터간 통신을 손실 없이 순서에 입각해 전달시켜주는 서비스
   - simple notifications service : 통보 기능을 구축할 수 있도록 하는 서비스
   - simple email service : 대량의 이메일 전송이 가능한 메일 서비스를 구축할 수 있도록 제공
   - flexible payments service 등 여러 서비스 존재
6. 콘텐츠 전송 관련 서비스
   - contents delivery network(cdn) : cloud front라는 이름으로 서비스 제공
7. 배포 서비스
   - cloudformation : 사전 작성된 템플릿 기반으로 필요한 리소스를 자동생성해주는 서비스, 사용요금은 없고 리소스 생성시부터 요금이 있다.
   - elastic beanstalk : 지원되는게 좀 한정적이지만 cloudformation보다는 덜 난해하다. 이 또한 생성된 리소스에만 요금 부과
   - opsworks : aws 고유 템플릿 문법이 아닌 chef의 레시피 문법을 사용한다. 유닉스 및 리눅스에서만 동작하는 점은 단점이지만, 사용환경만 맞다면 훨씬 편하다고 한다.

## ch3 aws를 이용한 web service 구축

