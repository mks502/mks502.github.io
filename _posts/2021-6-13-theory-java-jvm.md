---
layout: post
title: "💻 [Theory] JVM(Java Virtual Machine)의 구조"
category : Theory
tags: [theory, JVM, Java Virtual Machine]
---

# 💻 [Theory] JVM(Java Virtual Machine)의 구조

    안녕하세요. 👋
    
    오늘은 JVM(Java Virtual Machine)의 구조에 대해 알아보겠습니다.

## JVM(Java Virtual Machine) 이란?
JVM 이란 자바 가상 머신으로 <b>Java 바이트 코드를 실행할 수 있는 주체</b>이다.

특히나 CPU나 운영체제(플랫폼)의 종류와 무관하게 실행이 가능하다. -> 자바를 사용하는 이유이자 장점에 해당한다.

즉, 운영체제 위에서 동작하는 프로세스로 <b>자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할</b>을 한다.

JVM의 구성을 살펴보면 크게 4가지(Class Loader, Execution Engine, Garbage Collector, Runtime Data Area)로 나뉜다.

![jvm-structure](/images/2021-6-13/jvm-structure.png)

1. Class Loader (클래스 로더)

자바에서 소스를 작성하면 Person.java 처럼 .java 파일이 생성된다.

.java 소스를 자바컴파일러가 컴파일하면 Person.class 같은 .class파일(바이트코드)이 생성된다.

이렇게 생성된 <b>클래스 파일들을 분석한 뒤에</b> JVM이 운영체제로부터 할당받은 메모리영역인

<b>Runtime Data Area로 적재하는 역할(런타임 시 동적 로딩)</b>을 Class Loader가 한다.

>※자바 애플리케이션이 <b>실행중</b>일 때 이런 작업이 수행된다.


2. Execution Engine (실행 엔진)

Class Loader에 의해 <b>메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할</b>을 한다.

명령어를 하나 하나 실행하는 인터프리터(Interpreter)방식이 있고 JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.

JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.

- 인터프리터 방식 : 명령어를 하나씩 수행(기본적인 방식. 전체 수행은 느리나 명령어 하나씩의 동작은 빠름)
- JIT(Just In Time compiler)방식 : 전체 바이트 코드를 네이티브 코드로 변환하고 그 이후에는 인터프리팅하지 않고 네이티브 코드 상태로 실행(전체적인 동작은 빠르나 컴파일하는데 상당 시간 소요)

> 최초의 가상 머신은 인터프리터 방식만 써서 실행 속도가 느렸지만, JIT 컴파일 방식을 추가하여 이를 보완하고자 했다.
>그런데 JIT 컴파일은 바이트코드를 기계어로 바꾸기 때문에 실행 속도가 빠르지만 변환하는 데 비용이 발생하게 된다.
>그래서 <b>인터프리터 방식을 사용하다가 일정한 기준이 넘어가면 JIT 컴파일 방식</b>으로 실행한다.
  


3. Garbage Collector (가비지 컬렉터)

Garbage Collector(GC)는 Heap 메모리 영역에 생성(적재)된 객체들 중에 <b>참조되지 않는 객체들을 탐색 후 제거하는 역할</b>을 한다.

GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다. (참조가 없어지자마자 해제되는 것을 보장하지 않음)

또 다른 특징은 GC가 수행되는 동안 GC를 수행하는 쓰레드가 아닌 다른 모든 쓰레드가 일시정지된다.

특히 Full GC가 일어나서 수 초간 모든 쓰레드가 정지한다면 장애로 이어지는 치명적인 문제가 생길 수 있는 것이다.

> 내용이 많고 중요한 부분이므로 다음 포스트에서 더 자세히 다뤄보자

4. Runtime Data Area (런타임 데이터 영역)

JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.

이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.

---
##자바 런타임 메모리(Runtime Data area) 구조

![runtime-data-area-structure](/images/2021-6-13/runtime-data-area-structure.png)

#### 1.Method area (메소드 영역)

클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보,

Type정보(Interface인지 class인지), Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨), static 변수, final class 변수등이 생성되는 영역이다.

    클래스에 대한 정보와 함께 클래스 변수(static variable)가 저장되는 영역.
    JVM은 자바 프로그램에서 특정 클래스가 사용되면 해당 클래스의 클래스 파일(*.class)을 읽어들여, 클래스에 대한 정보를 메소드 영역에 저장한다.
    Class area (클래스 영역)이라고도 한다.

#### 2.Heap area (힙 영역)

런타임 시 결정되는 참조형 데이터타입이 저장되는 공간,

new 키워드로 생성된 객체와 배열이 생성되는 영역이다.

메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역이다.

#### 3.Stack area (스택 영역)

컴파일 시 결정되는 기본형 데이터타입이 저장되는 공간,

지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값등이 생성되는 영역이다.

int a = 10; 이라는 소스를 작성했다면 정수값이 할당될 수 있는 메모리공간을 a라고 잡아두고 그 메모리 영역에 값이 10이 들어간다. 즉, 스택에 메모리에 이름이 a라고 붙여주고 값이 10인 메모리 공간을 만든다.

클래스 Person p = new Person(); 이라는 소스를 작성했다면 Person p는 스택 영역에 생성되고 new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.

그리고 스택영역에 생성된 p의 값으로 힙 영역의 주소값을 가지고 있다. 즉, 스택 영역에 생성된 p가 힙 영역에 생성된 객체를 가리키고(참조하고) 있는 것이다.

메소드를 호출할 때마다 개별적으로 스택이 생성된다.

#### 4.PC Register (PC 레지스터)

Thread(쓰레드)가 생성될 때마다 생성되는 영역으로 Program Counter 즉, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. (*CPU의 레지스터와 다름)

이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.

#### 5.Native method stack

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다.

보통 C/C++등의 코드를 수행하기 위한 스택이다.

Java Native Interface를 통해 바이트코드로 변환됨


쓰레드가 생성되었을 때 기준으로

1,2번인 메소드 영역과 힙 영역을 모든 쓰레드가 공유하고,

3,4,5번인 스택 영역과 PC 레지스터, Native method stack은 각각의 쓰레드마다 생성되고 공유되지 않는다.

### 끝

    자바의 핵심 JVM의 구조에 대해 알아보았습니다.

-------------------------------------------------

Reference

<https://jeong-pro.tistory.com/148>

<https://gbsb.tistory.com/2>
