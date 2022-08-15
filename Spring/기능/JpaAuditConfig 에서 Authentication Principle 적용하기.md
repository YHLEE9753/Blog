# JpaAuditConfig 에서 Authentication Principle 적용하기
2022/08/16

## 1. 프로젝트에서 사용한 JpaAuditConfig 에서 Authentication Principle 적용 코드
- JpaAudit 에서 createdby 의 경우 JpaAuditConfig 를 통해 설정을 해주어야 한다.
- 보통 Spring Security filter 에서 적용되는 Authentication Principal 값을 활용한다.
- 이 예제의 경우 Authentication Principal 로 memberId 인 Long 값을 활용하는 경우이다

```java
import java.util.Optional;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.domain.AuditorAware;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.security.authentication.AnonymousAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;

@Configuration
@EnableJpaAuditing
public class JpaAuditConfig {

	@Bean
	public AuditorAware<Long> auditorAware() {
		return () -> {
			Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

			if (authentication == null || !authentication.isAuthenticated() || isAnonymous(authentication)) {
				return Optional.of(0L);
			}

			Long memberId = Optional.ofNullable((Long)authentication.getPrincipal()).orElse(0L);
			return Optional.of(memberId);
		};
	}

	private boolean isAnonymous(Authentication authentication) {
		return authentication instanceof AnonymousAuthenticationToken;
	}
}
```