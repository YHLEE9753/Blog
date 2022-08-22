# Sentry 를 이용한 로그 관리
2022/08/21

## 1. Sentry 
- 배포 환경에서 로깅과 모니터링은 매우 중요하다
- 에러 로깅과 모니터링을 동시에 할 수 있는 도구가 바로 Sentry 이다
- 에러 발생시 slack 등 알람을 쏴주는 것도 가능하고, 대부분의 언어 플랫폼을 지원한다.
- Spring 에 적용해 보자

## 2. Spring Boot 적용
```groovy
	// SENTRY
	implementation 'io.sentry:sentry-spring-boot-starter:6.3.0'
	implementation 'io.sentry:sentry-logback:6.3.0'
```
- 의존성을 추가한다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import io.sentry.spring.SentryTaskDecorator;

@Configuration
class SentryConfig {

	@Bean
	public SentryTaskDecorator sentryTaskDecorator() {
		return new SentryTaskDecorator();
	}
}
```
- Sentry 공식문서를 살펴보면 스프링부트의 경우 javax.servlet.ServletContainerInitializer를 자동으로 로드하지 않는다고 한다.
- 그 이유는, war 내부에서 실행되는 써드 파티 라이브러리가 스프링 부트 애플리케이션을 종료시키는 위험을 줄이기 위해 의도적으로 디자인되었기 때문이다
- 이를 해결하기 위해 Sentry 또한 io.sentry.spring.SentryServletContextInitializer를 빈으로 등록해서 사용하고 있다.
  - io.sentry.spring.SentryServletContextInitializer의 경우 이름에도 알 수 있듯이, ServletContextInitializer의 구현체이다.
- custom 하지 않는다면 SentryTaskDecorator 를 bean 등록하는 걸로 충분하다

```yaml
sentry:
  enabled: true
  dsn: <uri 주소>
```
- 이제 해당 설정을 프로퍼티에 명시해준다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
  <include resource="org/springframework/boot/logging/logback/console-appender.xml" />

  <property resource="application-prod.yml" />
  <appender name="SENTRY" class="io.sentry.logback.SentryAppender">
    <options>
      <dsn>${DSN}</dsn>
    </options>
    <minimumEventLevel>INFO</minimumEventLevel>
    <minimumBreadcrumbLevel>DEBUG</minimumBreadcrumbLevel>
  </appender>

  <logger name="org.hibernate.SQL" additivity="false">
    <level value = "DEBUG" />
    <appender-ref ref="SENTRY" />
  </logger>

  <root level="info">
    <appender-ref ref="CONSOLE" />
    <appender-ref ref="SENTRY" />
  </root>
</configuration>
```
- 그 후 `main/resources/` 에 `logback-spring.xml` 파일로 `log` 설정을 명시한다.
- error 이상의 log 가 sentry 에 찍히는 것을 확인할 수 있다.
- info 수준으로 log 가 찍히는 것을 확인할 수 있다.

## 참고 사이트
공식문서 : https://docs.sentry.io/platforms/java/guides/spring-boot/async/

bean 등록 : https://minkukjo.github.io/devops/2020/06/10/Infra-21/


