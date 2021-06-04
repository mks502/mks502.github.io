---
layout: post
title: 📖 [Spring] 💻 스프링 핵심원리 - 4. 스프링 컨테이너와 스프링 빈
category : Spring
tags: [spring, spring core, core]
---

# 💻 [Spring] 스프링 핵심원리 - 4. 스프링 컨테이너와 스프링 빈

    안녕하세요. 👋
    
    스프링으로 여러 프로젝트를 개발했었는데 
    
    문득 스프링 자체 핵심 원리에 대한 복습이 하고 싶었습니다.
    
    그래서 김영한님의 스프링 핵심원리 강의를 들으며 
    
    스프링을 왜 사용해야하는지와
    
    스프링 핵심원리에 대해 학습한 내용입니다. 
    
    네번째 포스트입니다.
        
## 스프링 컨테이너 생성

    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
    
- 스프링 컨테이너를 부를 때 BeanFactory , ApplicationContext 로 구분해서 이야기
  하지만 BeanFactory 를 직접 사용하는 경우는 거의 없으므로 <b>일반적으로 ApplicationContext 를 스프링 컨테이너</b>라 한다.
  
- ApplicationContext 는 인터페이스이다.

- 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
직전에 AppConfig 를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것
이다.

- 자바 설정 클래스를 기반으로 스프링 컨테이너( ApplicationContext )를 만들어보자.

        new AnnotationConfigApplicationContext(AppConfig.class);
        
이 클래스는 ApplicationContext 인터페이스의 구현체이다.

### 스프링 컨테이너 생성 과정

#### 1.스프링 컨테이너 생성

![create](/images/2021-5-25/create.JPG)

    new AnnotationConfigApplicationContext(AppConfig.class)
    
- 스프링 컨테이너를 생성할 때는 구성 정보를 지정해주어야 한다.
- 여기서는 AppConfig.class 를 구성 정보로 지정했다.

![bean](/images/2021-5-25/bean.JPG)

- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.
- 빈 이름
    - 빈 이름은 기본적으로 메서드 이름을 사용한다.
- 빈 이름을 직접 부여할 수 도 있다.
    - ex) @Bean(name="memberService2")  => memberService2 로 bean name을 부여
 
> 주의: 빈 이름은 항상 다른 이름을 부여해야 한다. 같은 이름을 부여하면, 다른 빈이 무시되거나, 기존 빈을
덮어버리거나 설정에 따라 오류가 발생한다.

![ready](/images/2021-5-25/ready.JPG)

![fin](/images/2021-5-25/fin.JPG)

- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 단순히 자바 코드를 호출하는 것 같지만, 차이가 있다.
- 스프링 컨테이너는 등록된 스프링 빈들을 싱글톤 패턴으로 생성하고 관리해준다.

> 스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다. 그런데 이렇게 자바 코드로 스프링 빈
을 등록하면 <b>생성자를 호출</b>하면서 <b>의존관계 주입</b>도 <b>한번</b>에 처리된다. 

정리

스프링 컨테이너를 생성하고, 설정(구성) 정보를 참고해서 스프링 빈도 등록하고, 의존관계도 설정했다.
이제 스프링 컨테이너에서 데이터를 조회해보자.

## 끝
    
    스프링을 더 올바르게 사용하기 위해 김영한님의 스프링 핵심 원리 강의를 들으며 정리한
    네번째 포스트였습니다.
    감사합니다. 🙏

스프링 핵심원리 포스트

[1. 객체 지향 설계와 스프링](https://mks502.github.io/spring-core-up-first/)    
[2. 스프링 핵심 원리 이해 1,2](https://mks502.github.io/spring-core-up-two/)
[3. IoC, DI, 그리고 컨테이너](https://mks502.github.io/spring-core-up-three-ioc-di-container/)

-------------------------------------------------

Reference

[김영한님의 스프링 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8#)
