# fetchResults fetchCount depreciated
2022/08/18


## 1. fetchResults fetchCount depreciated
- QueryDSL 를 오랜만에 사용하던 중 fetchResults fetchCount 이 depreciated 된것을 볼 수 있었다.
- 어떤 이유에서 depreciated 되었고, 어떤 방식으로 count 를 구현하는게 효과적일지 알아보자

## 2. depreciated 된 이유

> fetchResults() : Get the projection in QueryResults form. Make sure to use fetch() instead if you do not rely on the QueryResults.getOffset() or QueryResults.getLimit(), because it will be more performant. Also, count queries cannot be properly generated for all dialects. For example: in JPA count queries can’t be generated for queries that have multiple group by expressions or a having clause. Get the projection in QueryResults form. Use fetch() instead if you do not need the total count of rows in the query result.

> fetchCount() : An implementation is allowed to fall back to fetch().size().

- 모든 dialect 에서 QueryResults 로 count 쿼리를 날리는 것이 완벽하게 지원이 되지 않으므로 fetch 를 사용하라고 권장한다
- total count 가 필요하더라도 안정성을 위하여 fetch() 를 사용하고 count 쿼리를 따로 날리는 것을 추천한다.

## 3. 코드 비교
#### 기존 코드
```java
    public void paging2(){
        QueryResults<Member> resultList = queryFactory
                .selectFrom(member)
                .orderBy(member.username.desc())
                .offset(1)
                .limit(2)
                .fetchResults();
```
#### 변경 코드
```java
    public void paging2(){
        long size = queryFactory
                .selectFrom(member)
                .orderBy(member.username.desc())
                .offset(1)
                .limit(2)
                .fetch()
                .size();
```
- int 범위보다 클 수 있으니 long 으로 받자
- DB에 count 쿼리를 따로 날리는 방법이 좋아보이긴 하나, DB 마다 count(*)의 작동방식이 다르기도 하니 각각의 상황에 맞춰서 해결법을 생각해야한다.

## 4. count 쿼리 실행시 성능 주의
- 데이터 조회 쿼리는 여러 테이블을 조인해야 한다.
- count 쿼리는 조인이 필요없는 경우도 있다.
- 하지만 위와 같이 자동화된 count 쿼리는 원본 쿼리와 같이 모두 조인을 해버리기 때문에 성능이 안나올 수 있다.
- 따라서 count 쿼리에 조인이 필요없는 성능 최적화가 필요하다면
- count 전용 쿼리를 별도로 작성하자.


## 참고 사이트
https://velog.io/@antcode97/%EC%9E%98%EA%B0%80..-fetchResults-fetchCount

https://www.inflearn.com/questions/23280?re_comment_id=156501

https://okky.kr/article/1188081