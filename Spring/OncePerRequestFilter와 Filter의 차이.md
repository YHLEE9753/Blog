# OncePerRequestFilter 와 Filter 의 차이
2022/07/12

## 1. 포스팅 이유
JWT OAUTH 구현을 하던 중 팀원으로 부터 `authenticationfilter` 를 사용할 때 `GenericFilterBean` 말고 `OncePerRequestFilter` 사용하라고 추천받았다.
그 이유로는 `OncePerRequestFilter` 의 경우 다른 서블렛으로 부터 요청을 받을때 수행할 수 있다고 하는데 정확한 이유에 대해 한번 알아보자

## 2. Spring MVC request life cycle
![img.png](../images/mvcreaueestlifecycle.png)

## 3. GenericFilterBean
- `Filter` 은 서블릿이 실행되기 전에 `doFilter` 를 통해 로직이 수행되고, 서블릿이 실행된다.
- 이때 `Filter` 를 확장하여 스프링에서 제공하는 필터가 있는데 그것이 바로 `GenericFilterBean` 이다.
- 우리는 `Filter` 가 빈으로 등록될 수 있음을 알고 있다.
- `GenericFilterBean은` 기존 `Filter` 에서 얻어올 수 없는 정보였던 `Spring` 의 설정 정보를 가져올 수 있게 확장된 추상 클래스이다.
- 심지어 `setter` 메서드를 통한 정보 저장도 가능하다.

## 4. OncePerRequestFilter 사용 이유
- `Filter` 와 `GenericFilterBean` 의 공통적인 특징은 **매 서블릿 마다 호출이 된다** 이다.
- 서블릿은 사용자의 요청을 받으면 서블릿을 생성해 메모리에 저장해두고, 같은 클라이언트의 요청을 받으면 생성해둔 서블릿 객체를 재활용하여 요청을 처리한다.
- 즉 기복적으로 서블릿은 싱글톤이다.
- 문제는 다른 서블릿으로 `dispatch` 되는 경우가 존재할 수 있다.
- 인증과 제어의 경우 `ReqeustDispatcher` 클래스에 의해 다른 서블릿으로 `dispatch` 된다
- 이 때 이동할 서블릿에 도착하기 전에 다시 한번 `filter chain` 을 거치게 된다.
- 이때 또 다른 서블릿이 우리가 정의해둔 필터가 `Filter` 나 `GenericFilterBean` 로 구현된 `filter` 를 또 타면서 필터가 2번 실행되게 된다.
- 불필요하게 `Filter chain` 을 2번 타게 된다.
- `OncePerRequestFilter` 를 사용하면 사용자의 한번의 요청당 딱 한번만 실행되는 필터를 만들 수 있다.

> dispatch 되어 불필요하게 filter chain 을 여러번 거칠 수 있기에 사용자의 한번의 요청당 딱 한번만 실행되는 필터를 만들 수 있는 OncePerRequestFilter를 사용하라

