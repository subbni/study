# NumberExpression
[공식 문서](http://querydsl.com/static/querydsl/4.1.3/apidocs/com/querydsl/core/types/dsl/NumberExpression.html)

## 사용 이유
> 숫자를 다루는 복잡하고 반복적인 쿼리 작업을 안전하고 효율적으로 할 수 있도록 도와준다.
1. 타입 안정성 및 컴파일 시 오류 확인
    - 컴파일 시점에서 숫자 관련 오류를 바로 잡을 수 있음
3. 숫자 관련 다양한 함수 지원
    - avg(), sum(), count(), abs(), round(), ceil(), floor() 같은 SQL 숫자 함수를 자바 메서드처럼 바로 호출 가능
3. 동적 쿼리 및 복잡한 표현식 처리
```java
// 상품 가격의 평균을 구하는 쿼리
NumberExpression<Double> avgPrice = Expressions.numberTemplate(Double.class, "avg({0})", product.price);
queryFactory.select(avgPrice)
      .from(product)
      .fetch();
```
