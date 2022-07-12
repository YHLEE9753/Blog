# OncePerRequestFilter 와 Filter 의 차이
2022/07/12

## 1. 포스팅 이유
JWT OAUTH 구현을 하던 중 팀원으로 부터 authenticationfilter 를 사용할 때 GenericFilterBean 말고 OncePerRequestFilter 사용하라고 추천받았다.
그 이유로는 OncePerRequestFilter 의 경우 다른 서블렛으로 부터 요청을 받을때 수행할 수 있다고 하는데 정확한 이유에 대해 한번 알아보자

## 2. Spring MVC request life cycle
![img.png](../images/mvcreaueestlifecycle.png)

## 3. GenericFilterBean
- Filter 은 서블릿이 실행되기 전에 doFilter 를 통해 로직이 수행되고, 서블릿이 실행된다.
- 이때 Filter 를 확장하여 스프링에서 제공하는 필터가 있는데 그것이 바로 GenericFilterBean 이다.
- 우리는 Filter 가 빈으로 등록될 수 있음을 알고 있다.
- GenericFilterBean은 기존 Filter에서 얻어올 수 없는 정보였던 Spring의 설정 정보를 가져올 수 있게 확장된 추상 클래스이다.
- 심지어 setter 메서드를 통한 정보 저장도 가능하다.

