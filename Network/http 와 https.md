# HTTP 와 HTTPS
2022/07/23

## 1. HTTP(Hyper Text Transfer Protocol)
- HTTP(Hyper Text Transfer Protocol)란 서버/클라이언트 모델을 따라 데이터를 주고 받기 위한 프로토콜이다.
- HTTP는 인터넷에서 하이퍼텍스트를 교환하기 위한 통신 규약으로, 80번 포트를 사용하고 있다. 따라서 HTTP 서버가 80번 포트에서 요청을 기다리고 있으며, 클라이언트는 80번 포트로 요청을 보내게 된다.
- HTTP는 1989년 팀 버너스 리(Tim Berners Lee)에 의해 처음 설계되었으며, WWW(World-Wide-Web) 기반에서 세계적인 정보를 공유하는데 큰 역할을 하였다.


### 구조
- HTTP는 애플리케이션 레벨의 프로토콜로 TCP/IP 위에서 작동한다.
- HTTP는 상태를 가지고 있지 않는 Stateless 프로토콜이며 
- Method, Path, Version, Headers, Body 등으로 구성된다.

![img.png](../images/http.png)

### 문제
- HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이였기 때문에, 
- HTTP로 비밀번호나 주민등록번호 등을 주고 받으면 제3자가 정보를 조회할 수 있었다. 
- 이러한 문제를 해결하기 위해 HTTPS가 등장하게 되었다.

## 2. HTTPS(Hyper Text Transfer Protocol Secure)

- HyperText Transfer Protocol over Secure Socket Layer, 
- HTTP over TLS, 
- HTTP over SSL, 
- HTTP Secure
- 으로 불리는 HTTPS는 HTTP에 데이터 암호화가 추가된 프로토콜이다
- HTTPS는 HTTP와 다르게 443번 포트를 사용하며, 
- 네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 암호화를 지원하고 있다.

## 3. 대칭키 암호화와 비대칭키 암호화
- HTTPS는 대칭키 암호화 방식과 비대칭키 암호화 방식을 모두 사용하고 있다.

### 대칭키 암호화
- 클라이언트와 서버가 동일한 키를 사용해 암호화/복호화를 진행함
- 키가 노출되면 매우 위험하지만 연산 속도가 빠름

### 비대칭키 암호화
- 1개의 쌍으로 구성된 공개키와 개인키를 암호화/복호화 하는데 사용함
- 키가 노출되어도 비교적 안전하지만 연산 속도가 느림









