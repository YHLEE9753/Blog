# Yong Blog
[![Since](https://img.shields.io/badge/since-2022.07.12-333333.svg?style=flat-square)](https://github.com/NKLCWDT)
[![author](https://img.shields.io/badge/author-LeeYongHoon-0066FF.svg?style=flat-square)](https://github.com/NKLCWDT)

🔥 공부한 내용을 항상 기록합니다.<br>
🌈 프로젝트나 코드 작성을 하면서 들었던 의문점에 대해 해결합니다.<br>
📚 프로그래밍 도서 내용을 정리합니다.

## ⚙ Table
1. [Book](#-book)<br>
2. [Architecture](#-architecture)<br>
3. [Network](#-network)<br>
4. [Operating System](#-operating-system)<br>
5. [Database](#-database)<br>
6. [Querydsl](#-querydsl)<br>
7. [Dev](#-dev)<br>
8. [Spring](#-spring)<br>
9. [Git](#-git)<br>
10. [AWS](#-aws)<br>
11. [Java](#-java)<br>
12. [Question](#-question)<br>
13. [Other](#-other)<br>

## 📝 Book
### 읽은 책
오브젝트<br>
이펙티브 자바 - [스터디](https://github.com/Dev-Prison/Effective-Java)<br>
객체지향의 사실과 오해<br>
엘레강트 오브젝트<br>

### 읽을 책
클린코드<br>
만들면서 배우는 클린 아키텍처<br>
Real MySQL<br>
친절한 SQL 튜닝<br>
대규모 서비스를 지탱하는 기술<br>
IT 엔지니어를 위한 네트워크 입문<br>

## 📝 Architecture
### Layered Architecture 
[왜 layer 마다 dto 와 converter 를 두어야 할까?](https://github.com/YHLEE9753/Blog/blob/main/Architecture/%EC%99%9C%20layer%20%EB%A7%88%EB%8B%A4%20dto%20%EC%99%80%20converter%20%EB%A5%BC%20%EB%91%90%EC%96%B4%EC%95%BC%20%ED%95%A0%EA%B9%8C.md)<br>
[controller 와 service 에서 의존성 관리는 누구의 책임일까?](https://github.com/YHLEE9753/Blog/blob/main/Architecture/controller%20%EC%99%80%20service%20%EC%97%90%EC%84%9C%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EA%B4%80%EB%A6%AC%EB%8A%94%20%EB%88%84%EA%B5%AC%EC%9D%98%20%EC%B1%85%EC%9E%84%EC%9D%BC%EA%B9%8C.md)<br>
[조회로직은 전부 만들어야 할까?](https://github.com/YHLEE9753/Blog/blob/main/Architecture/%EC%A1%B0%ED%9A%8C%EB%A1%9C%EC%A7%81%EC%9D%80%20%EC%A0%84%EB%B6%80%20%EB%A7%8C%EB%93%A4%EC%96%B4%EC%95%BC%20%ED%95%A0%EA%B9%8C.md)<br>

## 📝 Network
### 이론
[OSI 7계층, TCP/IP 4계층](https://github.com/YHLEE9753/Blog/blob/main/Network/OSI%207%EA%B3%84%EC%B8%B5%20TCP%20IP%204%EA%B3%84%EC%B8%B5.md)<br>
[TCP와 UDP의 비교](https://github.com/YHLEE9753/Blog/blob/main/Network/TCP%EC%99%80%20UDP%EC%9D%98%20%EB%B9%84%EA%B5%90.md)<br>
[TCP 흐름제어 혼합제어(Gyoogle 출처)](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%20(%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4).md#tcp-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4)<br>
[HTTP 와 HTTPS](https://github.com/YHLEE9753/Blog/blob/main/Network/http%20%EC%99%80%20https.md)<br>
[HTTPS 보안 알고리즘](https://github.com/YHLEE9753/Blog/blob/main/Network/https%20%EB%B3%B4%EC%95%88%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98.md)<br>
[SSL and TLS](https://github.com/YHLEE9753/Blog/blob/main/Network/TLS%20and%20SSL.md)<br>

### 개발
[CORS 란?](https://github.com/YHLEE9753/Blog/blob/main/Network/CORS%20%EB%9E%80.md)<br>
[get post put delete patch](https://github.com/YHLEE9753/Blog/blob/main/Network/get%20post%20put%20delete%20patch.md)<br>
[RESTful한 URL(7원칙)](https://github.com/YHLEE9753/Blog/blob/main/Network/RESTful%ED%95%9C%20URL.md)<br>
URL 과 URI 의 차이<br>

### 면접질문
[Network 면접질문](https://github.com/YHLEE9753/Blog/blob/main/Network/Network%20%EB%A9%B4%EC%A0%91%EC%A7%88%EB%AC%B8.md)<br>

## 📝 Operating System

## 📝 Database
### SQL
inner join, outer join, in<br>
[언제 비관적락(pessimistic lock)을 사용하고 언제 낙관적락(optimistic lock)을 사용할까?](https://github.com/YHLEE9753/Blog/blob/main/Database/%EC%96%B8%EC%A0%9C%20%EB%B9%84%EA%B4%80%EC%A0%81%EB%9D%BD(pessimistic%20lock)%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EA%B3%A0%20%EC%96%B8%EC%A0%9C%20%EB%82%99%EA%B4%80%EC%A0%81%EB%9D%BD(optimistic%20lock)%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C.md)<br>

### Redis
[In-memory DB란?]<br>
[Redis 의 특징, 장단점, 사용처]<br>
[Redis 와 Memcached 의 차이]<br>

## 📝 JPA
페치 조인<br>
update 에서 bulk 연산<br>
[속성 컨텍스트(Persistence Context) - 엔티티 생명주기와 기능들](https://github.com/YHLEE9753/Blog/blob/main/Database/%EC%86%8D%EC%84%B1%20%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8(Persistence%20Context)%20-%20%EC%97%94%ED%8B%B0%ED%8B%B0%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%EC%99%80%20%EA%B8%B0%EB%8A%A5%EB%93%A4.md)<br>
[OneToOne 관계는 과연 지연로딩이 되는가?](https://github.com/YHLEE9753/Blog/blob/main/Database/OneToOne%20%EA%B4%80%EA%B3%84%EB%8A%94%20%EA%B3%BC%EC%97%B0%20%EC%A7%80%EC%97%B0%EB%A1%9C%EB%94%A9%EC%9D%B4%20%EB%90%98%EB%8A%94%EA%B0%80.md)<br>
[JPA N+1 문제]<br>
[Offset & Cursor]<br>

## 📝 Querydsl
[Spring gradle 에서 QueryDSL 설정](https://github.com/YHLEE9753/Blog/blob/main/Database/Querydsl/JPA%20Repository%20%EC%84%A4%EC%A0%95.md)<br>
[JpaRepository 를 사용하면서 QueryDSL 적용](https://github.com/YHLEE9753/Blog/blob/main/Database/Querydsl/JPA%20Repository%20%EC%84%A4%EC%A0%95.md)<br>
[QueryDsl 기본](https://www.notion.so/yhlee9753/Querydsl-1-0f5b10c23ea749b68ef9488131f1e09d)<br>
[fetchResults fetchCount depreciated](https://github.com/YHLEE9753/Blog/blob/main/Database/Querydsl/fetchResults%20fetchCount%20depreciated.md)<br>
[Querydsl Projection 방법]<br>


## 📝 Dev
[Jasypt 를 통한 정보 암호화](https://github.com/YHLEE9753/Blog/blob/main/Dev/Jasypt%20%EB%A5%BC%20%ED%86%B5%ED%95%9C%20%EC%A0%95%EB%B3%B4%20%EC%95%94%ED%98%B8%ED%99%94.md)<br>
Flyway 를 사용한 데이터베이스 형상관리<br>

## 📝 Spring
### Spring 이해
Web Server와 WAS(Web Application Server)에 대하여<br>
[서블릿(Servlet) 이란?](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EC%9D%B4%ED%95%B4/%EC%84%9C%EB%B8%94%EB%A6%BF(Servlet)%20%EC%9D%B4%EB%9E%80.md) <br>
[Dispatcher-Servlet(디스패처 서블릿)이란?](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EC%9D%B4%ED%95%B4/Dispatcher-Servlet(%EB%94%94%EC%8A%A4%ED%8C%A8%EC%B2%98%20%EC%84%9C%EB%B8%94%EB%A6%BF)%EC%9D%B4%EB%9E%80.md) <br>
[Filter, Interceptor 차이](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EC%9D%B4%ED%95%B4/Filter%2C%20Interceptor%20%EC%B0%A8%EC%9D%B4.md)<br>
필터(Filter) 가 스프링 빈 등록과 주입이 가능한 이유<br>
Spring 의 AOP 프록시 구현 방법<br>

### Spring 기능
[Spring 에서 CORS 설정하기](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EA%B8%B0%EB%8A%A5/Spring%20%EC%97%90%EC%84%9C%20CORS%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.md)<br>
[Spring profile](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EA%B8%B0%EB%8A%A5/Spring%20profile.md)<br>
[Spring h2 설정](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EA%B8%B0%EB%8A%A5/%EA%B8%B0%EB%8A%A5/h2%20%EC%84%A4%EC%A0%95.md)<br>
[JpaAuditConfig 에서 Authentication Principle 적용하기](https://github.com/YHLEE9753/Blog/blob/main/Spring/%EA%B8%B0%EB%8A%A5/JpaAuditConfig%20%EC%97%90%EC%84%9C%20Authentication%20Principle%20%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0.md)<br>
[RestControllerAdvice 와 error code 를 활용한 error handling]<br>
[Yaml 설정 바인딩]<br>
Logback 설정<br

### Security
[Spring Security - Lambda DSL](https://github.com/YHLEE9753/Blog/blob/main/Spring/Security/Spring%20Security%20-%20Lambda%20DSL.md)<br>
[depreciated 된 WebSecurityConfigurerAdapter 수정하기](https://github.com/YHLEE9753/Blog/blob/main/Spring/Security/depreciated%20%EB%90%9C%20WebSecurityConfigurerAdapter%20%EC%88%98%EC%A0%95%ED%95%98%EA%B8%B0.md)<br>
[OncePerRequestFilter와 Filter의 차이](https://github.com/YHLEE9753/Blog/blob/main/Spring/Security/OncePerRequestFilter%EC%99%80%20Filter%EC%9D%98%20%EC%B0%A8%EC%9D%B4.md)<br>
[AuthenticationSuccessHandler 로그인 성공 후 특정 url redirect 방법 : RedirectStrategy](https://github.com/YHLEE9753/Blog/blob/main/Spring/Security/AuthenticationSuccessHandler%20%EB%A1%9C%EA%B7%B8%EC%9D%B8%20%EC%84%B1%EA%B3%B5%20%ED%9B%84%20%ED%8A%B9%EC%A0%95%20url%20redirect%20%EB%B0%A9%EB%B2%95%20%20RedirectStrategy.md)<br>
Filter 와 Interceptor 의 차이<br>
Filter 에서 예외처리하기<br>
Spring Security 에서 Cors 설정하기<br>

### OAuth2
[JWT 인증의 의미](https://github.com/YHLEE9753/Blog/blob/main/Spring/OAuth2/JWT%20%EC%9D%B8%EC%A6%9D%EC%9D%98%20%EC%9D%98%EB%AF%B8.md)<br>
[AccessToken과 RefreshToken의 특징과 위치](https://github.com/YHLEE9753/Blog/blob/main/Spring/OAuth2/AccessToken%EA%B3%BC%20RefreshToken%EC%9D%98%20%ED%8A%B9%EC%A7%95%EA%B3%BC%20%EC%9C%84%EC%B9%98.md)<br>
[redis 활용 - 추가 정리 필요](https://github.com/YHLEE9753/Blog/blob/main/Spring/OAuth2/redis%20%ED%99%9C%EC%9A%A9.md)<br>
[JWT 활용 시 Bcrypt 를 사용하는 이유]<br>
[JWT 구조]<br>
[OAuth flow]<br>
[Spring token 관리구현1(RefreshToken:redis, AccessToken:front 관리)]<br>
[Spring token 관리구현2(RefreshToken:redis, AccessToken:Cookie 사용)]<br>

### Spring 활용
[유니크 제약 조건 예외처리](https://github.com/YHLEE9753/Blog/blob/main/Spring/%ED%99%9C%EC%9A%A9/%EC%9C%A0%EB%8B%88%ED%81%AC%20%EC%A0%9C%EC%95%BD%20%EC%A1%B0%EA%B1%B4%20%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.md)<br>
[Spring Boot S3 파일 업로드하기]<bR>

### Test & Document
[Mockito 와 BDDMockito 는 어떻게 구분하여 사용해야 할까?](https://github.com/YHLEE9753/Blog/blob/main/Spring/Test/Mockito%20%EC%99%80%20BDDMockito%20%EB%8A%94%20%EC%96%B4%EB%96%BB%EA%B2%8C%20%EA%B5%AC%EB%B6%84%ED%95%98%EC%97%AC%20%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC%20%ED%95%A0%EA%B9%8C.md)<br>
[Spring RestDocs 와 Swagger 의 장단점]<br>
[F.I.R.S.T 원칙]<br>
[통합 테스트와 단위 테스트]<br>
@nested<br>


## 📝 Git
[git submodule](https://github.com/YHLEE9753/Blog/blob/main/Git/git%20submodule.md)<br>
[git rebase]<br>
git 전략(main 1개만 쓰는 거랑 dev 를 활용하는 차이)<br>
git merge 전략(기본 merge, rebase, squash 차이)<br>

## 📝 AWS
### 기본
[리눅스 명령어](https://github.com/YHLEE9753/Blog/blob/main/AWS/%EB%A6%AC%EB%88%85%EC%8A%A4%20%EB%AA%85%EB%A0%B9%EC%96%B4.md)<br>
[EC2 생성, 탄력적IP, 네트워크 인터페이스, vi, shell](https://github.com/YHLEE9753/Blog/blob/main/AWS/EC2%20%EC%83%9D%EC%84%B1%2C%20%ED%83%84%EB%A0%A5%EC%A0%81IP%2C%20%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%2C%20vi%2C%20shell.md)<br>
[RDS, S3](https://github.com/YHLEE9753/Blog/blob/main/AWS/RDS%2C%20S3.md)<br>
[CloudWatch, ELB, lambda](https://github.com/YHLEE9753/Blog/blob/main/AWS/CloudWatch%2C%20ELB%2C%20lambda.md)<br>
[CI/CD, Elastic-beanstalk](https://github.com/YHLEE9753/Blog/blob/main/AWS/CI%20CD%2C%20Elastic-beanstalk.md)<br>
[Docker, IAM, VPC](https://github.com/YHLEE9753/Blog/blob/main/AWS/Dokcer%2C%20VPC%2C%20IAM.md)<br>
[CI/CD 에시, Document DB, DevOps](https://github.com/YHLEE9753/Blog/blob/main/AWS/Document%20DB%2C%20CI%20CD%20%EC%97%90%EC%8B%9C.md)<br>

### CI CD
[AWS CodeDeploy 와 GitHub Action 을 이용한 CICD]<br>
Docker 와 GitHub Action 을 이용한 CICD<br>
[Let's Encrypt 를 이용한 https 적용하기]<br>

## 📝 Java
[Checked Exception, UnChecked Exception](https://github.com/YHLEE9753/Blog/blob/main/Java/Checked%20Exception%2C%20UnChecked%20Exception.md)<br>
[Generics(1) - 서브클래싱(SubClassing)과 서브타이핑(SubTyping)](https://github.com/YHLEE9753/Blog/blob/main/Java/Generics(1)%20-%20%EC%84%9C%EB%B8%8C%ED%81%B4%EB%9E%98%EC%8B%B1(SubClassing)%EA%B3%BC%20%EC%84%9C%EB%B8%8C%ED%83%80%EC%9D%B4%ED%95%91(SubTyping).md)<br>
[Generics(2) - 공변(Contravariace)과 반공변(Covariance)](https://github.com/YHLEE9753/Blog/blob/main/Java/Generics(2)%20-%20%EA%B3%B5%EB%B3%80(Contravariace)%EA%B3%BC%20%EB%B0%98%EA%B3%B5%EB%B3%80(Covariance).md)<br>
[Generics(3) - Generic 타입과 네이밍](https://github.com/YHLEE9753/Blog/blob/main/Java/Generics(3)%20-%20Generic%20%ED%83%80%EC%9E%85%EA%B3%BC%20%EB%84%A4%EC%9D%B4%EB%B0%8D.md)<br>
[Generics(4) - Raw Type 과 type inference](https://github.com/YHLEE9753/Blog/blob/main/Java/Generics(4)%20-%20Raw%20Type%20%EA%B3%BC%20type%20inference.md)<br>
[Generics(5) - 사용 방식](https://github.com/YHLEE9753/Blog/blob/main/Java/Generics(5)%20-%20%EC%82%AC%EC%9A%A9%20%EB%B0%A9%EC%8B%9D.md)<br>
[record 클래스와 sealed 클래스와 interface(1) - record 클래스](https://github.com/YHLEE9753/Blog/blob/main/Java/record%20%ED%81%B4%EB%9E%98%EC%8A%A4.md)<br>
[record 클래스와 sealed 클래스와 interface(2) - sealed 클래스와 interface](https://github.com/YHLEE9753/Blog/blob/main/Java/sealed%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20interface.md)<br>


## 📝 Question
라이브러리와 프레임워크의 차이<br>
상황에 맞는 get, post, put, patch, delete 사용과 경로<br>
Url 단축<br>

## 📝 Other
Jira