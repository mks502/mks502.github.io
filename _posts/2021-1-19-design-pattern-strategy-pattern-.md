---
layout: post
title: 📖 [Design Pattern] 💻 Strategy Pattern
category : Design Pattern
tags: [design pattern, strategy]
---

# 💻 Strategy Pattern (전략적 패턴)

    안녕하세요. 👋
    
    오늘은 Strategy Pattern ( 전략적 패턴 ) 에 대해
    
    알아보려고 합니다.

## What is Strategy Pattern? (전략적 패턴이란?)

- Strategy Pattern 이란
   행위를 클래스로 캡슐화해 동적으로! 행위를 자유롭게
   바꿀 수 있게 해주는 디자인 패턴입니다.
   상황에 맞게 끔 전략(정책)에 맞게 끔 행위를 지정해줍니다. 

## 예시

여러 전투기가 있다고 가정을 했을 때
전투기는 공통적으로 공격을 할 무기가 있습니다.

하지만 전투기의 종류에 따라 각각 무기는 다릅니다.

또한 상황에 따라 무기가 달라질 수도 있습니다.

이럴때 전투기의 공통적인 행위에 대해 각각 상황에 맞게 
전략에 맞게 정해주는 것이 전략적 패턴입니다.

## 전략적 패턴의 사용 이유

전투기 추상 클래스
```java
package strategy;

public abstract class Fighter implements AttackStrategy{
    private String name;

    public Fighter(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }


}

```

F2 전투기
```java
package strategy;

public class F2 extends Fighter{
    public F2(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println("미사일로 공격");
    }
}

```

전략적 패턴의 사용이유는 다음과 같습니다. 위처럼 상속을 이용한다면
기능을 추가하거나 변경하려면 기존의 코드를 변경해야 합니다.
이는 OCP(Open-Closed Principle) 법칙에 위배됩니다.

또한 이런 방식은 시스템이 확장이 되었을 때 유지보수를 힘들게 합니다.
예를들어 F2 의외에 다른 여러 종류의 전투기들이 생겨날 때
새로운 종류의 모든 전투기들이 대해서 attck 메서드를 일일이 
수정해야하며 같은 메서드를 여러 클래스에서 똑같이 정의하고 있으므로 
메서드의 중복문제가 발생합니다.

---------


### 전략적 패턴 적용

전투기 추상클래스

```java
package strategy;

public abstract class Fighter {
    private String name;
    private AttackStrategy attackStrategy;

    public Fighter(String name) {
        this.name = name;
    }

    public void attack(){
        attackStrategy.attack();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public AttackStrategy getAttackStrategy() {
        return attackStrategy;
    }

    public void setAttackStrategy(AttackStrategy attackStrategy) {
        this.attackStrategy = attackStrategy;
    }

}
```

MissileAttack 구현체

```java
package strategy;

public class MissileAttack implements AttackStrategy{
    @Override
    public void attack() {
        System.out.println("미사일 공격");
    }
}

```

GunAttack 구현체
```java
package strategy;

public class GunAttack implements AttackStrategy{
    @Override
    public void attack() {
        System.out.println("총으로 공격");
    }
}

```

---------------

전투기의 공격 형태를 캡슐화 하여 상속보다는 구성 (Composition)을 이용합니다.
공격이라는 행위(행동)을 캡슐화 하여 Flight 클래스의 변경을 막습니다.
새로운 기능을 추가 시에 기존의 코드에 영향을 미치지 않고 
AttackStrategy 인터페이스를 상속 받아 새로운 기능의 클래스를 만들면 됩니다.
로직을 독릭적으로 관리하기 쉽습니다.

또한 setter를 이용해 외부에서 상황에 맞게 필요에 따라 전략적으로
동작할 기능을 변경할 수 있습니다.
이로인해 시스템이 유연하게 변경되고 확장될 수 있습니다.

```java
package strategy;

public class main {
    public static void main(String[] args) {
        Fighter f1 = new F1("F1");
        Fighter f2 = new F2("F2");

        f1.setAttackStrategy(new MissileAttack());
        f2.setAttackStrategy(new GunAttack());

        System.out.println(f1.getName()+" 전투기");
        f1.attack();

        System.out.println(f2.getName()+" 전투기");
        f2.attack();

        f2.setAttackStrategy(new MissileAttack());
        System.out.println(f2.getName()+" 전투기");
        f2.attack();
    }
}

```

![strategy](/images/2021-1-19/main.JPG)


------------------------------------------------

### 끝

    오늘은 전략적 패턴 (strategy pattern)에 대해 알아보았습니다.
    
    감사합니다. 🙏

-------------------------------------------------

Reference

https://victorydntmd.tistory.com/292

https://flowarc.tistory.com/entry/1-Strategy-Pattern