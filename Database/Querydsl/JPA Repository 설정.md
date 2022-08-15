# JpaRepository 를 사용하면서 QueryDSL 적용
2022/08/16

## 1. JpaRepository 를 사용하면서 QueryDSL 적용하기
```java
import java.util.Optional;

import prgrms.project.stuti.domain.member.model.Member;

public interface CustomMemberRepository {

	Optional<Member> findMemberById(Long memberId);

	Optional<Member> findMemberByEmail(String email);

	Optional<Member> findMemberByNickName(String nickname);
}
```
- Repository 에서 사용될 메서드들을 interface 로 구성한다.

```java
@RequiredArgsConstructor
public class CustomMemberRepositoryImpl implements CustomMemberRepository {

	private final JPAQueryFactory jpaQueryFactory;

	@Override
	public Optional<Member> findMemberById(Long memberId) {
		return Optional.ofNullable(
			jpaQueryFactory
				.selectFrom(member)
				.where(member.id.eq(memberId), isNotDeleted())
				.fetchOne()
		);
	}

	@Override
	public Optional<Member> findMemberByEmail(String email) {
		return Optional.ofNullable(
			jpaQueryFactory
				.selectFrom(member)
				.where(member.email.eq(email), isNotDeleted())
				.fetchOne()
		);
	}

	@Override
	public Optional<Member> findMemberByNickName(String nickname) {
		return Optional.ofNullable(
			jpaQueryFactory
				.selectFrom(member)
				.where(member.nickName.eq(nickname), isNotDeleted())
				.fetchOne()
		);
	}

	private BooleanExpression isNotDeleted() {
		return member.isDeleted.isFalse();
	}
}
```
- CustomMemberRepository interface 를 implements 하여 실제 구현을 한다.
- 이곳에서 JPAQueryFactory 를 이용하여 QueryDSL 를 적용한다.

```java
import org.springframework.data.jpa.repository.JpaRepository;

import prgrms.project.stuti.domain.member.model.Member;

public interface MemberRepository extends JpaRepository<Member, Long>, CustomMemberRepository {

}
```
- JpaRepository 를 extends 하는 Repository 를 만든다. JpaRepository 기능을 활용할 수 있다.
- customRepository interface 또한 extends 한다.
- MemberRepository 를 사용하여 최종적으로 JpaRepository 에서의 메서드와 QueryDSL 를 적용한 메서드를 동시에 사용할 수 있다.
