---
layout: post
title: "💻 [Theory] Java의 GC(Garbage Collection)"
category : Theory
tags: [theory, JVM, Java Virtual Machine]
---

# 💻 [Theory] Java의 가비지 컬렉션(Garbage Collection)

    안녕하세요. 👋
    
    오늘은 Java의 GC(Garbage Collection)에 대해 알아보겠습니다.

## 가비지 컬렉션(Garbage Collection)이란?

프로그램을 개발 하다 보면 <b>유효하지 않은 메모리</b>인 가바지(Garbage)가 발생하게 된다.
C언어를 이용하면 free()라는 함수를 통해 직접 메모리를 해제해주어야 한다.
하지만 Java나 Kotlin 을 이용해 개발을 하다 보면 개발자가 메모리를 직접 해제해주는 일이 없다.
그 이유는 <b>JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리</b>해주기 때문이다.
대신 Java에서 명시적으로 불필요한 데이터를 표현하기 위해서 일반적으로 null을 선언해준다.

### Minor GC와 Major GC 
JVM의 Heap영역은 처음 설계될 때 다음의 <b>※2가지를 전제(Weak Generational Hypothesis)로 설계</b>되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성되며, 메모리에 오랫동안 남아있는 경우는 드물다는 것이다. 그렇기 때문에 객체의 생존 기간에 따라 물리적인
Heap 영역을 나누게 되었는데, 이에 따라 Young, Old 총 2가지 영역으로 설계되었다. (<b>초기에는 Perm 영역이 존재하였지만 Java8부터 제거되었다.</b>)


<b>gc 영역 및 흐름</b>

![gc-flow](/images/2021-6-14/gc-flow.JPG)

#### Young 영역(Young Generation)
- 새롭게 생성된 객체가 할당(Allocation)되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션(Garbage Collection)을 Minor GC라고 부른다.

#### Old 영역(Old Generation)
- Young영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- 복사되는 과정에서 대부분 Young 영역보다 크게 할당되며, 크기가 큰 만큼 가비지는 적게 발생한다.
- Old 영역에 대한 가비지 컬렉션(Garbage Collection)을 Major GC 또는 Full GC라고 부른다.


예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 것이다. 이러한 경우를 대비하여
Old 영역에는 512 bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.

![card-table](/images/2021-6-14/card-table.png)

카드 테이블에는 <b>Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때 마다 그에 대한 정보가 표시</b>된다.
카드 테이블이 도입된 이유는 간단한다. Young 영역에서 가비지 컬렉션(Minor GC)가 실행될 때 모든 Old 영역에 존재하는
객체를 검사하여 참조되지 않는 Young 영역의 객체를 식별하는 것이 비효율적이기 때문이다.
그렇기 때문에 Young 영역에서 가비지 컬렉션이 진행될 때 <b>카드 테이블만 조회하여 GC의 대상인지 식별</b>할 수 있도록 하고 있다.

## 가비지 컬렉션(Garbage Collection)의 동작 방식

Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에, 세부적인 동작 방식은 다르다. 하지만
기본적으로 가비지 컬렉션이 실행된다고 하면 다음의 <b>2가지 공통적인 단계</b>를 따르게 된다.

- Stop The World
- Mark and Sweep

>추가로 'Compact' 라는 'Sweep' 후 분산되어있는 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나누는 과정이 있다.
>그리고 Compact 과정은 기본적으로 'Stop The World'를 유발한다.
>메모리 단편화를 방지한다.

#### 1. Stop The World

Stop The World는 가비지 컬렉션을 실행하기 위해 <b>JVM이 애플리케이션의 실행을 멈추는 작업</b>이다.
GC가 실행될 때는 <b>GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단</b>되고,
GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에,
<b>GC의 성능 개선을 위해 튜닝</b>을 한다고 하면 보통 <b>stop-the-world 의 시간을 줄이는 작업</b>을 하는 것이다.
또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.

#### 2. Mark and Sweep

Mark: 사용되는 메모리와 사용되지 않는 메모리를 마킹(식별)하는 작업
Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이
어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 <b>사용되고 있는 메모리를 식별</b>하는데, 이러한 과정을 Mark라고 한다.
이후에 <b>Mark가 되지 않은 객체들을 메모리에서 제거</b>하는데, 이러한 과정을 Sweep라고 한다.

> 여기서 참조(사용)되고 있는 것을 Reachable 참조(사용)되지 않는 것을 UnReachable 라고 한다.

---

### Minor GC의 동작 방식
Minor GC를 정확히 이해하기 위해서는 Young 영역의 구조에 대해 이해를 해야 한다. Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역, 총 3가지로 나뉘어진다.

