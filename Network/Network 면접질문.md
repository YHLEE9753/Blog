# Network 면접 질문
2022/07/15

## Table
예상질문 | 답안

# 1. 예상질문
### 1일차
1. OSI, TCP/IP
- OSI 7계층에 대해 소개해주세요
- TCP/IP 4계층에 대해 소개해주세요

2. TC/IP
- TCP/IP 데이터 흐름에 대해서 소개해주세요 

3. TCP UDP 차이
- TCP 와 UDP 의 차이점은 무엇인가요?
- 3 way handshake 에 대해서 소개하고 어떤 방식으로 이루어지는지 이야기해주세요
- 4 way handshake 에 대해서 소개하고 어떤 방식으로 이루어지는지 이야기 해주세요
- 만약 server 에서 Fin 을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 경우가 발생한다면 어떻게 될까요?

- 혼합제어가 왜 필요하고 어떤방식으로 이루어지나요?
- 흐름제어가 왜 필요하고 어떤방식으로 이루어지나요?

4. http
- get 과 post 의 차이점은 무엇인가요?

5. 추가 질문
- 현재 피시방에 있는데 게임을 하던도중 게임을 실행할 수 없게 되었다.
- 어느 계층에 문제가생긴것일까?

- 양방향 통신을 보장하기위해 2웨이가 아닌 3웨이 왜냐면 기본적으로 tcp 가 양방향
- 데이터 전송시 IP 만해도되는데 tcp 까지 쓴느이유는?
- DNS 서버 도 UDP 다

### 2일차

6. http 와 https 포트 넘버가 무엇인가요?
7. http 와 https 의 차이는 무엇인가요?
8. http 는 stateless 프로토콜이라고 하는데 stateless 의 특징이 무엇인가요?
9. https 연결과정을 말씀해 주세요
10. 인증서와 인증기관이 왜 필요할까요?
    1. 중간자 공격
       Man in the middle attack
       공격자가 통신 객체가 전송하는 공개 키를 자신의 공개 키로 바꾸는 것
11. 연결과정에서 인증서가 사용되는데 인증서와 기관에 대해 말씀해주시겠어요?
12. 인증서(public key)는 중간에 탈취과정 혹은 위변조등에 의해 데이터 손상이 발생할 수 있는데 이를 방지하기 위해 사용되는 보안 알고리즘이 무엇인지 아시나요?
    1.  인증된 CA 기관의 인증서가 아닐 경우에는 브라우저가 어떻게 동작하나요?
13. 전자 서명에 대해 말씀해 주시겠어요?
14. 공개 키 암호화 즉 비대칭키 알고리즘에 대해서 말씀해주시겠어요?
15. 공개키는 최대한 노출되는것이 좋을까요? 이유는 무엇인가요?
16. SSL 과 TSL 의 차이는 무엇인가요?
17. TSL 의 핸드세이크 프로토콜에 대해서 말씀해 주시겠어요?

18. 추가질문
- http 버전은 무엇인가?
- http 3 버전도 존재하는데 특징을 아시나요? - udp 사용
- http 특징인 비연결성에 대해 말씀해주세요
- SSL 가 TSL 도 프로토콜이라고 불리긴한다.
- SMPT 메일보내는 프로토콜, FTP 파일보내는 프로토콜 에도 SSL 과 TSL 이 적용가능하다.
- https 는 무조건 안전한것은 아니다 이유가 무엇일까?
  - ca 기업들이 여러개가 있는데, 신뢰받은 인증서 사용은 괜찮지만
  - 신뢰받지 못한 ca 기업들의 경우 안전성을 보장할 수 없다.

### 3일차
1. 로드밸런싱이란 무엇인가요?
2. 로드 밸런싱 알고리즘 중 라운드 로빈 방식에 대해 설명해주세요 - 장단점은 무엇인가요?
3. IP 해시 방식에 대해서 설명해 주세요
4. L4 로드 밸런서와 L7 로드 밸런서의 차이점을 설명해 주세요
5. 혹시 프로젝트 중에 로드밸런서를 활용한 경험이 있을까요?
6. DNS 는 무엇인가요?
7. 혹시 google.com 을 쳤을때 발생하는 네트워크절차를 설명하실수 있으신가요?
8. DNS 라운드 로빈은 무엇인가요? 
9. 기존 로드밸런싱이 아닌 DNS 라운드 로빈을 사용하는 이유는 무엇인가요>
10. DNS 라운드 로빈의 단점은 무엇인가요?
11. 개선 방법은 무엇이있을까요?
12. Blocking 과 Non Blocking 의 차이는 무엇인가요?
13. Sync 와 Async 의 차이는 무엇인가요?
14. Blacking Sync 와 Non Blocking Async 를 설명해 주시겠어요?

