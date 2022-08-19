# Spring Boot - Docker 를 이용한 수동 배포
2022/08/20

## 1. Docker 를 이용한 수동 배포
### 코드 작성
- spring web 만 가능한 간단한 spring project 를 만든다(이때 java version 은 11 이다)
- Docker 배포를 위해 build.gradle 를 수정한다.
```java
jar {
	enabled = false
}

bootJar{
	archivesBaseName = 'medigo'
	archiveFileName = 'medigo.jar'
	archiveVersion = "0.0.0"
}
```
- jar 는 1개의 jar 파일만 만들게 한다
- bootJar 는 만들어지는 jar 파일의 이름을 medigo.jar 로 설정한다.
- 이제 Dockerfile 를 만들어보자
```dockerfile
FROM openjdk:11
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} medigo.jar
ENTRYPOINT ["java","-jar","/medigo.jar"]
```
- java 11 버전의 환경을 구성한다
- build가 되는 시점에 JAR_FILE 이라는 변수명에 build/libs/*.jar 표현식을 선언했다는 의미이다
- build/libs 경로는 gradle로 빌드했을 때 jar 파일이 생성되는 경로이다.(Maven의 경우 target/*.jar 로 설정하면 된다)
- 위에 선언한 JAR_FILE 을 medigo.jar 로 복사한다.
- ENTRYPOINT : jar 파일을 실행하는 명령어(java -jar jar파일) 이다.
  - `ENTRYPOINT ["java","-jar","-Dspring.profiles.active=prod","/app.jar"]`
  - 운영 및 개발에서 사용되는 환경 설정을 분리해서 사용할 경우, 위와 같이 ENTRYPOINT를 설정할 수 있습니다.

### Docker hub 에 올리기
```java
// jar 파일 생성(build/libs 경로에 생성)
./gradlew build -x test
// (-x test: 테스트 실행 X)

// dockerfile -> docker image 로 빌드를 진행
docker build --build-arg DEPENDENCY=build/dependency -t 도커허브 ID/Repository --platform linux/amd64 .
docker build --build-arg DEPENDENCY=build/dependency -t dldydgns53/medigo .
// 맥북 M1의 경우 --platform linux/amd64 옵션을 마지막에 추가
// 가장 마지막 점(.) 도 포함

// 생성한 docker image 파일을 Docker Hub에 push
docker login
docker push dldydgns53/medigo
```

### AWS EC2 에서 실행하기
- EC2 인스턴스 생성 후 도커를 설치한다.
```text
도커 설치
$ sudo yum install docker

도커 실행
$ sudo systemctl start docker

도커 허브에 존재하는 이미지 파일 pull
$ sudo docker pull dldydgns53/medigo

도커 이미지를 통해 스프링 부트 애플리케이션 배포(실힝)
$ sudo docker run -p 8080:8080 dldydgns53/medigo
```
- ec2 ip 에 8080 포트 진입 시 배포가 성공된것을 확인할 수 있다.