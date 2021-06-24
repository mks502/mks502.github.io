---
layout: post
title: "💻 [Java] 문자열 String Constant Pool 과 문자열에서 ==, equals()의 차이점"
category : Theory
tags: [theory, java, string, string constant pool]
---

# 💻 [Java] String - String Constant Pool 과 문자열에서 ==, equals()의 차이점

    안녕하세요. 👋
    
    오늘은 자바의 String - String Constant Pool 과 문자열에서 ==, equals()의 차이점에 대해 알아보겠습니다.
    일반적으로 데이터를 비교할 때는 == 연산자를 많이 사용하는데
    문자열을 비교할 때는 equals라는 메소드를 사용하여 비교합니다.
 
    왜 그럴까요?
    String 객체를 생성하는 방법이 차이가 있기 때문입니다.

## String 생성 방법으로 인한 차이
String 객체를 생성하는데는 두가지 방법이 있다.
- String literal, 즉 큰 따옴표("")를 사용하는 방법
- new 연산자를 사용하는 방법

두 방법에는 상당히 큰 차이가 있다.

![test](/images/2021-6-24/test.JPG)

![result](/images/2021-6-24/result.JPG)

위의 테스트를 보면 알 수 있듯이

String literal로 생성한 객체는 내용이 같다면 같은 객체, 즉 동일한 메모리 주소를 가리키고 있다.

하지만 new 연산자로 생성한 String 객체는 문자열이 같더라도 개별적인 객체임을 알 수 있다.

## String Constant Pool

String literal로 생성하면 해당 String 값은 Heap 영역 내 "String Constant Pool"에 저장되어 재사용되지만,
new 연산자로 생성하면 같은 내용이라도 여러 개의 객체가 각각 Heap 영역을 차지하기 때문이다.

![string-pool](/images/2021-6-24/string-pool.png)

(그림에는 생략되어 있지만) 생성된 String 객체는 Stack 영역에 저장된다.

Heap 영역에는 "Cat", "Dog"과 같은 '값'들이 들어가게 되는데, 그림의 우측을 보면 중요한 차이를 발견할 수 있다.

String literal로 생성한 객체는 "String Pool"에 들어간다.
String literal로 생성한 객체의 값(ex. "Cat")이 이미 String Pool에 존재한다면, 해당 객체는 String Pool의 reference를 참조한다.
그림에서 s1과 s2가 같은 곳을 가리키고 있는 것도 이 때문이다.
new 연산자로 생성한 String 객체는 같은 값이 String Pool에 이미 존재하더라도, Heap 영역 내 별도의 객체를 가리키게 된다.

이런 차이 때문에 메모리를 효율적으로 사용하기 위해서는 항상 String literal(큰 따옴표)로 String을 생성을 권장한다.

## ==, equals()의 차이점

위와 같은 차이점 때문에 상황마다 차이가 생긴다.

== 연산 같은 경우는 주소값을 비교하므로 같은 문자열로 리터럴을 이용해 생성한 String 이라면 == 연산을 통해 동일하다는 결과를 받을 수 있다.
하지만, 같은 문자열이라도 new 연산을 통해 생성한 String이라면 별도의 객체가 되어버리기 때문에 == 연산을 통해 동일하다는 결과를 받을 수 없다.

하지만 String의 equals 메소드 같은 경우는 String 객체에서 실제 값인 문자열 자체에 대해 비교를 해준다.
그렇기 때문에 String 에서 올바르게 문자열이 동일한지를 확인하려면 == 연산 보다는 문자열 자체를 비교하는 equals 메소드를 사용하는 것이 좋다.  

##### 정리

- String 생성 방법은 리터럴 방법과 new 연산 방법이 있다.
- <b>literal 방법이 동일한 문자열 값에 대해 메모리를 공유할 수 있으므로 효율적이다</b>.
- 문자열 값에 대한 비교가 필요할 때 == 연산만으로는 동일한 문자열이더라도 <b>동일한 문자열임을 보장받을 수 없으므로 equals 메소드를 활용해야한다</b>.

        
### 끝

    자바의 String - String Constant Pool 과 문자열에서 ==, equals()의 차이점에 대해 알아보았습니다.
    감사합니다. 🙏

-------------------------------------------------

Reference

<https://starkying.tistory.com/entry/what-is-java-string-pool>

<https://www.baeldung.com/java-string-pool>