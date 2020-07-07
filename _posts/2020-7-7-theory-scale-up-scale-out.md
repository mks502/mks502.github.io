---
layout: post
title: 📖 [Theory] 💻 Scale-up & Scale-out
category : Theory
tags: [theory, server, storage]
---

# 💻 [Theory] 스케일 업(Scale-up)과 스케일 아웃(Scale-out)

    안녕하세요. 👋
    
    오늘은 서버에 대용량 트래픽이 몰려 서버가 다운이 되는 현상을 보신 적이 있을 겁니다.
    
    서버를 운영하다 많은 사용자의 증가와 처리하는 서비스 증가 등의 이유로 
    
    기존 서버로는 처리가 힘들어지는데요 이럴 때 서버 확장을 고려하게 됩니다.
    
    오늘은 서버 확장의 두가지 방법인 Scale-up 과 Scale-out 에 대해 알아보겠습니다.
        
# 💻 1. Scale-up 이란?

   Scale-up 이란 기존의 서버 자체의 성능을 높이는 것을 말합니다.
   
   즉 기존 서버에 리소스를 늘려 처리 능력을 향상시키는 것 입니다.
   
   보통 프로세서 자체를 추가하거나 프로세서 그 자체를 고성능 모델로 
   
   옮겨놓는 것을 말합니다.
   
   수직 스케일로 불리기도 합니다.
   
#### Scale-up 단점
    
    Scale-up의 단점으로는 스토리지 컨트롤러의 확장성 한계의 문제, 성능 그리고 용량 확장 제한에
    다 다른 경우에 새 시스템을 추가해야되는데 이때 발생하는 마이그레이션 비용 등이 있습니다.
    

## Scale-out 이란?

 Scale-out 이란 서버의 대수를 늘려서 처리 능력을 향상 시키는 것을 말합니다.
 
 Scale-out은 Scale-up에 비해 비용이 저렴하고 확장이 유연합니다.
 
 수평 스케일로 불리기도 합니다.
 
 #### Scale-out 단점
 
    Scale-out 의 단점은 여러 노드를 연결해 병렬 컴퓨팅 환경을 구성하고
    유지하기 위해 아키텍처에 대한 높은 이해도가 요구됩니다. 그리고 또한
    여러 노드에 트래픽을 균등하게 분산시키기 위해 로드 밸런싱 (load balancing)이
    필요하고 노드를 확장할수록 문제 발생의 잠재 원인도 늘어납니다. 

## Scale-up , Scale-out 정리

![UNCOMMITTED](/images/2020-7-7/scaleup_scaleout_comparison.png)

    Scale-up 과 Scale-out 둘다 서버 확장을 위한 방법이며
    두 방식 중 어떠한 방식이 더욱 좋다라기 보다는 주어진 상황에 따라 어떤 방식을 채택해
    사용해야 효율적인지를 따져봐야 합니다.
    예를 들면, 어떤 서비스의 사용자가 증가해서 트래픽이 많이 발생할 경우에는 scale-out이 scale-up 보다는 효과적이며,
    비용도 저렴합니다. 그 반면, 데이터베이스의 빈번한 갱신이 필요한 OLTP(온라인 트랜잭션)에서는
    scale-out보다는 scale-up이 효과적입니다.

    
### 끝
    
    서버 확장시 이용되는 두가지 방법인
    Scale-up 과 Scale-out에 대해 알아보았습니다.
    감사합니다. 🙏
    

-------------------------------------------------

Reference

https://tech.gluesys.com/blog/2020/02/17/storage_3_intro.html

https://toma0912.tistory.com/87