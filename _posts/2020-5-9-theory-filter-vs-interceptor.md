---
layout: post
title: 📖 [Theory] 💻 Filter & Interceptor
category : Theory
tags: [theory, security]
---

# 💻 🔐 Filter & Interceptor

    Spring 으로 API 서버를 구현할 때 서버에 보안성을 위해 인증(authentication)을 구현해야합니다.
    인증(authentication)을 구현하기 위해서 Filter 또는 Interceptor를 사용됩니다.
    기능 자체는 비슷하지만 차이점이 있습니다.
    해당 차이점을 알아보겠습니다.
    
## 🔑 Filter VS Interceptor

![spring-request-lifecycle](/images/2020-5-9/spring-request-lifecycle.jpg)



차이점| Filter | Interceptor |
|------|:---------:|:---------:| 
|스펙|J2EE 표준|Spring Framework|
|등록|Web Application|Spring context|
|실행 시점|DispatcherServlet 앞 | DispatcherServlet 과 Handler 사이 |
|실행 순서|1|2

## 정리

    Filter 와 Interceptor은 비슷한 기능을 하지만 확연히 다릅니다.
    
    우선 Filter 는 J2EE 표준으로 제공해주는 기능입니다.
    
    Interceptor는 서버로의 요청 전후 처리를 편하게 해주기 위해 Spring Framework에서
    
    제공해주는 기능입니다.
    
    또한 등록 시점도 Filter는 Web Application 자체에 등록을 
    
    해주고 Interceptor는 Spring 의 context에 등록을 해줍니다.
    
    맨 위의 그림과 같이 우선 어떠한 요청이 들어오면 Filter 부터 거치게 되고 추후에
    
    DispatcherServlet 에서 Handler로 연결하는 과정에서 Interceptor가 요청을 가로채
    
    확인을 합니다.
    
    Filter를 무조건 거쳐서 시작하기 때문에 주로 전역적으로 무엇인가를 처리해야 할 때는
    
    Filter를 사용하고 디테일한 처리는 주로 Interceptor를 사용합니다.  

-------------------------------------------------

참고

https://justforchangesake.wordpress.com/2014/05/07/spring-mvc-request-life-cycle/

https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/

https://www.leafcats.com/39