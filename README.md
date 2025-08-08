# Docker 를 사용해야 하는 이유
	1. 내 pc에서는 잘 돌아가던 파일이, server 에서는 돌아가지 않는 상황 방지
	2. 어플리케이션을 구동하기 위한 런타임 환경에 필요한 것들을 일일이 설치하지 않고 도커에 한번만 넣어주면 되서 편리하다.
	3. 또한 이것저것 설정하고 해결해야(디버깅) 하는 어려움을 해결할 수 있다.

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

# 쿠보네틱스




  
