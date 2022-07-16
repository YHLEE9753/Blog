# Mockito 와 BDDMockito 는 어떻게 구분하여 사용해야 할까?
2022/07/17

# 1. 포스팅 이유
프로젝트를 진행하면서 Mockito 와 BDDMockito 를 구분하지 않고 사용하였고 코드리뷰를 해주는 팀원으로 부터 가독성의 어려움과 구분해서 사용하는 이유를 알고 쓰는것이냐는 피드백을 받았다. 캠프와 같이 정해진 기간내에 다양한 지식과 프로젝트를 진행하다보면 쓰고 있는 기술과 라이브러리를 언제 왜 쓰는지 정확히 인지하지 못하는 경우가 많다. 이에 반성하며 언제나 근거와 상황을 가지고 라이브러리를 사용하는 습관을 가져야 될거 같다.


# 2. Mockito, BDDMockito
다음과 같은 코드를 볼 수 있다.

`org.mockito.Mockito` : `when().thenReturn()`
`org.mockito.BDDMockito` : `given().willReturn()`

2가지 버젼의 Mockito 가 있고 각 버전에는 쓰임새가 존재한다.

## Mockito
Mockito 는 말그대로 mock 객체를 생성하여 원하는 동작을 미리 정하고 이를 기반으로 테스트하는 방식이다.
```java
when(drawMock.getId()).thenReturn(1L);
```
- 간단한 코드로 mock 객체의 getId 메서드를 사용할 때 1L를 리턴한다는 동작을 설정한다.

## BDDMockito
### BDD
BDD 는 TDD 에서 파생된 방법론이다. 기획부에서 제안된 시나리오를 기반으로 테스트코드를 작성하는 방법이다. 주로 다음과 같은 방식을 사용한다
>Given(주어진 환경) → When(행위) → Then(기대 결과)


### 그렇다면 TDD 와 BDD 의 차이는 무엇인가?
- TDD의 테스트는 모듈의 기능을 확인하는 관점에서 작성이 되고
- BDD의 테스트는 시나리오 주체를 기준으로 한 행위를 확인하기 위한 관점에서 작성이 된다.
- 따라서 BDD 는 기획서에 작성된 다양한 시나리오를 받아 그 시나리오에 맞게 코드가 작성되기때문에 TDD의 관점에서 확인하기 어려운 유저 시나리오의 흐름을 파악할 수 있다.
---
- 이를 통해 기획서를 바탕으로 작성된 코드는 기존 방식보다 비용절감이 되며,
- 다양한 시나리오를 바탕으로 개발되기 때문에 빼먹을 수 있는 예외조건들을 놓치지 않고 테스트 할 수 있으며,
- 기획서가 변경되어도 기존의 기획서를 바탕으로 변경가능하기 때문에 리스크가 줄어든다.
- 또한 기획서를 통해 서비스에 대한 이해도를 높일 수 있다.

### BDDMockito 를 그래서 왜쓸까?
바로 BDD 테스트 방식이 기본적으로 `Given(주어진 환경) → When(행위) → Then(기대 결과)` 를 사용하기 때문이다.
```java
@Test
void test() {
    // given
    when(drawMock.getId()).thenReturn(1L);

    // when

    // then
}
```
mock 객체의 동작을 미리 설정할 때 when then 을 사용하면 given 동작에서 when 을 쓰게 되므로 의미가 불분명해진다.

```java
void test() {
    // given
    given(drawMock.getId()).willReturn(1L);

    // when

    // then
}
```
BDDMockito 를 사용함으로써 given 에 맞는 given 메서드를 사용함으로써 통일성을 가지며 가독성을 높일 수 있다.

> 즉 BDDMockito 는 BDD를 사용하여 테스트코드를 작성할 때, 시나리오에 맞게 테스트 코드가 읽힐 수 있도록 도와주는 프레임워크이다.


## 참고 사이트
https://tecoble.techcourse.co.kr/post/2020-09-29-compare-mockito-bddmockito/

https://soso01.tistory.com/2

https://tv.kakao.com/channel/3693125/cliplink/414004682
