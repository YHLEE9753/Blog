# depreciated 된 WebSecurityConfigurerAdapter 수정하기
2022/07/17

## 1. 포스팅 이유
스프링 부트 2.7 부터 WebSecurityConfigurerAdapter 이 depreciated 되었다. 코드 수정을 해보자

## 2. 왜 depreciated 되었는가
- Spring 프레임워크의 개발자들은 사용자가 컴포넌트 기반 보안 구성으로 이동하도록 권장한다.
- 따라서 WebSecurityConfigurerAdapter를 확장 하고 이전 방식과 같이 HttpSecurity 및 WebSecurity 를 구성하는 메서드를 재정의하는 대신
- SecurityFilterChain 및 WebSecurityCustomizer 유형의 두 빈을 선언하는 방식을 선택하였다.

## 3. code 비교(1)
### 기존코드
```java
@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
 
    @Override
    protected void configure(HttpSecurity http) throws Exception {
         
        // configure HTTP security...
         
    }
 
    @Override
    public void configure(WebSecurity web) throws Exception {
         
        // configure Web security...
         
    }      
}
```

### 변경코드
```java
@Configuration
public class SecurityConfiguration {
         
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     
    }
     
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
         
    }
         
}
```
- 변경 사항으로 `WebSecurityConfigurerAdapter` 를 더이상 `extends` 하지 않는다
- 따라서 `@Override` 가 아닌 직접 `@Bean` 을 통해 등록해 준다.
- 추가로 반환 값이 생겼다. `SecurityFilterChain` 와 `WebSecurityCustomizer` 이다.

## 4. code 비교(2)
### 기존코드
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
 
    @Bean
    public UserDetailsService userDetailsService() {
        return new ShopmeUserDetailsService();
    }
 
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
     
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests().antMatchers("/login").permitAll()
                .antMatchers("/users/**", "/settings/**").hasAuthority("Admin")
                .hasAnyAuthority("Admin", "Editor", "Salesperson")
                .hasAnyAuthority("Admin", "Editor", "Salesperson", "Shipper")
                .anyRequest().authenticated()
                .and().formLogin()
                .loginPage("/login")
                    .usernameParameter("email")
                    .permitAll()
                .and()
                .rememberMe().key("AbcdEfghIjklmNopQrsTuvXyz_0123456789")
                .and()
                .logout().permitAll();
 
        http.headers().frameOptions().sameOrigin();
    }
     
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/images/**", "/js/**", "/webjars/**"); 
    }
}
```

### 변경코드
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityCustomizer;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
 
@Configuration
public class SecurityConfiguration {
 
    @Bean
    public UserDetailsService userDetailsService() {
        return new ShopmeUserDetailsService();
    }
 
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
 
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     
        http.authorizeRequests().antMatchers("/login").permitAll()
                .antMatchers("/users/**", "/settings/**").hasAuthority("Admin")
                .hasAnyAuthority("Admin", "Editor", "Salesperson")
                .hasAnyAuthority("Admin", "Editor", "Salesperson", "Shipper")
                .anyRequest().authenticated()
                .and().formLogin()
                .loginPage("/login")
                    .usernameParameter("email")
                    .permitAll()
                .and()
                .rememberMe().key("AbcdEfghIjklmNopQrsTuvXyz_0123456789")
                .and()
                .logout().permitAll();
 
        http.headers().frameOptions().sameOrigin();
 
        return http.build();
    }
 
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return (web) -> web.ignoring().antMatchers("/images/**", "/js/**", "/webjars/**");
    }
 
}
```
- `return http.build();` 를 통해 `SecurityFilterChain` 를 반환한다.
- `(web) -> web.ignoring().antMatchers(...)` 를 통해 `WebSecurityCustomizer` 를 반환한다.

## 5. 추가적인 override 메서드들 적용
- JDBC 인증, 로컬 등 다양한 변수를 가지는 override 함수들이 존재한다
- 다름 예시들은 추후 코드 작성중 추가 조사가 필요할때 다음 사이트를 참고해서 적용시키자
[WebSecurityConfigurerAdapter override 함수들을 bean 으로 등록하기](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)

## 참고 사이트
https://codejava.net/frameworks/spring-boot/fix-websecurityconfigureradapter-deprecated

https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter
