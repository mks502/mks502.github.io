---
layout: post
title: "💻 [Java] Java의 변수 - 기본형과 참조형, 래퍼 클래스"
category : Theory
tags: [theory, java, primitive, reference]
---

# 💻 [Java] Java의 변수 - 기본형과 참조형, 래퍼 클래스

    안녕하세요. 👋
    
    오늘은 Java의 변수 - 기본형과 참조형에 대해 알아보겠습니다.

## 기본형 타입 (Primitive Type)

- 총 8가지의 기본형 타입(Primitive type)을 미리 정의하여 제공한다.
- 기본값이 있기 때문에 Null이 존재하지 않는다. 만약 기본형 타입에 null 값을 넣고 싶다면 래퍼 클래스를 활용한다.
- 실제 값을 저장하는 공간이다.
- 만약 컴파일 시점에 담을 수 있는 크기를 벗어나면 에러를 발생시키는 컴파일 에러가 발생한다. 주로 문법상의 에러가 많다. 예를 들어 ;을 안붙였다는 이유로 빨간 줄이 쳐지는 경우

![data-type](/images/2021-6-21/data-type.JPG)

## 참조형 타입 (Reference type)

- 기본형 타입을 제외한 타입들이 모두 참조형 타입(Reference type)이다.
- 값이 저장되어 있는 곳의 메모리 주소값을 저장한다.
- 주소를 지정하는 것이기 때문에 null 로 지정할 수 있다.
    
    ex) Date today; //참조형 변수 today 선언
    today = new Date(); // today에 생성 된 객체의 주소를 저장

### Wrapper 클래스

- 기본형 타입으로 존재하는 것들을 참조형 타입으로 사용할 수 있다.
- Wrapper 클래스를 이용하면 참조형 타입으로 사용되므로 값이 저장되어 있는 메모리 주소값을 저장한다.

### Wraaper 클래스를 사용하는 이유

- 원시 타입은 null을 담을 수 없다. 반면 참조 타입은 null을 가질 수 있다.
- 원시 타입은 제네릭 타입에서 사용할 수 없다. 반면에 참조 타입은 제네릭 타입에서 사용할 수 있다.

### 참조형 타입(Wrapper 클래스) vs 기본형 타입

기본형 타입은 null을 담을 수도 없고 제네릭 타입으로 사용도 불가능한데

그럼 왜 쓰는 것일까?

##### 1.접근 속도

- 원시 타입은 '스택' 메모리에 <b>값이 존재</b>한다.
- 반면에 참조 타입은 하나의 인스턴스이기 때문에 '스택'메모리에는 <b>참조 값(주소)</b>만 있고, 실제 값은 힙 메모리에 존재한다.
    - 기본형은 값에 바로 접근하지만 참조형은 주소 값을 이용해 실제 값에 접근해야함
    
- 그리고 값을 필요로 할 때마다 언박싱 과정을 거쳐야 하니 원시 타입과 비교해서 접근 속도가 느려지게 된다.
    - 예외적으로 엄청 큰 숫자를 복사해야 한다면, 참조값만 넘길 수 있는 참조 타입이 좋을 수 도 있다.

![performance](/images/2021-6-21/performance.jpg)

##### 2. 차지하는 메모리 양
- 차지하는 메모리에 양도 참조타입이 훨씬 많다.

64비트 컴퓨터 기준 비교표

![performance](/images/2021-6-21/memory.JPG)

##### 정리
- 기본적으로 성능과 메모리에 장점이 있는 원시 타입을 먼저 고려한다.
- 만약 null을 다뤄야 하거나, 제네릭 타입에서 사용되어야 한다면 참조 타입을 사용한다.
        
### 끝

    Java의 변수 - 기본형과 참조형, 래퍼 클래스에 대해 알아보았습니다.

-------------------------------------------------

Reference

<https://siyoon210.tistory.com/139>

<https://www.baeldung.com/java-primitives-vs-objects>

<https://stackoverflow.com/questions/2509025/when-to-use-primitive-and-when-reference-types-in-java>

<https://www.youtube.com/watch?v=xKj4N6eReQQ&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=16>

<https://gbsb.tistory.com/6>