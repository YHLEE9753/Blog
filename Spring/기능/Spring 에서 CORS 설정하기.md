# Spring 에서 CORS 설정하기
2022/07/13

## 1. 포스팅 이유
프로젝트 시작 전에 직접 CORS 설정을 해보자

## 2. CORS 란?
다음 게시물을 참고해주세요<br>
[CORS 란?](https://github.com/YHLEE9753/Blog/blob/main/Network/CORS%20%EB%9E%80.md)<br>

## 3. Spring 에서 CORS 설정
### @CrossOrigin 어노테이션 사용하기
- origins
  - 허용된 출처, 이 값은 pre-flight 응답과 실제 응답 모두에 access-control-allow-origin헤더에 배치된다.
- allowedHeaders
  - 실제 요청 중에 사용할 수 있는 요청 헤더 목록이다. pre-flight의 응답 헤더인 access-control-allow-header에 값이 사용된다.
- allowCredential
  - 브라우저가 요청과 관련된 쿠키를 포함해야 되는지 여부를 결정한다.
  - 이 값이 true이면, pre-flight 응답에는 값이 true로 설정된 access-control-allow-credentials 헤더가 포함된다.

```java
@CrossOrigin(origin="*", allowedHeaders = "*")
@Controller
public class MainController {
    @GetMapping(path = "/")
    public String main(Model model) {
        return "main";
    }
}
```

### Configuration 적용하기
- global 하게 적용하는 방법이다.
- `@Configuration` 을 작성하여 설정파일임을 고지한다
- `WebMvcConfigurer` 를 implements 한다.
- `addCorsMappings` 메소드를 오버라이드 한 후 CORS 를 적용할 URL 패턴을 정의한다.
- `allowedOrigins` 를 사용하여 자원 공유를 허락할 Origin 을 지정한다.
- `allowedMethods` 를 사용하여 허용 할 HTTP method 를 지정할 수 있다.
- `maxAge` 를 사용하여 원하는 시간만큼 pre-flight 리퀘스트를 캐싱해 둘 수 있다.

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://127.0.0.1:8080")
            .allowedMethods(
                HttpMethod.GET.name,
                HttpMethod.POST.name,
                HttpMethod.PUT.name,
                HttpMethod.DELETE.name
            )
    }
}
```

### CorsFilter 사용하기
서블릿 필터 인터페이스를 이용하여 개발되었다. 
웹 서버의 모든 리소스의 요청을 가로채서 Cross domain request인지 체크하여 실제 요청 페이지에 전달하기전에 적절한 CORS 정책과 해더들을 적용한다.

- Access-Control-Allow-Origin
  - 도메인 간 요청을 할 수 있는 권한이 부여된 도메인을 지정한다.
- Access-Control-Allow-Credentials
  - 도메인 간 요청에 credential 권한이 있는지 없는지 지정한다.
- Access-Control-Expose-Headers
  - 노출하기에 안전한 헤더를 나타낸다.
- Access-Control-Max-Age
  - pre-flighted 요청이 얼마만큼의 시간동안 캐시되는지
- Access-Control-Allow-Methods
  - 리소스에 접근할 때 메소드가 허용되는지
- Access-Control-Allow-Headers
  - 어떤 헤더 필드 네임이 실제 요청에서 사용할 수 있는지 가리킨다.
```java
@Component
public class SimpleCorsFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE, PUT, PATCH");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers", "Content-Type, Accept, X-Requested-With, remember-me");
        chain.doFilter(req, res);
    }

    @Override
    public void init(FilterConfig filterConfig) {
    }

    @Override
    public void destroy() {
    }
}
```

## 4. Spring Security 에서 설정
- configure를 재정의해 cors() 메서드를 사용한다
- 이때 몇가지 고려해야 한다.
- 첫번째로 csrf 필터를 비활성화해야 한다.
  - 디버깅을 해보면 알겠지만 Csrf 필터가 활성화 되어 있으면 Csrf 토큰이 없어 요청이 거부된다.
- 두 번째는 인가와 관련해서 Preflight 요청은 제외 해야 한다.
```java
    @Override
    protected void configure(HttpSecurity http) throws Exception{
    http.
    .authoizeRequests()
    .requestMatchers(CorsUtils::isPreFilghtReqeust).permitAll()
    .anyRequest().authenticated()
    .and()
    .cors()
        .configurationSource(corsConfigurationSource())
        .and()
    .csrf().disable()
    .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        
    }
    
    @Bean
    public CorsConfigurationSource corsConfigurationSource(){
    CorsConfiguration corsConfiguration = new CorsCOnfiguration();
    corsConfiguration.addAllowedOrigin("http://localhost:5500");
    corsConfiguration.setAllowedMethods(
        List.of(GET.name(), POST.name())
    );
    corsConfiguration.addAlowedHeader("*");
    
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", corConfiguration);
    
    return source;
    
    }
    //* addAllowedOrigin() : 허용할 URL
    //* addAllowedHeader() : 허용할 Header
    //* addAllowedMethod() : 허용할 Http Method
    
```

**다른 예시**
```java
@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final JwtTokenProvider jwtTokenProvider;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .httpBasic().disable()
                .cors().configurationSource(corsConfigurationSource())  ---------- (1)
//                .headers().frameOptions().disable()
                .and()
                    .csrf().disable()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                    .authorizeRequests()
                        .antMatchers("/api/v1/shops/**").hasRole("OWNER")
                        .antMatchers(HttpMethod.PUT, "/api/v1/users/**").hasAnyRole("USER", "OWNER")
                        .antMatchers(HttpMethod.DELETE, "/api/v1/users/**").hasAnyRole("USER", "OWNER")
                        .antMatchers(HttpMethod.GET, "/api/v1/users/").hasAnyRole("USER", "OWNER")
                        .anyRequest().permitAll()
                .and()
                    .addFilterBefore(new JwtAuthenticationFilter(jwtTokenProvider),
                            UsernamePasswordAuthenticationFilter.class);


    }
    
    // CORS 허용 적용
    @Bean --------- (2)
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();

        configuration.addAllowedOrigin("*");
        configuration.addAllowedHeader("*");
        configuration.addAllowedMethod("*");
        configuration.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
 }
```

## 5. Spring security 에서의 cors 와 Spring 의 cors 의 차이
- 스프링 시큐리티를 사용중이라면 WebSecurityConfigurerAdapter를 상속하는 클래스에서 configure를 재정의하여 cors()를 사용해야한다 라고 하는데 좀 더 자료를 찾아봐야 겠다.
- 또한 위와 같이 하면 따로 filter 를 적용시킬 필요는 없다고 한다.

## 참고 사이트
https://bohyeon-n.github.io/deploy/web/cors.html

https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0

https://yhmane.tistory.com/214

https://emgc.tistory.com/131

https://white-board.tistory.com/197?category=961447