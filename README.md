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
