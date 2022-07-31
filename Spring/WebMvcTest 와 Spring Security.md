# WebMvcTest 와 Spring Security
2022/07/18

## 1. 포스팅 이유
stateless 하게 서버를 유지하면서 refresh token 을 활용하는것은 사실상 불가능하다 그렇다면 accessToken 과 refreshtoken 을 어떻게 관리하는 것이 성능과 보안 2개를 잡을 수 있을까?




https://joomn11.tistory.com/87


https://velog.io/@cieroyou/WebMvcTest%EC%99%80-Spring-Security-%ED%95%A8%EA%BB%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

GET /api/v1/posts/{postId}/comments
GET /api/v1/posts/{postId}/comments/{commentId}
이거 2개 같이 못쓰는 이유가?