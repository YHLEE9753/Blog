# record 클래스
2022/07/23

_위 글은 Java SE 15 update - Record 공식 문서를 바탕으로 작성되었습니다.

## 1. Record 클래스

record 클래스는 여러 fields 들을 선언하고, 자동으로  appropriate accessors, constructors, equals, hashCode, and toString methods 을 만들어 줍니다.
선언된 fields 들은 final 이기 때문에 "data carrier" 로 사용되도록 의도되었습니다.

**record 로 선언된 Rectangle 클래스**

```java
record Rectangle(double length, double width) { }
```

**실제 구현되는 클래스 코드
**
```java
public final class Rectangle {
    private final double length;
    private final double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    double length() { return this.length; }
    double width()  { return this.width; }

    // Implementation of equals() and hashCode(), which specify
    // that two record objects are equal if they
    // are of the same type and contain equal field values.
    public boolean equals...
    public int hashCode...

    // An implementation of toString() that returns a string
    // representation of all the record class's fields,
    // including their names.
    public String toString() {...}
}
```
- 헤더는 필드들을 자동으로 private final field 로 선언하여 준다.
- new 를 통해 인스턴스 생성한다.

```Rectangle r = new Rectangle(4,5);```

## 2. Record 클래스의 표준 생성자
다음은 record 클래스 생성자 예시이다
```java
record Rectangle(double length, double width) {
    public Rectangle(double length, double width) {
        if (length <= 0 || width <= 0) {
            throw new java.lang.IllegalArgumentException(
                String.format("Invalid dimensions: %f, %f", length, width));
        }
        this.length = length;
        this.width = width;
    }
}
```
---
- 흥미롭게도 `this.length` 나 `this.width` 선언을 하지 않아도, 자동으로 선언하여 준다.

```java
record Rectangle(double length, double width) {
    public Rectangle {
        if (length <= 0 || width <= 0) {
            throw new java.lang.IllegalArgumentException(
                String.format("Invalid dimensions: %f, %f", length, width));
        }
    }
}
```

## 3. Record Class Members 의 명시적 선언
### public accessor method
```java
record Rectangle(double length, double width) {
 
    // Public accessor method
    public double length() {
        System.out.println("Length is " + length);
        return length;
    }
}
```
- 다음과 같이 `length` 메서드를 통해 `public` 하게 접근할 수 있다.
- `getLength` 로 메서드 명을 짓기도 한다.
---
### static field, method, initializer

```java
record Rectangle(double length, double width) {
    
    // Static field
    static double goldenRatio;

    // Static initializer
    static {
        goldenRatio = (1 + Math.sqrt(5)) / 2;
    }

    // Static method
    public static Rectangle createGoldenRectangle(double width) {
        return new Rectangle(width, width * goldenRatio);
    }
}
```
- 다음과 같이 static fields, static initializers, static methord 를 record class 안에 선언할 수 있다.
- 선언한 것들은 일반 클래스와 동일하게 사용된다.

---
### nested record
```java
record Rectangle(double length, double width) {

    // Nested record class
    record RotationAngle(double angle) {
        public RotationAngle {
            angle = Math.toRadians(angle);
        }
    }
    
    // Public instance method
    public Rectangle getRotatedRectangleBoundingBox(double angle) {
        RotationAngle ra = new RotationAngle(angle);
        double x = Math.abs(length * Math.cos(ra.angle())) +
                   Math.abs(width * Math.sin(ra.angle()));
        double y = Math.abs(length * Math.sin(ra.angle())) +
                   Math.abs(width * Math.cos(ra.angle()));
        return new Rectangle(x, y);
    }
}
```
- record 클래스 내부에서는 accessor method 유무에 상관 없이 instance methods 를 선언할 수 있다.
- 또한 nested 클래스와 interface 를 record 클래스 내부에서 선언할 수 있다.
- 이때 포함된 nested record 는 자동으로 static 선언이 된다.
- 단 native method 는 record 클래스 내부에서 선언할 수 없다.

## 4. Record 클래스의 특징

- generic record 클래스 생성 가능
```java 
record Triangle<C extends Coordinate> (C top, C left, C right) { }
```
- 1개나 2개 이상의 인터페이스를 implements 한 record 클래스 생성 가능

```java 
record Customer(...) implements Billable { }
```
- record 클래스와 component 에 annotation 을 달 수 있다.

```java 
record Rectangle(
    @GreaterThanZero double length,
    @GreaterThanZero double width) { }
```

## 5. local record class
- local record 클래스는 local 클래스와 비슷하다
- local record 는 method 내부에서 정의될 수 있다.


```java
import java.time.*;
import java.util.*;
import java.util.stream.*;

record Merchant(String name) { }

record Sale(Merchant merchant, LocalDate date, double value) { }

public class MerchantExample {
    
    List<Merchant> findTopMerchants(
        List<Sale> sales, List<Merchant> merchants, int year, Month month) {
    
        // Local record class
        record MerchantSales(Merchant merchant, double sales) {}

        return merchants.stream()
            .map(merchant -> new MerchantSales(
                merchant, this.computeSales(sales, merchant, year, month)))
            .sorted((m1, m2) -> Double.compare(m2.sales(), m1.sales()))
            .map(MerchantSales::merchant)
            .collect(Collectors.toList());
    }   
    
    double computeSales(List<Sale> sales, Merchant mt, int yr, Month mo) {
        return sales.stream()
            .filter(s -> s.merchant().name().equals(mt.name()) &&
                s.date().getYear() == yr &&
                s.date().getMonth() == mo)
            .mapToDouble(s -> s.value())
            .sum();
    }    

    public static void main(String[] args) {
        
        Merchant sneha = new Merchant("Sneha");
        Merchant raj = new Merchant("Raj");
        Merchant florence = new Merchant("Florence");
        Merchant leo = new Merchant("Leo");
        
        List<Merchant> merchantList = List.of(sneha, raj, florence, leo);
        
        List<Sale> salesList = List.of(
            new Sale(sneha,    LocalDate.of(2020, Month.NOVEMBER, 13), 11034.20),
            new Sale(raj,      LocalDate.of(2020, Month.NOVEMBER, 20),  8234.23),
            new Sale(florence, LocalDate.of(2020, Month.NOVEMBER, 19), 10003.67),
            // ...
            new Sale(leo,      LocalDate.of(2020, Month.NOVEMBER,  4),  9645.34));
        
        MerchantExample app = new MerchantExample();
        
        List<Merchant> topMerchants =
            app.findTopMerchants(salesList, merchantList, 2020, Month.NOVEMBER);
        System.out.println("Top merchants: ");
        topMerchants.stream().forEach(m -> System.out.println(m.name()));
    }
}
```
nested record 클래스와 같이, local record class 는 implicitly staic 합니다. 즉 static 이 아닌 local 클래스와 달리 자체 메서드는 바깥쪽 메서드의 변수에 액세스할 수 없습니다.

## 6. record 직렬화
참고 사이트 참고

## 참고 사이트
https://docs.oracle.com/en/java/javase/15/language/records.html

## 다음으로
[record 클래스와 sealed 클래스와 interface(2) - sealed 클래스와 interface](https://velog.io/@yhlee9753/record-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-sealed-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-interface2-sealed-%ED%81%B4%EB%9E%98%EC%8A%A4)
