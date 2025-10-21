# 배포
	"내 로컬에서만 실행되던 프로그램(웹/서버)를, 다른 사람들도 인터넷을 통해 접근 할 수 있게 만드는 과정"

	즉, 내 컴퓨터 안에서만 돌아가던 Node.js 서버나 React 웹 페이지를 
	인터넷 주소(https://000.com)로 누구나 접근 가능하게 만드는 걸 의미한다.

	배포의 핵심 구성 3가지
		1. 서버(server)
			내 코드를 실행시켜줄 컴퓨터(가상 or 물리) 
			aws ec2, vercel, render
		2. 데이터베이스(DB) 
			데이터(회원정보, 글, 로그 등)를 저장할 곳
			aws rds, mongoDB Atlas, supabase
		3. 도메인(domain)
			사람들이 접속할 수 있는 주소(URL) 

	배포의 일반적인 흐름
		1. 코드를 서버에 올림
			github > 배포 플랫폼(vercel, render, aws등) 으로 연결
			또는 직접 zip 파일 업로드
		2. 서버 실행 환경 세팅
			node.js 설치, npm install
			npm start 또는 pm2 start server.js
		3. DB 연결
			로컬 DB URL > 클라우드 DB URL 로 변경
		4. 도메인 연결
			기본 도멘인을 쓰거나, 직접 구매한 도메인 주소를 연결 가능

	배포 형태의 3가지 단계
		개발 환경(Local) 내 컴퓨터에서 테스트 http://localhost:3000
		테스트/스테이징 배포 전 검증용 서버 내부 테스트용 URL (staging)
		운영환경 실제 사용자가 접속

	가상 서버
		물리 서버가 아닌, 클라우드 상에 만들어진 내 전용 컴퓨터
		코드 실행, 요청 처리, api 응답 담당
		AWS EC2, Render, Railway, DigitalOcean Droplet, Google Compute Engine 등
	가상 DB
		물리 서버에 DB를 설치하지 않고, 클라우드에서 DB만 빌려 쓰는 서비스
		AWS RDS, MongoDB Atlas, Supabase, PlanetScale, Neon 등

# 배포 플랫폼 종류 (aws 말고)
	프론트엔드 전용 Vercel, Netlify : react, next.js, vue 등 정적/ssr 페이지 배포에 특화
	풀스택(백 + 프론트) render, railway, Fly.io : node.js 서버 + DB 함께 배포 가능
	전문 클라우드 aws, gcp, Azure : 서버, DB, 로드, 밸런스, 보안 등 전부 직접 세팅 가능
	
# Ec2 + RDS
	서버 관리 책임 : 직접 관리
	자동화 수준 : 낮음
	커스터 마이징 : 매우 높음
	특징 : 완전 수동 제어, 모든 설정(보안, 포트, 스케일링 등) 직접 해야한다.
	이상적 사용 사례 : 실무/학습용, 서버 구조 이해 목적

# Elastic Beanstalk
	서버 관리 책임 : AWS 대부분 관리
	자동화 수준 : 중간 ~ 높음
	커스터 마이징 : 제한적
	특징 : ec2, rds, auto scaling 등을 자동 구성, 코드만 업로드하면 배포됨
	이상적 사용 사례 : 일반적인 웹 서비스, 빠른 배포

# Lightsail 
	서버 관리 책임 : 간단한 관리
	자동화 수준 : 높음
	커스터 마이징 : 낮음
	특징 : 초보자용 ec2, 고정요금, 클릭 몇 번으로 서버 + DB 설정 가능
	이상적 사용 사례 : 소규모 프로젝트, 개인 포트폴리오

# Fargate
	서버 관리 책임 : 거의 없음
	자동화 수준 : 매우 높음
	커스터 마이징 : 높음(컨테이너 기반)
	특징 : Docker 컨테이너를 자동 실행.관리.ec2 없어도 됨
	이상적 사용 사례 : 대규모 트래픽, DevOps 자동화 환경

# Docker 를 사용해야 하는 이유
	1. 내 pc에서는 잘 돌아가던 파일이, server 에서는 돌아가지 않는 상황 방지
	2. 어플리케이션을 구동하기 위한 런타임 환경에 필요한 것들을 일일이 설치하지 않고 도커에 한번만 넣어주면 되서 편리하다.
	3. 또한 이것저것 설정하고 해결해야(디버깅) 하는 어려움을 해결할 수 있다.
    4. Docker 공부는 필수 

 시간 날 때 마다 공부 하기 도커

# docker 의 흐름
	1. local macine 에서 이미지 생성 push [Container Registry]
	2. container registry 에서 내가 생성한 이미지가 저장되어 있다면
	3. 다른 pc 또는 서버에서 [container registry] 이미지를 pull 하여 가져온다.
	4. 이때!  docker 와 같은 [컨테이너 엔진]을 설치해야한다.

# docker 설치해야하는 곳
	1. 우리가 개발하고 있는 머신에 docker 설치
 	2. server 에도 docker 설치
	3. 어플리케이션에서 구동하는데 필요한 docker 파일을 작성한 다음
 	4. 어플리케이션에서 스냅샷 할 수 있는 이미지를 생성 Build 를 통해 image 생성
  	5. image 를 [container registry] 에 push 를 진행
   	6. server 에서 다운 받아서 [pull] 컨테이너를 실행
 
# docker 체험 명령어
1. npm init -y (프로젝트 초기화)
2. npm i express (심플한 팬들 생성)
3. index.js 파일 생성
4. dockerfile 생성 (어떤 이미지를 만들 건지/ 프로젝트에 어떤 것들이 필요한지를 명시 하는 곳)
5. docker build -f Dockerfile -t fun-docker . (error 가 나는 경우 docker 데스크탑 실행 안해줘서 나는 오류)
6. docker images (이미지 확인)
7. docker run -d -p 8080:8080 fun-docker (d 는 디테치), (p는 포트번호)
8. docker ps (실행중인 도커를 확인 할 수 있다)
9. docker logs [숫자] (실행중인지 를 확인 할 수 있다.
10. docker 데스크탑 사이트에서 레파지토리 생성 docker-example
11. 만든 이미지의 이름을 변경
12. docker login 진행
13. docker push 를 통해 생성한 이미지를 내 레파지토리에 넣어주는 역할을 진행
14. 도커 데스크탑 사이트에서 제대로 올라왔는지 확인 하면 끝

# docker ec2 가상 컴퓨터에서 실행하기
	1. gradlew clean build
	2. docker build -t kimminseongg/board:v1 .
	3. docker push kimminseongg/board:v1
	4. ssh -i docker.pem ubuntu@44.211.134.11
	5. docker pull kimminseongg/board:v1
	6. docker run kimminseogg/board:v1
	7. docker run -d -p 8060:8060  kimminseongg/board:v1

# docker 내 컴퓨터에서 실행하기
 	1. cmd 에서 실행하는 방법 
  	2. docker run -p 8060:8060 kimminseongg/board:v1 실행
   	3. http://localhost:8060/board/openBoardList.do
    4. 추가로 cmd 에서 ipconfig /all 을 사용하여 자신의 컴퓨너 io 를 사용하여 프로젝트 실행이 가능하다. localhost 부분을 ip 주소로 대체

# docker 활용해서 다른 프로젝트 올려보기
ex) react, vue, nexacro 등..
백단 spring boot 사용

# docker, 젠킨스, 쿠보네틱스, 컴포저, 주키퍼, 카프카
공부해야할 내용

# 백엔드
## Spring boot 에서 rest API를 설계할 때 사용하는 구조 
 mvc 패턴 (model-view-controller) 
 rest api 환경에서 view 대신 json 등으로 데이터를 반환

 일반적인 spring boot rest api 구조 
  controller : API 요청 처리
  service : 비즈니스 로직 처리 (인터페이스)
  serviceimpl : 구현체 (유지보수성 UP)
  repository : DB 접근 JPA, MyBatis
  DTO : Data transfer object 요청/응답 모델
  entity : DB Entity 정의 (JPA)
  config : 설정 관련 클래스

  순서 controller-service-serviceimpl-dao-mapper-xml-db
  즉, dao 인터페이스와 xml mapper가 mybatis를 통해 자동 연결되는 구조

## JPA (java persistence api)
 객체-관계 매핑, java 객체 <-> DB 자동 매핑
 장점) 코드 양이 적음, 유지보수 쉬움, 관계 매핑 편리
 단점) 복잡한 쿼리 작성 어려움, 성능 튜닝 어려울 수 있음

 ex) 간단한 회원/게시판/댓글 시스템 jpa + repository

## mybatis
 sql 매핑 플레임워크, sql을 직접 작성하여 매핑
 장점) 복잡한 쿼리 자유롭게 작성 가능, db 튜닝 용이
 단점) xml 또는 어노테이션 방식 사용 -> 코드 양 많아짐 유지보수 복잡해질 수 있음

 ex) erp, mes 처럼 데이터가 많고 복잡한 조인/조건이 많을 떄 mybatis가 적합

# Rest Api 설계 시 보완적으로 쓰이는 기술/패턴
 DTO : api 요청과 응답 시 entity와 분리된 객체 사용, 보완성 및 유지보수 향상
 ExceptionHandler : 전역 예외 처리 - API 오류 응답 일관성 있게 처리
 validation : 입력값 검증 처리
 ResponseEntity : Http 상태 코드 + 응답 객체 함께 전달
 Swagger : Open API 문서 자동화 도구
 AOP : 로깅, 트랜잭션, 보안 등의 횡단 관심사 분리
 JWT, OAuth2 : 인증/인가 처리 시 사용




  
