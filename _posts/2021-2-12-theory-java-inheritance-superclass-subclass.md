---
layout: post
title: 📖 [Java] 💻 상속 - 슈퍼(부모)클래스와 서브(자식)클래스
category : Theory
tags: [theory, java, inheritance]
---

# 💻 Java 상속 - 슈퍼(부모) 클래스와 서브(자식) 클래스

    안녕하세요. 👋
    
    오늘은 자바의 상속과 그에 따른
    
    슈퍼(부모) 클래스와 서브(자식) 클래스에 대해
    
    포스팅 해보겠습니다.

## 상속(Inheritance) 이란??

자바의 상속(Inheritance)이란 기존 클래스를 재사용하여 새로운 클래스를 작성하는 것을 말합니다.

쉽게 말하자면 실생활에서 부모에게 자식이 부모의 재산을 상속 받는 것 처럼

서브(자식) 클래스가 슈퍼(부모) 클래스의 자산(부모 클래스의 멤버)을 물려받는 것을 뜻합니다.

상속을 이용하면 공통적인 기능을 다시 작성할 필요 없이 재사용 할 수 있고

코드의 중복을 줄여줍니다. 보다 적은 양의 코드로 새로운 클래스를 쉽게 작성할 수 있습니다.

extends라는 키워드를 사용합니다.
    
    ex)
    class SubClass extends SuperClass 
    
## 슈퍼클래스와 서브클래스

슈퍼(부모) 클래스와 서브(자식) 클래스는 이러한 특징을 가지고 있습니다.

슈퍼(부모) 클래스를 상속받아 만들어진 서브(자식) 클래스는

슈퍼 클래스의 멤버와 메소드를 자기 자신 것처럼 사용할 수 있습니다. (코드 중복 줄여줌)

하지만 슈퍼클래스는 서브클래스의 멤버와 메소드를 사용할 수 없습니다.

또한 슈퍼 클래스는 원래의 가시성을 유지합니다.

슈퍼클래스에서 만약 접근지시자가 public 이었다면 상속 받은 서브클래스에서도 public입니다.

이로인해 접근지시자에 따른 차이가 발생합니다.

    -private 는 오직 해당 클래스에서만 사용이 가능하므로 서브클래스에서 사용이 불가합니다.
    -protected 는 슈퍼클래스와 서브클래스만 사용이 가능하므로 서브클래스에서도 사용이 가능합니다.
    -접근지시자가 없는 경우 패키지(package)멤버로 같은 패키지내에서만 사용이 가능합니다.
    -public 의 경우 모든 클래스에서 사용이 가능합니다.
    
슈퍼클래스의 private 접근지시자의 멤버들은 서브클래스에서 접근/사용이 불가능합니다.
그렇기 때문에 슈퍼클래스의 private 접근지시자의 멤버들에 접근할 수 있는
public 접근지시자의 메소드를 만들어 
이 메소드를 통해 접근할 수 있도록 합니다.
(캡슐화)

## 상속의 예시

```java

public class Extends {

    public static void main(String[] args) {
        RacingCar racingCar = new RacingCar();
        racingCar.setCarNumber("12나 7777");
        racingCar.setMaxSpeed(300);

        System.out.println( "name: " +racingCar.name ); // 슈퍼클래스의 name이 public 접근 지시자이므로 바로 접근 가능

        System.out.println( "carNumber: " +racingCar.getCarNumber() ); // 슈퍼클래스의 carNumber가 private 접근 지사자이므로
                                                        //바로 접근 불가! public 메소드를 통해 접근
        System.out.println( "maxSpeed: " +racingCar.getMaxSpeed() );
    }
}

class Car {
    //private 접근 지시자이므로 상속받은 서브클래스에서 접근 불가
    private String carNumber;
    public String name;

    public Car() {
        this.carNumber = "";
        this.name="붕붕자동차";
        System.out.println("자동차가 생성완료");
    }

    //아래의 getter setter를 public 접근 지시자로 메소드를 만들어
    // private 접근지시자인 carNumber에 대해 서브클래스에서 public 메소드를 통해 접근할 수 있도록 함
    public String getCarNumber() {
        return carNumber;
    }

    public void setCarNumber(String carNumber) {
        this.carNumber = carNumber;
    }
}

class RacingCar extends Car {
    private int maxSpeed;

    public RacingCar() {
        this.maxSpeed = 0;
        System.out.println("경주용차 생성완료");
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }
}


```

### 결과

![result](/images/2021-2-12/result.JPG)


### 끝

    간단하게 Java의 상속과 그에 따른
    슈퍼클래스 서브클래스에 대해 
    알아보았습니다.
    감사합니다. 🙏

-------------------------------------------------

Reference

https://enter.tistory.com/108
