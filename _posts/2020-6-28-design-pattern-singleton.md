---
layout: post
title: 📖 [Design Pattern] 💻 디자인 패턴 Singletone Pattern
category : Theory
tags: [theory, Design Pattern, Singletone]
---

# 💻 [Design Pattern] 싱글톤 패턴

    안녕하세요. 👋
    
    디자인 패턴 중
    
    Java 와 Spring 에서 주로 사용되는
    
    Singletone Pattern 에 대해 알아보겠습니다.

# 💻 싱글톤 패턴 이란 무엇인가?
    
    소프트웨어 디자인 패턴에서 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도
    실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.
    이와 같은 디자인 유형을 싱글턴 패턴이라고 한다.
    주로 공통된 객체를 여러개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용된다.
    
    
    즉 Singleton 이름 그대로 하나의 객체만을 생성해서 사용하는 디자인 패턴을 말합니다.
    단 하나의 최초 생성된 객체를 공용으로 사용하기 때문에 멀티쓰레드 환경에서 Thread-Safe 가 중요합니다.

## 1. 싱글톤 패턴의 사용이유?
    
    싱글톤 패턴은 주로 자바와 Spring Framework 에서 사용됩니다.
    
    싱글톤 패턴은 최초 한번 객체를 생성해 재사용 할 수 있기에 메모리 낭비를 방지할 수 있습니다.
    이렇게 최초 한번 생성된 싱글톤 객체는 전역성을 갖고 있기에 어디서나 사용할 수 있습니다.

## 2. 싱글톤 패턴 구현 방법

1) Eager Init
```java
public class EagerInit {
    private static EagerInit uniqueInstance = new EagerInit();

    private EagerInit(){

    }

    public static EagerInit getInstance(){
        return uniqueInstance;
    }

}
```
    가장 쉽게 만들 수 있는 Singletone Pattern으로 
    static 키워드의 특징을 이용해서 클래스 로더가 초기화 하는 시점에서 정적 바인딩(static binding)을 통해
    해당 인스턴스를 메모리에 등록해서 사용하는 것입니다.
    Eager Init 방식은 Thread-Safe 하지 않습니다.
    Singletone 구현 시 중요한 점이, 멀티 스레딩 환경에서도 동작 가능하게끔 구현해야 한다는 것입니다.
    즉, Thread-safe 가 보장되어야 합니다.
2) Lazy init

```java
package singletone;
public class LazyInit {
    private static LazyInit uniqueInstance;

    private LazyInit() {
    }

    public static LazyInit getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new LazyInit();
        }
        return uniqueInstance;
    }
}
```
    Eager Init 을 개선시켜 클래스 로딩 시점이 아닌 인스턴스가 필요하여 요청시 동적 바인딩을 통해 구현 가능합니다.
    하지만 여러 쓰레드에서 getInstance에 접근할 수 있어 Thread-Safe 하지 않습니다.
                    
3) synchronized 방법 

```java
package singletone;

public class Synchronize {

    private static Synchronize uniqueInstance;

    private Synchronize() {
    }

    public static synchronized Synchronize getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Synchronize();
        }
        return uniqueInstance;
    }
}

```
    synchronized 키워드를 통해서 동기화를 통해 Lazy 초기화와 Thread-Safe 하게 싱글톤을 구현할 수 있습니다.
    하지만 성능이 상당히 좋지 못합니다.

4) Lazy Holder 방법

```java
package singletone;

public class Holder {

    private Holder() {}
    private static class InnerInstanceClazz() {
        // 클래스 로딩 시점에서 생성
        private static final Holder uniqueInstance = new Holder();
    }

    public static Holder getInstance() {
        return InnerInstanceClazz.instance;
    }

}
```
    Holder 방법은 Singletone Pattern 구현에 가장 많이 쓰이는 대표적인 형태입니다.
    Holder 클래스에는 InnerInstanceClazz 클래스의 변수가 없어서
    static 멤버 클래스지만 클래스 로더가 초기화 과정을 진행할 때
    InnerInstanceClazz 메서드를 초기화 하지않고, getInstance() 메서드 호출 시 초기화 됩니다.
    동적바인딩(Dynamic Binding) 의 특징을 이용하여 Thread-safe 하면서 성능이 뛰어납니다
    
    InnerInstanceClazz 내부 인스턴스는 static 이기 때문에 클래스 로딩 시점에 한번만 호출된다는 점을 이용한것이고
    final을 써서 값이 다시 할당되지 않도록 합니다.
    
### 끝

    Design Pattern 중 자바와 Spring Framework 에서
    많이 사용되는 Singletone Pattern에 대해 알아보았습니다.
    감사합니다. 🙏
    

-------------------------------------------------

Reference

https://elfinlas.github.io/2019/09/23/java-singleton/

https://medium.com/webeveloper/%EC%8B%B1%EA%B8%80%ED%84%B4-%ED%8C%A8%ED%84%B4-singleton-pattern-db75ed29c36

