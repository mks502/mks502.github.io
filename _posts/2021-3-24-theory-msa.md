---
layout: post
title: 📖 [Theory] 💻 MSA (Micro Service Architecture)란?
category : Theory
tags: [theory, msa, micro service architecture]
---

# 💻 MSA (Micro Service Architecture)란?
    
    마이크로서비스는 애플리케이션 구축을 위한 아키텍처 기반의 접근 방식입니다. 마이크로서비스를 전통적인 모놀리식(monolithic)
    접근 방식과 구별 짓는 기준은 애플리케이션을 핵심 기능으로 세분화하는 방식입니다.
    각 기능을 서비스라고 부르며, 독립적으로 구축하고 배포할 수 있습니다.
    이는 개별 서비스가 다른 서비스에 부정적 영향을 주지 않으면서 작동(또는 장애가 발생)할 수 있음을 의미합니다.
    (https://www.redhat.com/ko/topics/microservices/what-are-microservices)
    
즉, "하나의 큰 어플리케이션을 여러개의 작은 어플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍쳐" 입니다.

하나의 큰 어플리케이션을 각각 독립적으로 수행되는 여러개의 작은 어플리케이션으로 구성하면 

작은 어플리케이션 단위의 서비스를 재사용하기 쉽고 문제가 발생하더라도 커다란 어플리케이션 전부를 수정할 필요 없이

문제가 생긴 작은 어플리케이션만 수정을 하면 됩니다. 또한 독립적으로 수행되므로 각 서비스에 가장 유리한 방식으로

독립적으로 개발이 가능합니다. 

점차 요구되는 서비스가 커지게 되면서 등장하게 되었습니다.

또한 서비스들이 각각 독립적으로 수행되므로 서비스들간 잘 설계 된 API 통신을 통해 서비스 제공이 이뤄집니다.
        
## MSA (Micro Service Architecture)의 등장 배경

![msa](/images/2021-3-24/msa.png)

MSA (Micro Service Architecture)가 등장하기 전

예전부터 많이 사용되어왔고 현재에도 많이 남아있는 전통적인 Monolithic Architecture 형태가 있습니다.

Monolithic Architecture란, 소프트웨어의 모든 구성요소가 하나의 프로젝트에 통합되어있는 형태입니다.

아직까지는 많은 소프트웨어가 Monolithic 형태로 구현되어 있고, 소규모 프로젝트에는 Monolithic Architecture가 훨씬 합리적입니다.

하나로 통합되어 개발되기에 간단한 Architecture이고, 유지보수가 용이하기 때문입니다. 

하지만 점차 요구되는 기능이 커지며 그에 따라

일정 규모 이상의 서비스, 혹은 여러 개발자가 협업하는 프로젝트에서 Monolithic Architecture은 뚜렷한 한계를 보입니다.

    👉 부분장애가 전체 서비스의 장애로 이어지는 경우가 많습니다.
        - 부분적인 장애로 인해 전체 서비스가 다운이 되는 치명적인 경우도 생길 수 있습니다.
    
    👉 하나로 통합되어 있기 때문에 테스트/빌드시 애플리케이션 전체를 테스트/빌드 해야합니다.
        - 기능 수정/추가 에러 수정 등으로 테스트/빌드를 해야할 때 전체를 테스트 / 빌드 해야하기 때문에 상당한 시간이 소요됩니다.
    
    👉 서비스 / 프로젝트가 커지면 커질수록 전체 시스템 구조 영향도 파악이 어렵습니다.
        - 여러 컴포넌트가 하나의 서비스로 강하게 결합되어 있어 파악이 규모가 커질수록 파악이 어렵습니다.
        
    👉 하나의 언어와 프레임워크에 종속적이게 됩니다.
        - 인공지능 / 블록체인 등 특정분야마다 유리한 언어와 프레임워크가 있지만 하나의 전체 서비스 개발 언어/프레임워크에 종속되게 됩니다.
    
    👉 부분적인 Scale-out이 어렵습니다.
        - 부분적인 서비스에 대해 갑작스런 대용량 트래픽이 몰리는등 Scale-out이 필요할 때 트래픽이 몰리는 서비스에 대해서만 Scale-out을 
          하는 것이 아닌 전체 서비스에 대해 Scale-out을 해주어야합니다.
          
이러한 Monolithic Architecture의 한계 때문에 MSA (Micro Service Architecture)가 등장하게 되었습니다.

## MSA의 장점

MSA (Micro Service Architecture)는 Monolithic Architecture에서 서비스가 커짐에 따라
생겼던 문제점들로 인해 등장한만큼 해당 문제점들에 대해 보완이 가능합니다.
 
    배포(deployment) 관점
    👉 각 서비스 별 개별 배포 가능 ( 배포 시 전체 서비스의 중단이 없음 )
        - 해당 되는 서비스만 재배포 하면 됩니다.
        - 요구사항을 신속하게 반영하여 빠르게 배포할 수 있습니다.
        
    확장(scaling) 관점
    👉 특정 서비스에 대한 확장성이 용이함.
        - 전체 서비스를 확장할 필요없이 확장이 필요한 서비스만 확장이 가능합니다. 
        - 클라우드 사용에 적합한 아키텍쳐입니다.
        - 컨테이너 기술 사용에 적합한 아키텍쳐입니다.
    
    장애(failure) 관점
    👉 장애가 전체 서비스로 확장될 가능성이 적음
        - 서비스가 모두 분리되어 있기 때문에 장애가 난 서비스에만 문제가 생깁니다.
        - 장애가 발생한 서비스를 제외 한 나머지 서비스들은 정상적으로 제공이 가능합니다.

## MSA의 단점

MSA (Micro Service Architecture)는 장점이 많지만 그만큼 Monolithic Architecture에 비해 

복잡하고 고려해야할 것이 많습니다. 그에 따른 단점들입니다.

    성능 
        👉 서비스 간 호출 시 API 통신을 사용하기 때문에, 통신 비용이나, Latency가 그만큼 늘어나게 됩니다.
    테스트 / 트랜잭션
        👉 서비스가 분리되어 있기 때문에 테스트와 트랜잭션의 복잡도가 증가하고, 많은 자원을 필요로 합니다.
    데이터 관리 
        👉 데이터가 여러 서비스에 걸쳐 분산되기 때문에 한번에 조회하기 어렵고, 데이터의 정합성 또한 관리하기 어렵습니다



## 정리

    MSA (Micro Service Architecture)에 대해 알아보았습니다.
    
    MSA (Micro Service Architecture)

-------------------------------------------------

참고

https://www.redhat.com/ko/topics/microservices/what-are-microservices

https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e

https://wooaoe.tistory.com/57