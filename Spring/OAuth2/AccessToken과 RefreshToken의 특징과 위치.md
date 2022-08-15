# AccessToken과 RefreshToken의 특징과 위치
2022/07/18

## 1. 포스팅 이유
stateless 하게 서버를 유지하면서 refresh token 을 활용하는것은 사실상 불가능하다 그렇다면 accessToken 과 refreshtoken 을 어떻게 관리하는 것이 성능과 보안 2개를 잡을 수 있을까?

## 2. Access TOken
- 토큰에 사용자 정보를 식별할 수 있는 정보를 담아 놓고, 인증이 필요할 때 이 JWT 토큰을 사용하는 방식이다.
- 서버는 AccessToken을 데이터베이스나 파일등에 저장 할 필요 없이 메모리상에서 미리 정의 된 비밀키를 이용해 비교하는 것 만으로 인증을 처리하기 때문에 추가적인 I/O 작업이 필요가 없다
- 반면에 그런 이유로 서버는 특정 사용자의 접속을 강제로 만료시키기 어렵다
- 일반적으로 클라이언트는 스스로의 저장 공간에서 토큰을 삭제하는 방법을 사용해 사용자의 접근을 막는다.

### 설정
- 짧은 만료 시간을 설정
- 기기나 AccessToken이 탈취되더라도 빠르게 만료
- 사용자는 자주 로그인을 해야 한다. 이를 위해 refresh Token 도입

### 저장 위치
1. Cookie
- https only 와 secure 설정시 javascript 를 통한 접근을 막을 수 있다.
- CORS 설정을 통해 정해진 url 로만 접근을 설정하면 CSRF 공격을 막을 수 있다.
- 따라서 Cookie 에 저장하는것이 가장 안전하다

2. local storage
- javascript 로 접근 가능하므로 짧은 만료시간 설정이라해도 보안적으로 취약하다

## 3. Refresh Token 
- 사용자가 로그인을 할 때에 AccessToken과 함께 그에 비해 긴 만료 시간을 갖는 RefreshToken을 클라이언트에 함께 발급합니다. 
- 주로 AccessToken은 30분 내외, RefreshToken은 2주에서 한달 정도의 만료 기간을 부여합니다.
- 클라이언트는 AccessToken이 만료되었다는 오류를 받으면 따로 저장해두었던 RefreshToken을 이용하여 AccessToken의 재발급을 요청합니다.
- 서버는 유효한 RefreshToken으로 요청이 들어오면 새로운 AccessToken을 발급하고, 만료된 RefreshToken으로 요청이 들어오면 오류를 반환해, 사용자에게 로그인을 요구합니다.
- AccessToken은 서버에 따로 저장해 둘 필요가 없지만, RefreshToken의 경우 서버의 stroage에 따로 저장해서 이후 검증에 활용해야 합니다.
- 그러므로 RefreshToken을 이용한다는 것은 추가적인 I/O 작업이 필요하다는 의미이며, 
- 이는 I/O 작업이 필요없는 빠른 인증 처리를 장점으로 내세우는 JWT의 스펙에 포함되지 않는 부가적인 기술입니다.

### 문제
- 기존의 JWT 방식은 서버를 stateless 하게 만드는 것인데 refresh token 은 만료기간이 길어 클라이언트에서 저장하게 두면 안된다.
- 따라서 서버에서 관리하는 것이 좋은데 그렇게 되면 결국 서버에 별도릐 storage 를 만들어야 한다.
- 결국 db 접속으로 인한 서버 부하 문제로 이어질 수 있다.

### 저장 위치
1. Server DB
- 부하를 감당할 수 없다
- 오버헤드(비용)이 크다

2. Redis
- 빠르다. 레디스는 주기억장치에 데이터를 저장하므로 db 보다 성능이 좋다
- 전원이 꺼지면 데이터가 날라간다. 자주쓰고 날아가도 괜찮은 정보를 여기에 저장한다면 성능적으로 좋다
- 위 2가지 특징이 토큰을 저장하기에 최적의 db 라는 이유이다.

## 3. 로그아웃(BlackList 기법)
- 로그아웃이 요청된 AccessToke 은 Redis 저장소에 넣어 접근을 통제한다.
- Redis 저장소에 로그아웃이 요청된 AccessToken을 남은 유효시간을 계산하여 넣어 관리하는 것이다.
- 매 권한 API 요청마다 Redis 저장소 내 해당 AccessToken의 존재여부를 확인하여 존재하지 않는 경우에만 접근을 허용하는 방식이다.
- AccessToken의 유효시간이 지나면 Redis 저장소에서도 해당 AccessToken이 제거될 것이다.

### 만약 로그아웃해서 유효시간이 남은 accesstoken 이 redis 에 존재하는데 재 로그인한다면?
- 로그인 시 redis 의 accesstoken 을 체크하고 해당 토큰을 제거해야 될 것이다.

## 참고 사이트
https://velog.io/@ehdrms2034/Access-Token-%EC%A0%80%EC%9E%A5-%EC%9C%84%EC%B9%98%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0

https://blog.ull.im/engineering/2019/02/07/jwt-strategy.html

https://velog.io/@lluna/%EC%84%B8%EC%85%98%EA%B3%BC-JWT-%EC%A4%91-JWT%EB%A5%BC-%EA%B3%A0%EB%A5%B8-%EC%9D%B4%EC%9C%A0-Redis%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%9D%B4%EC%9C%A0

https://sol-devlog.tistory.com/33