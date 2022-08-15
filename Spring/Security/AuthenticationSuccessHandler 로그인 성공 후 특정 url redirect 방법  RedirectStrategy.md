# AuthenticationSuccessHandler 로그인 성공 후 특정 url redirect 방법 : RedirectStrategy
2022/07/17

## 1. 포스팅 이유
OAuth 구현중 로그인 성공 후 AccessToken 을 response body 에 담아서 전달하는데 전달하고자 하는 url 설정이 필요했다.

## 2. RedirectStrategy, RequestCache
- RequestCache : 로그인 화면을 보여주기 전에 사용자 요청을 저장하고 이를 꺼내오는 메카니즘을 정의하는 인터페이스
- RedirectStrategy : Spring Security 가 화면 이동에 대한 규칙을 정의하는 부분을 만든 인터페이스로 이 인터페이스를 구현한 객체로 화면 redirect 된다
`void sendRedirect` 메서드가 정의되어 있다.

## 3. 적용 코드
```java
@Slf4j
@RequiredArgsConstructor
@Component
public class OAuth2SuccessHandler implements AuthenticationSuccessHandler {

    private final TokenService tokenService;
    private final ObjectMapper objectMapper;
    private final UserService userService;

    private RedirectStrategy redirectStratgy = new DefaultRedirectStrategy();

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
        Authentication authentication)
        throws IOException, ServletException {
        // 인증 된 principal 를 가지고 온다.
        OAuth2User oAuth2User = (OAuth2User) authentication.getPrincipal();
        Map<String, Object> attributes = oAuth2User.getAttributes();
        String email = (String) attributes.get("email");
        String name = (String) attributes.get("name");
        String picture = (String) attributes.get("picture");

        // 최초 로그인이라면 회원가입 처리를 한다.(User 로 회원가입)
        userService.getUser(email)
            .orElseGet(() -> userService.saveUser(
                User.builder()
                    .username(email)
                    .name(name)
                    .picture(picture)
                    .role(ROLE_USER)
                    .build()
            ));

        // 토큰 생성
        Token token = tokenService.generateToken(email, ROLE_USER.stringValue);

        resultRedirectStrategy(request, response, token);
    }

    protected void resultRedirectStrategy(HttpServletRequest request, HttpServletResponse response,
        Token token) throws IOException, ServletException {

        response.setContentType("text/html;charset=UTF-8");
        response.setContentType("application/json;charset=UTF-8");

        Cookie cookie = new Cookie("refreshToken", token.getRefreshToken());

        cookie.setSecure(true);
        cookie.setHttpOnly(true);
        cookie.setPath("/");
        cookie.setMaxAge((int) tokenService.getRefreshPeriod());

        response.addCookie(cookie);

        PrintWriter writer = response.getWriter();
        writer.println(objectMapper.writeValueAsString(token.getAccessToken()));

        String targetUrl = "http://localhost:3000/main";
        redirectStratgy.sendRedirect(request, response, targetUrl);
    }
}
```
- 위 코드는 실제 프로젝트 OAuth 구현 코드이다.
- 로그인 성공시 accesstoken 을 response body 에 담고, refreshtoken 을 쿠키에 담어서
- targetUrl 로 redirect 를 진행시킨다.
- 기존에 서버 port 인 8080 이동하는 문제가 있어 3000 번 포트까지 전체 url 을 지정하였다.

## 참고 사이트
https://to-dy.tistory.com/94

https://www.baeldung.com/spring_redirect_after_login



