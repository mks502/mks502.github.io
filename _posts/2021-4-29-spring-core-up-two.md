---
layout: post
title: 📖 [Spring] 💻 스프링 핵심원리 - 2. 스프링 핵심 원리 이해 1,2
category : Spring
tags: [spring, spring core, core]
---

# 💻 [Spring] 스프링 핵심원리 - 스프링 핵심 원리 이해 1,2

    안녕하세요. 👋
    
    스프링으로 여러 프로젝트를 개발했었는데 
    
    문득 스프링 자체 핵심 원리에 대한 복습이 하고 싶었습니다.
    
    그래서 김영한님의 스프링 핵심원리 강의를 들으며 
    
    스프링을 왜 사용해야하는지와
    
    스프링 핵심원리에 대해 학습한 내용입니다. 
    
    두번째 시간입니다. 
    
    직접 순수 자바만을 이용해 객체 지향 원리를 적용해 예제 프로젝트를 개발해보았습니다.
    
    직접 실습해보며 느낀 주요 내용들을 위주로 정리하겠습니다.
        
## 1. 서론

스프링의 핵심 원리를 이해 하기 위한 예제가 필요했습니다.

순수 자바만을 이용해 다음의 비즈니스 요구사항을 가진 프로젝트를 개발해야합니다. 

    비즈니스 요구사항과 설계
    - 회원
        - 회원을 가입하고 조회할 수 있다.
        - 회원은 일반과 VIP 두 가지 등급이 있다.
        - 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동할 수 있다. (미확정)
    - 주문과 할인 정책
        - 회원은 상품을 주문할 수 있다.
        - 회원 등급에 따라 할인 정책을 적용할 수 있다.
        - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라. (나중에 변경 될 수 있
        다.)
        -할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 고민을
        미루고 싶다. 최악의 경우 할인을 적용하지 않을 수 도 있다. (미확정)
        
객체 지향의 핵심인 다형성을 이용해 역할(인터페이스) 과 구현 (실제 구현 객체)으로 나눠서 개발하여 자유롭게 구현 객체를 조립할 수 있게 했습니다.
다형성을 이용해 개발한 덕분에 회원 저장소는 물론이고, 할인 정책도 유연하게 변경할 수 있는 상태를 만들었습니다.
아무런 문제가 없어보이지만!
새로운 할인 정책을 개발하고 적용할 때 몇가지의 문제들이 생깁니다.

그 문제들을 중점으로 확인해보겠습니다.


## 2. 새로운 할인 정책 개발 시 문제점

