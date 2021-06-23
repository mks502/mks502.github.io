---
layout: post
title: "💻 [Theory] 데이터베이스의 락(lock)"
category : Theory
tags: [theory, database, db, index]
---

# 💻 [Theory] 데이터베이스의 락(lock)

    이번에는 데이터베이스의 락(lock)에 대해 알아보겠습니다.
    사실 locking 기법은 직접적으로 db에 lock 을 걸지 않고 Version을 확인하여 동시성을 제어하는 방식인
    Optimistic Lock(낙관적 락) 기법도 있습니다. (애플리케이션 단에서의 lock)
    
    하지만 이번 포스트에서 다루는 lokc은 데이터베이스단에서 lock 하는 Pessimistic Lock(비관적 락)을 다루겠습니다.
    
    추후 포스트에서 Optimistic Lock 과 Pessimistic Lock에 대해서 다시 자세히 다뤄보겠습니다.

## 데이터베이스 락(lock)이란 무엇인가?
>Record locking is the technique of preventing simultaneous access to data in a database, to prevent inconsistent results.
>
>레코드 잠금은 일관성없는 결과를 방지하기 위해 데이터베이스의 데이터에 대한 동시 액세스를 방지하는 기술이다.

데이터베이스 락은 같은 자원을 액세스하는 다중 트랜잭션 환경에서 DB의 일관성과 무결성을 유지하기 위해 트랜잭션의 순차적 진행을 보장하는 직렬화 기법이다.

## 데이터베이스 락(lock)의 종류

Lock의 종류는 크게 Shared Lock, Exclusive Lock로 나뉜다.

#### Shared Lock (공유 락)

- Read Lock 이라고도 한다.
- 공유 Lock은 데이터를 읽을 때 사용되어지는 Lock이다.
- 공유 Lock 은 같은 공유 Lock 끼리는 동시에 접근이 가능하다.
- 데이터 일관성과 무결성을 해치지 않기 때문이다.
- 사용자가 데이터를 읽어 갈 뿐, 데이터 변경이 없기 때문에 가능하다.
- 베타 Lock을 걸 수는 없다.


#### Exclusive Lock (베타 락)

- Write Lock이라고도 한다.
- 베타 Lock은 데이터를 변경하고자 할 때 사용된다.
- 트랜잭션이 완료될 때까지 유지된다.
- 베타 Lock은 Lock이 해제될 때까지 다른 트랜잭션(읽기 포함)은 해당 리소스에 접근할 수 없다.
- 베타 Lock가 되어있는 데이터에 대해서 어떠한 Lock도 설정할 수 없다.

#### Lock의 설정 범위(Level)

- <b>※ 행(Row)</b>
    - 행 수준의 Lock은 1개의 행(Row)를 기준으로 Lock 설정을 한다.
    - DML에 대한 Lock으로 <b>가장 일반적으로 사용하는 Lock</b>이다.

- 데이터베이스
    - 데이터베이스 범위의 lock은 전체 데이터베이스를 기준으로 lock 하는 것이다.
    - 즉, 1개의 세션만이 DB의 데이터에 접근이 가능하다.
    - 해당 기능은 일반적으로는 사용하지 않는다.
    - 사용하는 때가 있다면 DB의 소프트웨어 버전을 올린다던지 주요한 DB의 업데이트에 사용한다.
    
- 파일
    - 데이터베이스 파일을 기준으로 lock을 설정한다.
    - 파일 이란 테이블, row 등과 같은 실제 데이터가 쓰여지는 물리적인 저장소이다.
    - 해당 범위의 Lock은 잘 사용되지는 않는다.
    
- 테이블
    - 테이블 수준의 Lock은 테이블을 기준으로 Lock을 설정한다.
    - 이는 테이블의 모든 행을 업데이트 하는 등의 전체 테이블에 영향을 주는 변경을 수행할 때 유용하다.
    - 즉, DDL(create, alter, drop 등) 구문과 함께 사용되며 DDL Lock이라고도 한다.

- 페이지와 블럭
    - 파일의 일부인 페이지와 블록을 기준으로 Lock을 설정한다. 잘 사용되지는 않는다.

- 컬럼
    - 컬럼 기준의 Lock은 컬럼을 기준으로 Lock을 설정할 수 있다.
    - 하지만 이 형식은 Lock 설정 및 해제의 리소스가 많이 들기 때문에 일반적으로 사용되지는 않는다.
    - 지원하는 DBMS도 많지 않다.

##블로킹(Blocking)

- 블로킹은 Lock간(베타 - 베타, 베타 - 공유)의 경합이 발생하여 특정 Transaction이 작업을 진행하지 못하고 멈춰선 상태를 말한다.
- 공유Lock 끼리는 블로킹이 발생하지 않지만 베타Lock은 블로킹을 발생시킨다(접근이 불가하기 때문).
- 블로킹을 해소하기 위해서는 이전의 트랜잭션이 완료(커밋 OR 롤백)되어야 한다.
- 뒤에 들어온 트랜잭션은 이전 트랜잭션이 마무리되어야 이후 진행이 가능하다.
- 이런 경합은 성능에 좋지 않은 영향을 미친다.
- 따라서 경합을 최소화 할 필요가 있다.

![blocking](/images/2021-6-15/blocking.png)

#### Blocking 해결 방안

- 한 트랜잭션의 길이를 너무 길게하는 것은 경합의 확률을 높인다.
- 동일 데이터를 동시에 변경하는 작업을 배제한다. 트랜잭션이 많을 시간에는 대용량 갱신 작업을 자제해야 한다.
- 트랜잭션 격리 수준을 불필요하게 상향 조정하지 않는다.
- 쿼리를 오랜시간 잡아두지 않도록 적절한 튜닝을 진행한다.
- lock timeout을 통해 Lock의 최대 시간을 지정할 수 도 있따.

##교착 상태(Dead Lock)
- 교착상태는 두 트랜잭션이 각각 Lock을 설정하고 다음 서로의 Lock에 접근하여 값을 얻어오려고 할 때 이미 각각의 트랜잭션에 의해 Lock이 설정되어 있기 때문에 양쪽 트랜잭션 모두 영원히 처리가 되지않게 되는 상태이다.

![deadlock](/images/2021-6-15/deadlock.png)

위와 같이 각각의 트랜잭션에 Lock을 걸고 상대방 Lock에 접근하여 반환 받지 못하는 상황에서 Dead Lock이 발생하게 된다.

#### Dead Lock 해결 방안
- Dead Lock 상황이 감지되면 일반적으로 하나를 강제로 중지시켜 한 트랜잭션은 정상적으로 실행되며 중지된 트랜잭션에서 변경한 데이터는 원래 상태로 되돌려 놓습니다.
- Dead Lock 방지를 위해 접근 순서를 동일하게 하는 것이 중요합니다. 접근 순서 규칙을 정해야 합니다.
- SET LOCK_TIMEOUT문을 이용하여 잠금해제 시간을 조절

### 끝

    데이터베이스 락(lock)에 대해 알아보았습니다.
    감사합니다. 🙏
    
-------------------------------------------------

Reference

<http://wiki.gurubee.net/display/STUDY/1.Lock>

<https://sabarada.tistory.com/121>

<https://centbin-dev.tistory.com/40>