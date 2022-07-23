# Generics(2) - 공변(Contravariace)과 반공변(Covariance)
2022/07/23

# 1. 공변(Contravariace)과 반공변(Covariance)

- **가변성(Variance)** : 특정 타입의 객체를 다른 타입의 객체로 변환할 수 있는 성격
    - **공변성(Covariant) **
      : X -> Y 가 가능할 때 `C<T>` 가 `C<X>` -> `C<Y>` 로 가능하다
      : T’가 T의 서브타입이면, `C<T’>`는 `C<T>`의 서브타입이다.
    - **반공변성(Contravariant) **
      : X -> Y 가 가능할 때 `C<T>` 가 `C<Y>` -> `C<X>` 로 가능하다
      : T’가 T의 서브타입이면, `C<T>`는 `C<T’>`의 서브타입이다.
    - **무공변성(invariant)	**
      : C와 `C<T’>`는 아무 관계가 없다.

- **불변성(Invariant)** :  X -> Y가 가능하더라도 `C<X>`는 `C<X>`로만 사용할 수 있다.

---  
**공변성 **: 자기 자신과 자식 객체로 타입 변환을 허용해주는 것입니다.(자바의 array 가 대표적인 공변의 예시)

```java
Object[] before = new Long[1];
```

**불공변** : List<String>과 List<Object>가 있을 때 두 개의 타입은 전혀 관련이 없다는 뜻입니다.
기본적으로 제네릭은 불변이다.
```java
public class Test {
    public static void test(List<Object> list) {

    }
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("test");
        test(list);   // 컴파일 에러
    } 
}
```



## 참고 사이트 및 도서
https://velog.io/@lsb156/covariance-contravariance
https://sungjk.github.io/2021/02/20/variance.html
https://devlog-wjdrbs96.tistory.com/263