- Eden 영역: 새로 생성된 객체가 할당(Allocation)되는 영역
- Survivor 영역: 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역

객체가 새롭게 생성되면 Young 영역 중에서도 Eden 영역에 할당(Allocation)이 된다. 그리고 <b>Eden 영역이 꽉 차면 Minor GC가 발생</b>하게 되는데,
사용되지 않는 메모리는 해제되고 Eden 영역에 존재하는 객체는 (사용중인) Survivor 영역으로 옮겨지게 된다.
<b>Survivor 영역은 총 2개이지만 반드시 1개의 영역에만 데이터가 존재</b>해야 하는데,Young 영역의 동작 순서를 자세히 살펴보도록 하자.
 
1.　새로 생성된 객체가 Eden 영역에 할당된다.

2.　객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행된다.

- 2-1. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
- 2-2. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
    
3.　1~2번의 과정이 반복되다가 Survivor 영역이 가득 차게 되면 Survivor 영역의 살아남은 객체를 다른 Survivor 영역으로 이동시킨다.(1개의 Survivor 영역은 반드시 빈 상태가 된다.)

4.　이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동(Promotion)된다.
 


객체의 생존 횟수를 카운트하기 위해 Minor GC에서 객체가 살아남은 횟수를 의미하는 age를 Object Header에 기록한다. 그리고 Minor GC 때 Object Header에 기록된 age를 보고 Promotion 여부를 결정한다.
또한 Survivor 영역 중 1개는 반드시 사용이 되어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아님을 파악할 수 있다.

이러한 진행 과정을 그림으로 살펴보면 다음과 같다.

![minor-gc](/images/2021-6-14/minor-gc.png)


HotSpot JVM에서는 Eden 영역에 객체를 빠르게 할당(Allocation)하기 위해 bump the pointer와 TLABs(Thread-Local Allocation Buffers)라는 기술을 사용하고 있다.
bump the pointer 란 <b>Eden 영역에 마지막으로 할당된 객체의 주소를 캐싱해두는 것</b>이다. bump the pointer를 통해 새로운 객체를 위해 유효한 메모리를 탐색할
필요 없이 마지막 주소의 다음 주소를 사용하게 함으로써 속도를 높이고 있다. 이를 통해 새로운 객체를 할당할 때 객체의 크기가 Eden 영역에 적합한지만 판별하면 되므로
빠르게 메모리 할당을 할 수 있다.

싱글 쓰레드 환경이라면 문제가 없겠지만 멀티쓰레드 환경이라면 객체를 Eden 영역에 할당할 때 락(Lock)을 걸어 동기화를 해주어야 한다.
멀티 쓰레드 환경에서의 성능 문제를 해결하기 위해 HotSpot JVM은 추가로 <b>TLABs(Thread-Local Allocation Buffers)라는 기술을 도입</b>하게 되었다.

TLABs(Thread-Local Allocation Buffers)란
<b>각각의 쓰레드마다 Eden 영역에 객체를 할당하기 위한 주소를 부여함으로써 동기화 작업 없이 빠르게 메모리를 할당하도록 하는 기술</b>이다.
각각의 쓰레드는 자신이 갖는 주소에만 객체를 할당함으로써 동기화 없이 bump the poitner를 통해 빠르게 객체를 할당하도록 하고 있다.
 
### Major GC의 동작 방식

- Young 영역에서 오래 살아남은 객체는 Old 영역으로 Promotion 됨을 확인할 수 있었다.
- <b>Major GC는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생</b>하게 된다.
- Young 영역은 일반적으로 Old 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다.
- 그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다.
- 하지만 Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수도 있다.
- 그렇기 때문에 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다.

## 가비지 컬렉션 비교 요약

![gc](/images/2021-6-14/gc.png)

## 가비지 컬렉션 알고리즘의 종류

기본적인 가비지 콜렉션 과정에 대해 알아보았다. 한편 가비지 콜렉터는 다음과 같은 5개의 종류가 존재하고 상황에 따라 필요한 가비지 콜렉터를 선택해서 사용할 수 있다.

- Serial GC
- Palleral GC
- Palleral Old GC
- CMS GC (Concurrent Mark Sweep)
- G1 GC (Garbage First)

### 끝

    Java의 가비지 컬렉션(Garbage Collection)에 대해 알아보았습니다.

-------------------------------------------------

Reference

<https://d2.naver.com/helloworld/1329>

<https://mangkyu.tistory.com/118?category=872426>
