---
layout: post
title: 📖 [Java] 💻 What is Collection?
category : Theory
tags: [theory, java, collection]
---

# 💻 Java Collection

    안녕하세요. 👋
    
    오늘은 자바의 Collection 에 대해서
    
    알아보려고 합니다.

## What is Collection? (Collection이란?)

    자바 Collection 이란
    여러 데이터, 자료구조의 집합이라고 볼 수 있습니다.
    
    JCF(Java Collections Framework)는 이러한 데이터, 자료구조인 Collection과
    이를 구현하는 클래스를 정의하는 인터페이스를 제공하고 있습니다.
    

## Collection 의 종류

![collection](/images/2020-5-16/collection.png)


    Collection 은 List, Set , Queue 로 크게 3가지 상위 인터페이스로 분류할 수 있습니다.
    Map의 경우에는 Collection 인터페이스를 상속받고 있지는 않지만 Collection으로 분류됩니다.
    
### 1.List
    List는 순서가 있는 데이터의 집합으로 데이터의 중복을 허용합니다.
    
   -   ArrayList
   
        배열과 유사하게 데이터에 대한 인덱스를 가지고 있어
        조회 기능에 성능이 뛰어납니다.
    
   -   LinkedList
  
        양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만
        수정하면 되기에 유용합니다.
    
        스택, 큐, 덱등을 만들기 위한 용도로 쓰입니다
    
   -   Vector
   
        과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화처리가 일어나
        비교적 성능이 좋지 않고 무거워 잘 쓰이지 않습니다.
    
### 2.Set
    
    Set은 데이터의 중복을 허용하지 않습니다.
    
   -    HashSet
        
        가장 빠른 임의 접근 속도를 가집니다.
        
        순서를 전혀 예측할 수 없습니다.
   -    TreeSet
   
        정렬방법을 지정할 수 있습니다.        
        
   -    LinkedHashSet
   
        Set을 순서대로 사용할 수 있습니다.
        
### 3.Queue
   
    List와 유사하고 FIFO (First In First Out) 선입선출의 자료구조입니다.
    주로 LinkedList로 구현됩니다.

### 4.map

    키(Key), 값(Value) 쌍으로 이루어진 데이터의 집합으로
    순서가 없으며 데이터는 값(value)는 중복이 가능하지만
    키(Key)는 중복이 불가능합니다.
    
   -    Hashtable
   
        HashMap 보다는 느리지만 동기화를 지원해줍니다.
        
        null 값 불가
        
   -    HashMap
   
        중복과 순서가 허용되지 않으며 null값이 올 수 있씁니다.
   
   -    TreeMap
   
        정렬된 순서대로 키(Key)와 값(Value)를 저장해 검색이 빠릅니다.
        


### 끝

    간단하게 Java의 Collection에 대해 
    알아보았습니다.
    감사합니다. 🙏

-------------------------------------------------

Reference

https://gangnam-americano.tistory.com/41
