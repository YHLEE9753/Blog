# Jasypt 를 통한 정보 암호화
2022/07/14

## 1. 포스팅 이유
profile 설정을 통해 개발 환경을 분리해보자

## 2. Profile 이 무엇인가? + 코드적용
- Spring Profiles는 애플리케이션 설정을 특정 환경에서만 적용되게 하거나, 환경 별(local, test, production 등)로 다르게 적용할 때 사용된다.
- `@Component`, `@Configuration`, `@ConfigurationProperties` 에는 `@Profile` 이 마킹될 수 있어 특정 환경으로 제한 할 수 있다.
```java
@Configuration(proxyBeanMethods = false)
@Profile("production")
public class ProductionConfiguration {

    // ...

}
```
> 만약 automatic scanning 대신에 @ConfigurationProperties 이 @EnableConfigurationProperties 빈을 등록한다며
> @Profile 이 @EnableConfigurationProperties 을 가지는 @Configuration 클래스에도 명시가 될 필요가 있다.
> @ConfigurationProperties 이 스캔될 때, @Profile 는 @ConfigurationProperties 클래스에 지정할 수 있다.

## 3. yaml 파일 적용
- spring.profiles.active 를 통해 profile 를 지정할 수 있다.
```yaml
spring:
  profiles:
    active: "dev,hsqldb"
```
- 활성화된 profile 이 없으면 default profile 이 활성화된다.
- 다음과 같이 default 를 명시할 수도 있다.
```yaml
spring:
  profiles:
    default: "none"
```

- active 와 default 는 오직 non-profile specific documents 에만 적용될 수 있다.
- 이것은 spring.config.activate.on-profile 에 의해 [profile specific files](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files.profile-specific) 이나 [documents activated](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files.activation-properties) 에 포함될 수 없다는 의미이다.
- 정확히 무슨말인지는 잘 모르겠다... 
```yaml
# this document is valid
spring:
  profiles:
    active: "prod"
---
# this document is invalid
spring:
  config:
    activate:
      on-profile: "prod"
  profiles:
    active: "metrics"
```

## 4. Active Profiles 추가
- 때로는 profile 을 대체하는 것보다는 include 를 통해 profile 를 추가하는 것도 가능한다
```yaml
spring:
  profiles:
    include:
      - "common"
      - "local"


```

## 5. Active group
- 때로는 profile이 너무 세분화되어 사용하기 번거로운경우가 있다.
- 이런 경우 그룹화를 통해 논리적 이름을 정의할 수 있다.
```yaml
spring:
  profiles:
    group:
      production:
      - "proddb"
      - "prodmq"
```

## 6. 커맨더라인의 우선순위
- profile 은 커맨드 라인에서도 활성화 할 수 있다.
```
java -jar ... --spring.profiles.active=production
```
- 커맨드 라인는 설정데이터(.properties, yml) 보다 우선순위가 높은 PropertySource 이다
- 따라서 설정 데이터와 중복되는 속성을 override 한다.
- 즉 커맨드 라인에서 profile 을 활성화 하면, properties 와 yml 에서 acrive 한 profile 은 활성화되지 않는다.

## 7. application-{profile}
- profile 별로 설정데이터(yml) 파일을 만들어서 사용할 수 있다.
- 네이밍은 "application-{profile}"이다.
- profile 별 설정 데이터는 해당 profile 이 활성화 될 때에만 로딩된다.
- 추가로 위의 네이밍을 사용한 경우 자기 자신의 profile 을 지정하면 오류가 발생한다.
```yaml
# application-tiget.properties

spring.profiles.active=tiger # 오류발생
```
- profile 이 활성화되면 자동으로 해당 설정 데이터 파일이 로딩된다.

## 8. profile 활성화 순서
설정 데이터 파일이 적용되는 순서를 알아보자
- 만약 spring.profiles.active=production,test 로 profile 이 활성화된경우
1. 기본 설정 데이터 파일:application.yml
2. application-production.yml
3. application-test.yml
- 순서대로 설정이 적용된다.

- 모든 설정 파일에 같은 속성이 있다면, "application-test.yml" 에 설정된 속성이 앞선 설정 데이터 파일들을 override하여 적용된다.

## 9. 실제 적용 예시
### application.yml

```yaml
spring:
  profiles:
    active: local
    group:
      local: local, common
      prod: prod, common
```
### application-common.yml
```yaml
spring:
  application:
    name: hoon
```
### application-local.yml
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: url
    username: username
    password: password

  jpa:
    database: mysql
    hibernate:
      ddl-auto: create-drop
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect

  flyway:
    enabled: false

server:
  port: 8080

```

## 참고 사이트
https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.profiles

https://dev-j.tistory.com/9