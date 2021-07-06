---
layout: post
title: "💻 [Spring] Spring MVC 구조, 처리 과정"
category : Spring
tags: [spring, spring mvc]
---

# 💻 [Spring] Spring MVC 구조, 처리 과정

    안녕하세요. 👋
    
    이번에는 Spring Framework의 근간이 되는  
    Spring MVC 구조와 처리 과정에 대해 알아보도록 하겠습니다.
   
        
## Spring MVC 구조
![spring-mvc-life-cycle](/images/2021-6-11/spring-mvc-life-cycle.JPG)

## Spring MVC 처리 과정

클라이언트가 서버에 어떤 요청(Reuqest)을 한다면 Filter의 유무에 따라 Filter를 거치게 된다.
- 대표적으로 Spring Security의 필터가 있다.
- 요청에 대한 처리 후 반환시에도 마찬가지로 Filter의 유무에 따라 거치게 된다.

1.클라이언트 요청을 <b>DispatcherServlet</b>이 가로챈다. 

2.요청을 가로챈 DispatcherServlet은 <b>HandlerMapping</b>(URL 분석등..)에게 어떤 컨트롤러에게 요청을 위임하면 좋을지 물어본다.

- HandlerMapping은 servlet-context.xml에서 @Controller로 등록한 것들을 스캔해서 찾아놨기 때문에 어느 컨트롤러에게 요청을 위임해야할지 알고 있다.

- Interceptor 의 유무에 따라 Interceptor 에서 요청이 거쳐지게 될 수도 있다. (요청 처리 후 반환 시에도 동일) 

3.요청에 매핑된 컨트롤러가 있다면 @RequestMapping을 통하여 요청을 처리할 메서드에 도달한다.

4. 컨트롤러에서는 해당 요청을 처리할 Service를 주입(DI)받아 비즈니스로직을 Service에게 위임한다.

5. Service에서는 요청에 필요한 작업 대부분(기능)을 담당하며 데이터베이스에 접근이 필요하면 DAO를 주입받아 DB처리는 DAO에게 위임한다.

6. DAO는 jdbc를 이용해서 DB에 접근해 저장되어있는 정보를 받아 서비스에게 다시 돌려준다.

(이 때, 보통 Request와 함께 날아온 DTO 객체(@RequestParam, @RequestBody, ...)로 부터 DB 조회에 필요한 데이터를 받아와 쿼리를 만들어 보내고, 결과로 받은 Entity 객체를 가지고 Response에 필요한 DTO객체로 변환한다.)

7. 모든 비즈니스 로직을 끝낸 서비스가 결과물을 컨트롤러에게 넘긴다.

8. 결과물을 받은 컨트롤러는 필요에 따라 Model 객체에 결과물 넣거나, 어떤 view(jsp)파일을 보여줄 것인지등의 정보를 담아 DispatcherServlet에게 보낸다.

9. DispatcherServlet은 ViewResolver에게 받은 뷰의 대한 정보를 넘긴다.

10. ViewResolver는 해당 JSP를 찾아서(응답할 View를 찾음) DispatcherServlet에게 알려준다.

(servlet-context.xml에서 suffix, prefix를 통해 /WEB-INF/views/index.jsp 이렇게 만들어주는 것도 ViewResolver)

11. DispatcherServlet은 응답할 View에게 Render를 지시하고 View는 응답 로직을 처리한다.

12. 결과적으로 DispatcherServlet이 클라이언트에게 렌더링된 View를 응답한다.


### 정리

> ( )는 거칠 수도 있고 안 거칠 수도 있음.

요청 -> (Filter) -> DispatcherServlet -> HandlerMapping -> (Interceptor) -> Controller -> Service -> Controller -> (Interceptor) -> DispatcherServlet -> ViewResolver -> (Filter) -> View


## 끝
    
    Spring MVC 구조, 처리 과정에 대해 알아보았습니다.
    감사합니다. 🙏

-------------------------------------------------

같이 보면 좋을 포스팅

[Filter 와 Interceptor](https://mks502.github.io/theory-filter-vs-interceptor/)

Reference

<https://jeong-pro.tistory.com/96>
