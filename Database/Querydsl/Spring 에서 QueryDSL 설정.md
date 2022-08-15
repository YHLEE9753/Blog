# Spring gradle 에서 QueryDSL 설정
2022/08/16

# QueryDSL 설정
## 1. build.gradle
```groovy
// 1. queryDsl version 정보 추가
buildscript {
    ext {
        queryDslVersion = "5.0.0"
    }
}

plugins {
    id 'org.springframework.boot' version '2.6.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    // 2. querydsl plugins 추가
    id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
    id 'java'
}

//...

dependencies {
    // 3. querydsl dependencies 추가
    implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
    implementation "com.querydsl:querydsl-apt:${queryDslVersion}"
    //...
}

test {
    useJUnitPlatform()
}

/*
 * queryDSL 설정 추가
 */
// querydsl에서 사용할 경로 설정
def querydslDir = "$buildDir/generated/querydsl"
// JPA 사용 여부와 사용할 경로를 설정
querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}
// build 시 사용할 sourceSet 추가
sourceSets {
    main.java.srcDir querydslDir
}
// querydsl 컴파일시 사용할 옵션 설정
compileQuerydsl{
    options.annotationProcessorPath = configurations.querydsl
}
// querydsl 이 compileClassPath 를 상속하도록 설정
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
    querydsl.extendsFrom compileClasspath
}
```

## 2. 검증 엔티티
```java
package com.example.querydsltest;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import lombok.Getter;
import lombok.Setter;

@Entity
@Getter
@Setter
public class Hello {

	@Id
	@GeneratedValue
	private Long id;
}
```
- 검증용 Q타입 생성을 위해 `Gradle` -> `Tasks` -> `other` -> `compileQuerydsl을` 두번 클릭해준다.
- `build.generated.querydsl` 에 추가된것을 확인할 수 있다.
- 엔티티 변경시 해당 폴더를 제거하고 다시 `compile` 한다.

## 3. 테스트 코드로 테스트
```java
package com.example.querydsltest;

import static org.assertj.core.api.Assertions.*;

import javax.persistence.EntityManager;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import com.querydsl.jpa.impl.JPAQueryFactory;

@SpringBootTest
@Transactional
class HelloTest {
	@Autowired
	EntityManager em;

	@Test
	void test () {
		Hello hello = new Hello();
		em.persist(hello);

		JPAQueryFactory query = new JPAQueryFactory(em);
		QHello qHello  = new QHello("h");
		Hello result  = query
			.selectFrom(qHello )
			.fetchOne();


		assertThat(result).isEqualTo(hello);
		assertThat(result.getId()).isEqualTo(hello.getId());
	}
}
```

## 4. QueryDSL Config
```java
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.querydsl.jpa.impl.JPAQueryFactory;

@Configuration
public class QuerydslConfig {

	@PersistenceContext
	private EntityManager entityManager;

	@Bean
	public JPAQueryFactory jpaQueryFactory() {
		return new JPAQueryFactory(entityManager);
	}
}
```
- 이제 프로젝트 어디에서든 JPAQuery 주입받아 querydsl 사용 가능하다.
```java
@RequiredArgsConstructor
@Repository
public class ApplicationQueryRepository {

    private final JPAQueryFactory query;

    public List<Application> findApplications(Long userNo){
        QApplication qApplication = QApplication.application;

        List <Application> fetched = query
                .select(qApplication.application)
                .from(qApplication.application)
                .where(qApplication.application.user.userNo.eq(userNo))
                .fetch();

        return fetched;
    }
}
```

