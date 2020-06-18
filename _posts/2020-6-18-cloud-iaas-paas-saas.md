---
layout: post
title: 📖 [Java] 💻 클라우드 컴퓨팅과 IaaS PaaS SaaS 란
category : Theory
tags: [theory, cloud computing, Iaas, PaaS, SaaS]
---

# 💻 클라우드 컴퓨팅과 IaaS PaaS SaaS

    안녕하세요. 👋
    
    오늘은 클라우드 컴퓨팅과 IaaS PaaS SaaS 에 대해서
    
    알아보려고 합니다.

## What is Cloud Computing? (클라우드 컴퓨팅 이란?)

    클라우드 컴퓨팅(영어: cloud computing)은 클라우드(인터넷)을 통해 가상화된 컴퓨터의
    시스템리소스(IT 리소스)를 요구하는 즉시 제공(on-demand availability)하는 것이다.
        
    좀 더 간단히 말하자면 클라우드 컴퓨팅이란 가상화 기술을 이용해 컴퓨터의 자원을 분할하여
    사용자에게 원하는 만큼 컴퓨터 자원을 네트워크를 통해 제공해주는 것을 말합니다.
    
    이를 통해 클라우드 컴퓨팅을 이용하는 고객은 자신이 원하는 만큼씩만 자원을 사용할 수 있습니다.
    

## IaaS PaaS SaaS

클라우드 컴퓨팅에서 어느 정도로 자원을 제공해주냐에 따라
    
IaaS ,PaaS ,SaaS 로 나뉩니다.

    On-Premises 는 직접 개인이나 회사에서 직접 서버 컴퓨터를 두고
    서버를 돌리는 것을 말합니다.
    
    모든 것을 직접 관리해야합니다. 

![cloud](/images/2020-6-18/saas-vs-paas-vs-iaas.jpg)

    
### 1.IaaS
    
    서비스로서의 인프라스트럭처(Infrastructure as a Service, IaaS)는 
    서버, 스토리지, 네트워크를 필요에 따라 인프라 자원을 사용할 수 있게 클라우드 서비스를 제공하는 형태입니다.
 
    간단하게 말하자면 IaaS 는 컴퓨터 만을 제공받아서 서버 개발 환경을 직접 선택하여 
    개발 할 수 있습니다.
    
    OS, DB , 웹서버 등을 직접 구성해야합니다.
    
대표적으로 AWS EC2, Naver Cloud Platform , Google Cloud Platform, Azure Virtual Machines 등이 있습니다.

![AWS](/images/2020-6-18/aws.png)
![NCP](/images/2020-6-18/ncp.jpg)
![GCP](/images/2020-6-18/gcp.png)
![AZURE](/images/2020-6-18/azure.png)


### 2.PaaS

    PaaS (Platform as a Service)라고도하는 클라우드 플랫폼 서비스는 
    주로 애플리케이션에 사용되는 동안 특정 소프트웨어에 클라우드 구성 요소를 제공합니다. 
    PaaS는 개발자가 맞춤형 애플리케이션을 개발하고 구축 할 수있는 프레임 워크를 제공합니다. 
    모든 서버, 스토리지 및 네트워킹은 엔터프라이즈 또는 타사 공급자가 관리 할 수 있으며
    개발자는 응용 프로그램 관리를 유지할 수 있습니다.
    
    간단히 말하자면 PaaS 는 IaaS가 컴퓨터만을 제공해주었다면 더 나아가 플랫폼 까지 제공해주는 것 입니다.
    
    개발자가 직접 IaaS 에서 해야 했던 서버 개발 환경을 셋팅하지 않아도 됩니다.
    이미 PaaS에서 OS, DBMS, 개발 도구 등이 구성되어 있어서
    
    개발자는 단순히 완성된 코드만 올리기만 하면 됩니다.
    
대표적으로 AWS Elastic Beanstalk, Windows Azure, Heroku 등이 있습니다.

![HEROKU](/images/2020-6-18/heroku.png)

### 3.SaaS
    
    서비스로서의 소프트웨어(Software as a Service, SaaS)는 소프트웨어 및 관련 데이터는 중앙에 호스팅되고
    사용자는 웹 브라우저 등의 클라이언트를 통해 접속하는 형태의 소프트웨어 전달 모델입니다.
    주문형 소프트웨어(on-demand software, 온디맨드 소프트웨어)라고도 합니다.
    
    SaaS 는 다시 PaaS에서 나아가 소프트웨어까지 제공되는 서비스를 말합니다.
    이미 완성 된 서비스를 네트워크를 통해 제공해줍니다.
    
    보통 우리가 많이 사용하는 YouTube , Google docs 처럼 이미 서비스가 완성 된 상태로
    제공 되어지는 것을 말합니다. 
    
대표적으로 YouTube , Google docs 등이 있습니다.    

![YouTube](/images/2020-6-18/youtube.jpg)
![Google](/images/2020-6-18/google.jpg)

### 끝

    오늘은 클라우드 컴퓨팅과
    
    IaaS , PaaS , SaaS 에 대해
    
    알아보았습니다.
    
    감사합니다. 🙏
    

-------------------------------------------------

Reference

https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4%EB%A1%9C%EC%84%9C%EC%9D%98_%EC%9D%B8%ED%94%84%EB%9D%BC%EC%8A%A4%ED%8A%B8%EB%9F%AD%EC%B2%98


https://ko.wikipedia.org/wiki/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C_%EC%BB%B4%ED%93%A8%ED%8C%85


https://shlee0882.tistory.com/259