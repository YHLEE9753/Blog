# Spring Security - Lambda DSL
2022/07/17

## 1. 포스팅 이유
configure 작성에 있어 기존의 방식에서 Lambda DSL 을 적용해 보자

## 2. Lambda DSL
- Spring Security 5.2 릴리스 에는 람다를 사용하여 HTTP 보안을 구성할 수 있는 DSL에 대한 개선 사항 이 포함되어 있다
- 이전 구성 스타일이 여전히 유효하고 지원된다는 점에 유의하는 것이 중요하다 
- 람다의 추가는 더 많은 유연성을 제공하기 위한 것이지만 사용은 선택 사항이다

## 3. code 비교
### 기존방식
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/blog/**").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .rememberMe();
    }
}
```

### 개선방식
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests(authorizeRequests ->
                authorizeRequests
                    .antMatchers("/blog/**").permitAll()
                    .anyRequest().authenticated()
            )
            .formLogin(formLogin ->
                formLogin
                    .loginPage("/login")
                    .permitAll()
            )
            .rememberMe(withDefaults());
    }
}
```
- `and` 를 더이상 사용할 필요없다.
- 기존의 `()` 로 끝나는 문법에 `(a -> {})` 를 이용하여 좀더 가독성을 높인다.
- `default` 경우 `()` 를 비우는 것이 아닌 `withDefaults()` 를 작성한다.

### 나만의 방식
```java
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final CustomOAuth2UserService oAuth2UserService;
    private final OAuth2SuccessHandler successHandler;
    private final TokenService tokenService;

    @Bean
    protected SecurityFilterChain configure(HttpSecurity http) throws Exception {
        http.httpBasic(httpBasic -> httpBasic.disable());
        http.cors(cors -> cors.disable());
        http.csrf(csrf -> csrf.disable());
        http.sessionManagement(sessionManagement ->
            sessionManagement.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        );

        // 순서 1
        http.authorizeRequests(authorizeRequests ->
            authorizeRequests.antMatchers("/token/**", "/login/**").permitAll()
        );

        // 순서 2
        http.authorizeRequests(authorizeRequests ->
            authorizeRequests
                .antMatchers(GET, "/api/user/**")
                .hasAnyAuthority(ROLE_USER.stringValue)
        );

        // 순서 3
        http.authorizeRequests(authorizeRequests ->
            authorizeRequests.anyRequest().authenticated());

        http.formLogin(formLogin ->
            formLogin.loginPage("/login")
                .failureUrl("/login")
        );

        http.logout(logout->logout.logoutSuccessUrl("/login"));

        http.oauth2Login(oauth2Login ->
            oauth2Login.successHandler(successHandler)
                .userInfoEndpoint()
                .userService(oAuth2UserService)
        );

        http.addFilterBefore(new JwtAuthenticationFilter(tokenService),
            UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}

```
- 아예 http 를 전부 붙여서 사용해 보았다.
- 근데 생각보다 가독성이 안좋아보인다
- 기존의 방식을 사용하는게 좋을 거 같다.

## 참고 사이트
https://spring.io/blog/2019/11/21/spring-security-lambda-dsl