두가지 문제점이 있습니다. 바로 앞선 [첫번째 포스트](https://mks502.github.io/spring-core-up-first/) 에서 나왔던 내용처럼

※ 다형성 만으로는 OCP, DIP를 지킬 수는 없었습니다.

1.기존의 고정 할인 정책
```java
package com.mks.core.discount;

import com.mks.core.member.Grade;
import com.mks.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{

    private int discountFixAmount = 1000; //1000원 할인

    @Override
    public int discount(Member member, int price) {
        if (member.getGrade() == Grade.VIP) {
            return discountFixAmount;
        }
        else{
            return 0;
        }
    }
}
```

2.새로운 정률 할인 정책
 
```java
package com.mks.core.discount;

import com.mks.core.member.Grade;
import com.mks.core.member.Member;

public class RateDiscountPolicy implements DiscountPolicy {

    private int discountPercent = 10;

    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return price * discountPercent / 100;
        }
        else {
            return 0;
        }
    }
}

```

3.OrderServiceImpl 클래스

```java
public class OrderServiceImpl implements OrderService {
    
    private final MemberRepository memberRepository = new MemoryMemberRepository();
    
    //OrderServiceImpl에서 직접 주입해주기 때문에 할인 정책이 바뀔때 마다 변경해줘야함 
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
    // private final DiscountPolicy discountPolicy = new FixDiscountPolicy(); 


    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);


        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

우선 위의 1번처럼 기존의 고정 할인 정책이 있었습니다. 새로운 할인 정책을 만들고 적용해야 하더라도 역할과 구현을 잘 구분했기 때문에 2번처럼 새로운 정률 할인 정책을
쉽게 만들 수 있습니다.

아무런 문제점이 없어보이지만 <b>3번 OrderServiceImpl 클래스를 보면 두가지 문제점</b>을 발견할 수 있습니다.

#### 👉 DIP 원칙 위반 - 추상화와 구체화 둘다 의존

OrderServiceImpl 클래스는 MemberRepository 인터페이스와 DiscountPolicy 인터페이스를 의존하면서 구현 클래스인 MemoryMemberRepository와 RateDiscountPolicy 까지도 의존하고 있습니다.
OrderServiceImpl 클래스가 직접 각 인터페이스에 직접 구현체를 주입하며 인터페이스, 구현체 모두를 의존하고 있음을 알 수 있습니다.

이는 <b>DIP 원칙에 위반</b> 되는 것입니다. 

#### 👉 OCP 원칙 위반 - 할인 정책 변경 시마다 client 코드를 수정

OrderServiceImpl 클래스의 주석에 써 있듯이 OrderServiceImpl 에서 직접 실제 할인 정책 구현체를 주입해주기 때문에 할인 정책이 바뀔때 마다 직접 변경해줘야 합니다.

이는 구현 객체를 변경하기 위해 client 코드를 수정해야 하기 때문에 <b>OCP 원칙에 위반</b>되는 것 입니다.

## 3. 해결책 - 관심사의 분리!

현재 상태는 주문 서비스 구현체가 주문 서비스만을 책임지고 제공하는 것 뿐만 아니라 어떤 할인 정책을 사용해야하는지까지 직접 고르며 책임지고 있습니다.

다양한 책임을 갖고 있는 것 입니다. 이는 또 SRP 원칙을 위반하기도 합니다.

관심사를 분리할 필요성이 생깁니다. 객체는 자신의 사용 역할에만 충실하면 되지 구성까지 책임질 필요가 없기 때문입니다.

애플리케이션의 전체 동작 방식을 구성해주기 위한 (객체 생성 및 연결(주입)) <b>별도의 설정 class</b>가 필요해집니다.! 

<b>관심사의 분리가 이뤄집니다! </b>

    => 객체 생성 및 연결(주입)과 비즈니스 로직을 실행하는 부분이 각각의 책임을 갖고 분리가 됩니다. (실행 책임 / 구성 책임)

AppConfig.java
```java
public class AppConfig {
    // 애플리케이션 전체 동작 방식을 구성해주기 위한 별도의 클래스 (객체 생성 및 연결/주입만을 담당함)
    public MemberService memberService(){
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService(){
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```

AppConfig 클래스 내의 memberService 메소드는 MemberService의 구현체인 MemberServiceImpl를 반환합니다.
MemberServiceImpl 객체를 생성할 때 MemberRepository의 구현체인 MemoryMemberRepository 객체를 주입해줍니다.

    위와 같이
    MemberServiceImpl 생성자를 통해 구현체인 MemoryMemberRepository 객체를 주입해주는 것을 생성자를 통한 의존성 주입이라고 합니다.

MemberServiceImpl.java
```java
public class MemberServiceImpl implements MemberService {

    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

MemberServiceImpl는 이제 MemberRepository 인터페이스에만 의존하게 되므로 <b>DIP 원칙을 지키게 됩니다.</b>

OrderServiceImpl.java
```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member,itemPrice);

        return new Order(memberId,itemName,itemPrice,discountPrice);
    }
}
```

OrderServiceImpl는 MemberRepository 인터페이스와 DiscountPolicy 인터페이스에만 의존하므로 마찬가지로 <b>DIP 원칙을 지키게 됩니다.</b>
위와 같이 MemberServiceImpl와 OrderServiceImpl는 <b>인터페이스에만 의존</b>하게 함으로써 코드상으로는 어떠한 구현 객체가 들어올지는 알 수 없고, 생성자를 통해 외부에서 구현객체를 주입 받게 됩니다.

AppConfig class의 등장으로 설정 class (AppConfig) 를 통해 객체의 생성과 연결을 시키고 MemberServiceImpl와 OrderServiceImpl는 각자 자신의 비즈니스 로직을 실행만 하면 되므로 역할의 분리가 이뤄지게 됩니다.
이렇게 외부에서 MemberServiceImpl와 OrderServiceImpl에 실제 구현 객체를 넣어주는 것을 DI (의존성 주입)이라고 합니다.

AppConfig.java
```java
public class AppConfig {
    public MemberService memberService (){
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    public DiscountPolicy discountPolicy (){
    //return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

또한 위와 같은 AppConfig 등장 이전에는 기존 할인 정책에서 새로운 할인 정책을 바꿀 때 OrderServiceImpl 에서 직접 실제 할인 정책 구현체를 주입해주기 때문에
OrderServiceImpl을 직접 변경해야했습니다. 그로인해 OCP 원칙이 위반되었었습니다.

하지만 위와 같은 AppCofig의 등장하며 실제 구현 객체를 넣어주는 역할을 담당하기 때문에
AppConfig 에서만 어떤 구현체를 사용할지 정하면 되게 되었습니다.

이로 인해 <b>OCP 원칙</b> 또한 지켜지게 되었습니다.


### AppConfig 등장 결과

AppConfig의 등장으로 구현 객체들은 자신의 로직을 실행하는 역할만 담당하게 되었습니다.

구현 객체의 주입을 AppConfig 가 담당하게 되며 

프로그램의 제어 흐름은 이제 AppConfig 에서 담당하게 되었습니다.

이렇게 명확하게 

실제 로직을 담당하는 역할과

각 구현 객체들을 주입시켜주며 프로그램의 제어 흐름을 담당해줄 역할이 나뉘며

객체지향의 원칙 중 위반되었던 OCP 원칙 DIP 원칙까지 지킬 수 있게 되었습니다.


## 끝
    
    스프링을 더 올바르게 사용하기 위해 김영한님의 스프링 핵심 원리 강의를 들으며 정리한
    두번째 포스트였습니다.
    스프링의 정말 핵심이 되는 DI (의존성 주입)과 IoC (제어의 역전)에 대해 다시 한번
    알아보고 더 명확히 알 수 있었습니다.
    감사합니다. 🙏

스프링 핵심원리 첫번째 포스트 

[1. 객체 지향 설계와 스프링](https://mks502.github.io/spring-core-up-first/)    

-------------------------------------------------

Reference

[김영한님의 스프링 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8#)
