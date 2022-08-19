# Docker 와 GitHub Action 을 이용한 CICD(2) - MySQL, Redis 연동
2022/08/20

## 1. Docker 와 GitHub Action 을 이용한 CICD
- 의존성을 추가하고 yml 파일을 작성하라
### build.gradle
- redis 의존성 추가
- mysql 의존성 추기

### application.yml
#### application.yml
```yaml
spring:
  profiles:
    active: dev
    group:
      main: main, common
      dev: dev, common
```
#### application-dev.yml
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://mysqlDB:3306/kppl?serverTimezone=Asia/Seoul
    username: admin
    password: 1234
  redis:
    host: redis_boot
    port: 6379
  jpa:
    database: mysql
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
```
#### application-common.yml
```yaml
spring:
  application:
    name: stuti
  jpa:
    open-in-view: false
    properties:
      hibernate:
        default_batch_fetch_size: 500
        format_sql: true

```
## 2. deploy.yml 수정
- mysql 과 redis 이미지를 pull 하여 컨테이너화 한후
- 서비스 컨테이너에 연결하자
```yaml
    - name: Deploy
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.AWS_SERVER_DNS }}
        username: ec2-user
        key: ${{ secrets.SSH_KEYGEN_PRIVATE_KEY }}
        script: |
          docker pull dldydgns53/medigo
          docker stop server
          docker network create -d bridge medinet
          docker run -d --network medinet --network-alias mysql -v  /build/DB/mysql:/var/lib/mysql --name mysqlDB -e MYSQL_DATABASE=kppl -e MYSQL_USER=admin -e MYSQL_PASSWORD=1234 -e MYSQL_ROOT_PASSWORD=1234 -p 3306:3306 mysql
          docker run -d --network medinet --network-alias redis -v  /build/DB/redis:/var/lib/redis --name redis_boot -d -p 6379:6379 redis
          docker run -d --rm --name server --network medinet -p 80:8080 dldydgns53/medigo
```
- docker network 를 통해 같은 network 를 공유한다
- db 는 volumn 처리를 해주자
- 성공적으로 cicd 가 적용된것을 확인할 수 있다.

## 3. docker-compose 로 작성
- docker-compose 로 작성할 경우 해당 스크립트를 ec2 안에 저장한후
- medigo-cicd.yml 파일을 수정해야 한다.
- 해당 compose 는 미적용하였다.
#### medigo-cicd.yml
```yaml
    - name: Deploy
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.AWS_SERVER_DNS }}
        username: ec2-user
        key: ${{ secrets.SSH_KEYGEN_PRIVATE_KEY }}
        script: |
          docker pull dldydgns53/medigo
          docker stop server
          docker compose-up -d
```
#### docker-compose.yml(1)
```yaml
version: "3"
services:
  mysqlDB:
    image: mysql
    container_name: mysqlDB
    environment:
      - MYSQL_DATABASE=kppl
      - MYSQL_ROOT_PASSWORD=1234
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=admin

    ports:
      - 3306:3306
    networks:
      - test
  redisDB:
    image: redis
    command: redis-server --port 6379
    container_name: redis_boot
    hostname: redis_boot
    ports:
      - 6379:6379
  app:
    build: .
    ports:
      - 8080:8080
    restart: on-failure
    depends_on:
      - mysqlDB
      - redisDB
    networks:
      - test
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysqlDB:3306/kppl?serverTimezone=Asia/Seoul
      SPRING_DATASOURCE_USERNAME: mysql
      SPRING_DATASOURCE_PASSWORD: 1234

networks:
  test:
```
- 하지만 해당 스크립트는 실패가 된다.(db 연동 실패)

#### docker-compose.yml(2)
```yaml
version: "3"
services:
  mysqlDB:
    image: mysql
    restart: always
    container_name: mysqlDB
    ports:
      - 3306:3306
    environment:
      - MYSQL_DATABASE=kppl
      - MYSQL_ROOT_PASSWORD=1234
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=admin
    volumes:
      - mysqlDB:/var/lib/mysql

  redisDB:
    image: redis
    restart: always
    ports:
      - 6379:6379
    container_name: redis_boot
    hostname: redis_boot
    volumes:
      - redisDB:/var/lib/redis


  medigo:
    image: dldydgns53/medigo
    restart: always
    container_name: server
    ports:
      - 8080:8080
    depends_on:
      - mysqlDB
      - redisDB

volumes:
  mysqlDB:
  redisDB:
```
- 인스턴스가 맛탱이 갔는데 이것도 테스트 해보자

## 참고 사이트
https://velog.io/@jkijki12/CICD%EB%A5%BC-%EA%B5%AC%EC%B6%95%ED%95%B4%EB%B3%B4%EC%9E%90

https://jistol.github.io/docker/2017/09/01/docker-redis/

https://basketdeveloper.tistory.com/85

https://hanseom.tistory.com/243?category=924749

https://jaehun2841.github.io/2018/12/01/2018-12-01-docker-6/#Redis-Cluster%EB%A5%BC-%EB%B9%8C%EB%93%9C%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-docker-compose-yml-%ED%8C%8C%EC%9D%BC