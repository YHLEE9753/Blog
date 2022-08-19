# Docker 와 GitHub Action 을 이용한 CICD(1) - CICD 구축
2022/08/20

## 1. Docker 와 GitHub Action 을 이용한 CICD
### build.gradle
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
- medigo.jar 로 jar 파일 1개만 build.lib 에 생성되게 설정

### Dockerfile
```dockerfile
FROM openjdk:11
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} medigo.jar
ENTRYPOINT ["java","-jar","/medigo.jar"]
```
- openjdk를 이용하여 타겟 디렉터리의 jar를 컨테이너의 medigo.jar로 넣고 실행한다는 의미


### ec2 설정
```shell
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
```



### workflow
```shell
name: medigo-deploy-1.0.0

on:
  # 수동 트리거 부분(무시하세요)
  workflow_dispatch:
    inputs:
      name:
        description: "Test scenario"
        required: true
        default: "Run Deployment"
      home:
        description: "User name"
        required: false
        default: "YHLEE9753"
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    # java 11 사용 adopt 가 아닌 temurin 사용 권장
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'temurin'

    - name: Grant execute permission for gradlew
      run: chmod +x gradlew

    - name: Build with Gradle
      run: ./gradlew build

    # 나의 Docker username 과 password 를 github secret 에 추가하여 접속할 수 있게 설정
    # dldydgns53/medigo 로 build 하고 push 한다.(repo 미리 생성 - 안해도 되나?)
    - name: Docker build
      run: |
        docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
        docker build -t dldydgns53/medigo .
        docker push dldydgns53/medigo

    # 원래는 배포 후 ssh 에 접속하여 이미지를 pull 하고 run 해야 한다.
    # 이부분은 회사마다 규약이 다르지만 이번 프로젝트에는 이 또한 자동화 시켜 보자
    # ssh-action 을 사용하여 ssh 에 직접 접속할 수 있게 만든 후 
    # dldydgns53/medigo 를 pull 하고 run 시킨다.(8080포트를 매핑하여 80에 띄운다)
    - name: Deploy
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.AWS_SERVER_DNS }}
        username: ec2-user
        key: ${{ secrets.SSH_KEYGEN_PRIVATE_KEY }}
        envs: GITHUB_SHA
        script: |
          docker pull dldydgns53/medigo
          docker stop server
          docker run -d --rm --name server -p 80:8080 dldydgns53/medigo
```

### ssh-action
- ssh-action 이다. 터미널에서 작업을 자동화 할 수 있다.
- ssh 키를 우선 세팅한다
- ec2 에서 `cd ~/.ssh` 를 통해 key 가 있는 곳으로 이동 이곳에 `authorized_keys` 가 존재한다.
- `ssh-keygen -t rsa -b 4096 -C "[내메일]" -f medigo` 를 통해 key pair 를 생성한다.
  - `ssh-keygen -t rsa -b 4096 -C "dldydgns53@naver.com" -f medigo`
- `medigo`, `medigo.pub` 를 얻을 수 있다. 개인키인 `medigo` 는 github secret 에 `SSH_KEYGEN_PRIVATE_KEY` 로 저장한다
  - 주의!! : 이때 `-----BEGIN OPENSSH PRIVATE KEY-----` 와 `-----END OPENSSH PRIVATE KEY-----` 까지 전부 복사한다.
- 공개키인 `medigo.pub`은 해당 폴더에 있는 `authorized_keys`에 추가해야 한다.
- `cat medigo.pub >> authorized_keys` 를 통해 authorized_keys에 추가한다
  - 단일 직각 브래킷>>이 아닌 이중 직각 브래킷>을 사용해야 한다. Double은 추가를 나타내고 single은 덮어쓰기를 나타낸다

#### 사이트 들어가면 docker cicd 적용된거 확인가능하다 성공적!!!


## 참고 사이트
https://itcoin.tistory.com/685

https://intrepidgeeks.com/tutorial/deploy-github-tasks-to-the-server-via-ssh-and-synchronization#!