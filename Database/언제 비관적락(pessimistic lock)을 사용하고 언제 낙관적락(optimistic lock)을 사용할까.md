# 언제 비관적락(pessimistic lock)을 사용하고 언제 낙관적락(optimistic lock)을 사용할까?
2022/07/17


프로젝트를 진행하면서 다량의 트래픽이 몰리는 응모 시스템을 개발중이다. 응모 부분을 담당하여 개발하면서 기본 비즈니스 로직을 구현하였다. 추가 회의를 진행하였고, 트래픽이 몰릴때 db 에 lock 을 걸어 데이터 무결성을 보장하면 좋을거 같다는 의견과 함께 어떤 락을 거는게 좋을지 고민해보라는 피드백을 받았다. 오늘 고민의 내용을 담아본다.

1. 트랜잭션
2. 트랜잭션 격리성
3. 비관적 락(pessimistic lock)
4. 낙관적 락(optimistic lock)
5. 결론

# 1. 트랜잭션
트랜잭션 : 여러 읽기/쓰기를 논리적으로 하나로 묶음
예를 들어 db 에 select 를 하고 update 를 하고 insert 를 한다고 가정해 보자. select 한 후 update 를 진행하면서 db 데이터는 변경된다. 이때 insert 에서 오류가 발생한다면 update 된 데이터는 믿을 수 없는 데이터가 된다. 따라서 DB 는 **All or Nothing**g 전략을 취한다. 이 전략을 취하기 위해 트랜잭션이라는 단위를 사용한다. 즉 중간에 오류가 발생하면 rollback 을 통해 트랜잭션 전체를 초기화 시킨다.

트랜잭션은 보통 커넥션 단위라고 하며 따라서 트랜잭션 전파는 굉장히 중요한 문제이다. 이와 관련된 내용은 추후 포스팅하겠다.

## ACID
트랜잭션은 4가지 특징을 가진다.
### 원자성(Atomicity)
- 트랜잭션은 최소단위로, 전부 처리되거나 아예처리되지 않는다(All or nothing)
### 일관성(Consistency)
- 트랜잭션이 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지한다(무결성 보장)
### 격리성(Isolation)
- 트랜잭션을 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장한다
### 영속성(Durability)
- 성공적으로 수행된 트랜잭션은 DB 에 영속적으로 반영된다
- 하나의 트랜잭션이 성공적으로 수행되었다면, 해당 트랜잭션에 대한 로그가 남아야하는 성질을 말한다

# 2. 트랜잭션 격리성
격리성은 오늘의 주제에서 굉장히 중요하다. 바로 격리성을 보장하기 위해 lock 을 사용하기 때문이다.
## 1. 경쟁상태(Race Condtion)
- 여러 클라이언트가 같은 데이터에 접근할 때 문제가 발생한다.
- 만약 단순히 트랜잭션을 순서대로 실행한다면 성능이 저하 될 것이다.
- 따라서 관련 문제들을 보고 이를 해결하기 위해 지원되는 다양한 격리 수준 지원을 확인하자

## 2. 격리성 관련 문제들
1. dirty read : 커밋되지 않는 데이터 읽기
    - 변경사항이 반영되지 않은 값을 다른 트랜잭션에서 읽는 경우
2. dirty write : 커밋되지 않은 데이터 덮어쓰기
3. Nonrepeatable read : 읽는 동안 데이터 변경
    - 트랜잭션이 끝나기 전에 수정사항이 반영되어, 트랜잭션 내에서 쿼리 결과가 일관성을 가지지 못하는 경우
4. phantom read : 외부에서 수행되는 삽입/삭제 작업으로 인해 트랜잭션 내에서의 동일한 쿼리가 다른 값을 반환하는 경우
5. lost update : 같은 데이터를 쓸 때 발생하는 변경 유실
- 예) count 증가, 위키페이지 수정

## 3. 4개의 트랜잭션 고립 단계
위의 격리성 관련 문제들을 해결하기 위해 4가지의 트랜잭션 고립 단계가 존재한다.
- READ UNCOMMITTED: 커밋하지 않은 데이터(= 다른 트랜잭션이 수정중인 데이터)를 읽을 수 있다. 이 경우 Dirty read가 발생할 수밖에 없다.
- READ COMMITTED: 커밋한 데이터(= 다른 트랜잭션이 수정을 끝낸 데이터, 혹은 수정 중이지 않은 데이터)만 읽을 수 있다. Dirty read는 방지할 수 있지만, Nonrepeatable read는 방지할 수 없다.
- REPEATABLE READ: 한번 조회한 데이터를 반복해서 조회해도 같은 데이터가 나온다. 다만 Phantom read는 발생하게 된다.
- SERIALIZABLE: 가장 엄격한 수준으로, Phantom read 역시 발생하지 않는다. 그러나 동시성 처리 성능이 급격히 떨어진다.


# 3. 비관적 락(pessimistic lock), 낙관적 락(optimistic lock)

비관적 락 : 트랜잭션이 시작될 때 shared lock 또는 exclusive lock 을 건다. 즉 shared lock 을 걸게 되면 write 를 하기 위해서 exclusive lock 을 얻어야 하는데, shared lock 이 다른 트랜잭션에 의해 걸려 잇으면 해당 lock 을 얻지 못해 업데이트를 할 수 없다.

- transaction1 이 해당 row 를 참조하면 lock 이 걸린다.
- transaction2 는 exclusive lock 이 얻기에 transaction1 이 commit 될때까지 대기한다.
- transaction1 이 commite 되면 transaction2 는 해당 row 에 쿼리를 적용한다.

낙관적 락 : DB 수준이 아닌 Application level 에서 잡아주는 lock 이다.

- A 가 table 의 row1를 select 한다. 이때 version 은 1 로 설정된다.
- B 가 table 의 row를 select 한다 이때 version 은 1 로 설정된다.
- A 가 row1 을 업데이트 하면서 version 을 2로 교체한다.
- B 는 version이 2 로 바뀌었으므로 update 에 실패한다.
    - version 이 변경되었으므로 수정 요청은 반영되지 않는다.

## 무엇이 더 좋을까?
낙관적 락 : 변경 사항이 데이터베이스에 커밋될 때만 레코드가 잠기는 경우.
비관적 락 : 편집하는 동안 레코드가 잠기는 경우.

- 낙관적 락은 트랜잭션이 커밋 될때 적용되고 application level 의 lock 을 잡으므로 성능적으로 비관적 락보다 좋다.
- 하지만 롤백이 발생할때 비관적 락을 롤백을하면 끝나지만 낙관적 락은 수동으로 롤백처리를 해야되 구현하기도 까다롭고 update 를 한번 더 해야 하는 등 성능적으로 좋지 않다.
- 따라서 충돌이 적은 경우 낙관적 락을 사용하지만
- 충돌이 많거나 은행 업무와 같이 무결성이 중요한 경우에는 비관적 락을 사용한다.


# 4. 락으로 트래픽 처리가 가능할까?
https://d2.naver.com/helloworld/5048491
캐시를 사용하는 것도 하나의 방법이다.

# 5. 결론
트랜잭션에 대해 공부할 수 있는 기회였다. d2 사이트를 보면서 좀더 공부해보아야 겠다.

## 참고 사이트
https://www.youtube.com/watch?v=urpF7jwVNWs

https://www.youtube.com/watch?v=poyjLx-LOEU

https://ko.wikipedia.org/wiki/ACID

https://sabarada.tistory.com/117#recentComments

https://sabarada.tistory.com/175

https://stackoverflow.com/questions/129329/optimistic-vs-pessimistic-locking
