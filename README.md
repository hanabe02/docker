# docker

	1. gradlew clean build
	2. docker build -t kimminseongg/board:v1 .
	3. docker push kimminseongg/board:v1
	4. ssh -i docker.pem ubuntu@44.211.134.11
	5. docker pull kimminseongg/board:v1
	6. docker run kimminseogg/board:v1
	7. docker run -d -p 8060:8060  kimminseongg/board:v1
http://44.211.134.11:8060/board/openBoardList.do