----
- 로드밸런서가 장애가 난경우를 대비하여 어떻게 대응하면 좋을까?
  - 마스터 슬래이브 같이 이중화를 통해 대비(로드밸런서를 2개 이상 둔다) -> 추가적으로 알아보자
- Blocking Non Blocking 제어권


---
## 2. 답안
1. OSI, TCP/IP
- OSI 7계층에 대해 소개해주세요
- TCP/IP 4계층에 대해 소개해주세요

어플리케이션 계층, 전송계층, 인터넷계층, 네트워크 연결 계층
어플리케이션 계층 : 
실제 서비스를 제공하는 계층으로 데이터를 교환하기 위한 다양한 프로토콜이 존재한다(HTTP, HTTPS, DNS)
전송 계층 : 
어플리케이션 계층과 인터넷 계층 사이의 데이터 전달 중계 역할
대표적으로 UDP 와 TCP 가 존재하고 전달받은데이터를 전송에 알맞은 형태인 세그먼트 단위로 나눠서 전송합니다.
TCP 헤더가 추가된게 세그먼트이고 UDP 헤드가 추가된게 데이터그램이다.
인터넷 계층:
기존 세그먼트나 데이터 그램에 IP 헤더가 추가된 packet 을 데이터 단위로 가지며, 패킷을 IP 주소로 지정된 목적지로 전송하는 계층이다.
네트워크 연결 계층:
네트워크 장비간에 데이터를 전송하는 역할을 수행. 최종적으로 전송될 목적지를 식별하기 위해 컴퓨터의 고유 물리적 주소인 MAC 주소를 사용 

3. TCP UDP 차이
- TCP 와 UDP 의 차이점은 무엇인가요?

전송 계층에 사용된느 프로토콜로써 둘다 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달하는 기능
중요 차이점은 TCP 는 client 와 server 가 데이터를 주고 받지만 UDP 는 일반적으로 데이터를 전송받는것을 확인할수 있다.
데이터를 주고 받는 TCP 는 패킷의 순서와 안정적인 전송을 보장하는 신뢰성있는 연결이지만 UDP 는 보장하지 않는다.
UDP 는 빠른 속도로 데이터를 전송하는데 적합하다 동영상 전송의 경우 1,2개의 프레임이 없어도 문제가 안되며 오히려 실시간으로 빠르게 데이터 전송을 하는것이 더 중요하다
TCP 는 가상회선 패킷 교환 방식을 이용하는데 데이터 전송 이전에 논리적인 연결을 하고, 최초 setup 을 통해 경로를 설정한다.
UDP 는 각 라우터에서 최적의 경로를 찾아서 패킷이 독립적으로 전달된다.

- 3 way handshake 에 대해서 소개하고 어떤 방식으로 이루어지는지 이야기해주세요

데이터 전송이전에 통신여부를 확인하기 위한 세션 수립
syn connection 을 연결할 떄 사용
FIn connection 을 닫을 때 사용
Ack 수신자가 수신한 데이터를 전송자에게 다시 전송하면서 제어할 때 사용

Client > Server : TCP SYN
Server > Client : TCP SYN ACK
Client > Server : TCP ACK

- 4 way handshake 에 대해서 소개하고 어떤 방식으로 이루어지는지 이야기 해주세요

연결 해제를
->FIN
ACK<-
FIN<-
->ACK
이떄 Fin 을 받게 되면 무조건 time wait 한다(아래 질문)

- 만약 server 에서 Fin 을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 경우가 발생한다면 어떻게 될까요?

- 흐름제어가 왜 필요하고 어떤방식으로 이루어지나요?

받는 쪽이 보내는 쪽보다 데이터 처리속도가 빠르면 문제 없지만, 보내느 쪽의 속도가 더 빠른 경우
받는 쪽의 데이터받는 저장 용량을 초과하여 데이터 손실이 일어날 수 있고
이런 손실이 잦아지면 불필요하게 응답과 데이터 재전송이 빈번하게 일어난다.

1. stop and wait
매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

2. Sliding Window

