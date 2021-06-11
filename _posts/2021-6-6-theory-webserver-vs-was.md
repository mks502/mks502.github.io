---
layout: post
title: "💻 [Theory] Web Server와 WAS"
category : Theory
tags: [theory, oop]
---

# 💻 [Theory] Web Server와 WAS

    안녕하세요. 👋
    
    오늘은 Web Server와 WAS에 대해 알아보겠습니다.

## Web Server 란?

![webserver](/images/2021-6-6/web-server.svg)

- 클라이언트가 요청하는 HTML 문서나 각종 리소스 정적 컨텐츠를 전달하는 소프트웨어 이다.
- 위에서 말한 소프트웨어 외에도 해당 소프트웨어를 실행시키는 하드웨어 또한 WEB서버라고 말한다.
- 그러나 이 포스트에서는 <b>Apache</b> HTTP 서버, NGINX, WebtoB 와 같은 소프트웨어 에 해당하는 WEB서버에 대해서 말하도록 하겠다.

>웹 서버란 클라이언트(사용자)가 웹 브라우저에서 어떤 페이지 요청을 하면 웹서버에서 그 요청을 받아
><b>정적 컨텐츠를 제공하는 서버</b>이다. 여기서 정적 컨텐츠란 <b>단순 HTML문서 ,CSS, javascript,이미지,파일 등 즉시 응답가능한 컨텐츠<b/>이다.
>그렇다면 웹 서버는 정적 컨텐츠만 제공하나? 그것은 아니다.
>웹 서버가 동적 컨텐츠를 요청 받으면 WAS에게 해당 요청을 넘겨주고, WAS에서 처리한 결과를 클라이언트(사용자)에게 전달해주는 역할도 한다.

## WAS (Web Application Server) 란?

![was](/images/2021-6-6/was.png)

- Web Application Server 의 약자이다.
- 동적 컨텐츠를 제공하기 위해 만들어진 애플리케이션 서버로써 <b>웹 프로그램을 실행</b>할 수 있는 환경을 제공한다.
- 비즈니스 로직 수행이 가능하다.
- DB 접속 가능
- 웹 컨테이너(Web Container) 혹은 서블릿 컨테이너(Servlet Container)라고도 불린다.
    - Container 란 JSP, Servlet 을 실행시킬 수 있는 소프트웨어를 말한다.
    - 즉, WAS는 JPS, Servlet 구동 환경을 제공한다

- WAS는 Web Server + Web Container 로 정적인 컨텐츠도 처리가 가능하다.
    - 정적 컨텐츠 요청이 오면 Web Server 가 동적 컨텐츠 요청이 오면 Web Container 가 처리한다.

- 대표적인 WAS로는 <b>Tomcat</b>,JBoss, Jeus, WebLogic, WebSphere 등이 있다.

## Web Server와 WAS의 차이

간략히 말하자면

<b>Web Server 는 정적인 컨텐츠를 제공하는 서버</b>이고 <b>WAS는 동적인 컨텐츠를 제공하는 서버</b>이다.

예전의 WAS 에서는 정적인 Web Server 기능을 제공하지 않았지만 오랜 기간 WAS 가 발전을 거듭하여 최근 WAS 들은 Web Server 도

갖게 되어 <b>동적인 컨텐츠 뿐만 아니라 정적인 컨텐츠도 제공</b>해주고 있다.

그렇기 때문에, Web Server 없이 WAS 만으로도 모든 처리가 가능하다.

하지만 주로 서버 구성 시에 Web Server와 WAS를 분리하여 구성한다.

## 왜 Web Server와 WAS를 구분할까?

#### 1. 기능을 분리하여 서버 부하를 방지한다.

- WAS 가 혼자서 모든 요청을 처리할 수 있지만 그만큼 WAS 가 감당해야 하는 부담이 커지게 된다.

- <b>기능을 분리해서 각 서버가 감당하는 부하를 줄일 수 있도록 Web Server와 WAS 를 분리</b>한다.

- Web Server 와 WAS 를 분리하므로써 각자의 역할이 분명해진다.

#### 2. 물리적으로 분리하여 보안을 강화한다.

- WAS 에는 실제 Web Application 이 올라가 있기 때문에 외부와 직접 연결이 되어 있다면 중요한 정보나 리소스들이 외부로 노출될 수 있다.

- 이를 막기 위해서 Web Server 를 WAS 앞단에 배치해서 리소스를 안전하게 보호할 수 있다.

- WAS 앞단 Web Server에 SSL에 대한 암복호화 처리해 보안을 더 강화할 수도 있다. 

#### 3. Web Server에 여러 대의 WAS를 연결할 수 있다.

- 규모가 큰 서비스에서는 하나의 WEB서버에 하나의 WAS가 연결된 구조만으로는 많은 요청을 처리하는 데에 어려움이 생긴다.
- 그렇기 때문에 수많은 요청을 한군데가 아닌 여러 군데에서 처리를 할 수 있도록 동일한 Web Application 여러 개를 띄운다.
- 이때 여러 대의 WAS에 각각 요청을 들어오도록 하지 않고 앞에 WEB서버를 두고 각 WAS들을 WEB서버에 연결해서 WEB서버로 들어오는 수많은 요청을 각 WAS들에 적절하게 분배해주도록 한다.

- ※ 이렇게 배치하여 <b>로드밸런싱</b>을 해줌으로써 하나의 WAS가 처리하는 요청의 양이 줄어들어 안정적인 서비스 운영이 가능하다.
- ※ 더불어 fail over(장애 극복), fail back 도 가능

#### 4. 다른 종류의 WAS로 서비스 할 수 있다.

- Java 서버, PHP 서버와 같이 서로 다른 서버를 하나의 WEB서버에 연결해서 서비스 할 수 있다.

### 정리

>위처럼 다양한 이유가 있지만, 최근엔 WAS의 기능이 많이 강화되었기 때문에
><b> WEB서버와 WAS를 분리하는 가장 큰 이유는 로드밸런싱</b>이라고 볼 수 있다.

### 끝

    Web Server와 WAS에 대해 알아보았습니다.

-------------------------------------------------

Reference