수신하는 쪽에서 설정한 윈도우 크기만큼 송신측에서 세그먼트를 전송할 수 있게 데이터 흐름을 동적으로 조절
송 수신은 각각 window 를 가지는데 3way 를 통해  서로의 size 를 맞추게 된다.
수신 윈도우 >= 송신 윈도우
현재 전송중인 패킷 수 <= 송신 윈도우
수신 윈도우는 송신 쪽으로부터 데이터를 받았다는 flag 인 ACK 를 받으면 동적으로 윈도우를 옮긴다.


- 혼합제어가 왜 필요하고 어떤방식으로 이루어지나요?

한 라우터에 데이터가 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 된다. 
이런 경우 호스트들은 또 다시 재전송을 하게되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실을 발생시키게 된다.
네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송속도를 강제로 줄인다

1. AIMD(Additive Increase / Multiplicative Decrease)
처음에 패킷을 하나씩 보내고 이것이 문제없이 도착하면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가시켜가며 전송하는 방법
패킷 전송에 실패하거나 일정 시간을 넘으면 패킷의 보내는 속도를 절반으로 줄인다.
문제점 : 초기에 높은 대역폭을 사용하지 못하여 오랜시간이걸리며 혼잡 상황을 미리 감지하지 못한다.

2. slow start
AIMD 와 달리 전송속도를 지수 함수꼴로 증가.
한번 혼잡 현상이 발생하면 절반으로 감소(Fast Recovery - 단 혼잡 상황을 1번 겪으면 그후 는 AIMD 사용)
네트워크 수용량을 예측가능
Timeout 이후 혼잡 현상이 발생한 size 의 절반까지는 지수함수꼴로 증가 -> 그 후는 원만하게 1씩 증가

3. Fast Retransmit
재전송 타이머 값이 종종 상대적으로 길어지므로, 손실된 패킷의 재전송 전에 지연시간이 커진다.
송신측에서 바로바로 여러 개의 세그먼트를 전송할 경우, 
세그먼트가 손실되면 수신측에서는 중복 ACK를 보내게 되는데, 
타임아웃 전에 송신측에서 중복 ACK를 3번받게 되면 세그먼트를 즉시 전송한다. 
즉, 수신측이 기다리는 순서번호의 세그먼트보다 큰 순서번호의 세그먼트가 3개 도착할 경우를 의미한다.

4. http
- get 과 post 의 차이점은 무엇인가요?

GET 은 클라이언트에서 서버로 어떠한 리소스로 부터 정보를 요청하기 위해 사용되는 메서드이다.
GET을 통한 요청은 URL 주소 끝에 파라미터로 포함되어 전송

GET 요청은 캐시가 가능하다.
: GET을 통해 서버에 리소스를 요청할 때 웹 캐시가 요청을 가로채 서버로부터 리소스를 다시 다운로드하는 대신 리소스의 복사본을 반환한다. HTTP 헤더에서 cache-control 헤더를 통해 캐시 옵션을 지정할 수 있다.


GET 요청은 브라우저 히스토리에 남는다.

GET 요청은 북마크 될 수 있다.

GET 요청은 길이 제한이 있다.
: GET 요청의 길이 제한은 표준이 따로 있는건 아니고 브라우저마다 제한이 다르다고 한다.

GET 요청은 중요한 정보를 다루면 안된다. ( 보안 )
: GET 요청은 파라미터에 다 노출되어 버리기 때문에 최소한의 보안 의식이라 생각하자.

GET은 데이터를 요청할때만 사용 된다.

POST는 클라이언트에서 서버로 리소스를 생성하거나 업데이트하기 위해 데이터를 보낼 때 사용 되는 메서드다.
POST는 전송할 데이터를 HTTP 메시지 body 부분에 담아서 서버로 보낸다
하지만 데이터를 암호화하지 않으면 body의 데이터도 결국 볼 수 있는건 똑같다

POST 요청은 캐시되지 않는다.

POST 요청은 브라우저 히스토리에 남지 않는다.

POST 요청은 북마크 되지 않는다.

POST 요청은 데이터 길이에 제한이 없다


멱등성 (idempotent) : GET 요청은 멱등이며, POST는 멱등이 아니다.
멱등의 사전적 정의는 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미한다.
GET은 리소스를 조회한다는 점에서 여러 번 요청하더라도 응답이 똑같을 것 이다. 
반대로 POST는 리소스를 새로 생성하거나 업데이트할 때 사용되기 때문에 멱등이 아니라고 볼 수 있다.