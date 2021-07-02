---
layout: post
title: "💻 [Theory] JPA의 낙관적 잠금(Optimistic Lock), 비관적 잠금(Pessimistic Lock)"
category : Theory
tags: [theory, database, jpa, Optimistic Lock,Pessimistic Lock]
---

# 💻 [Theory] JPA의 낙관적 잠금(Optimistic Lock), 비관적 잠금(Pessimistic Lock)

    애플리케이션을 개발하다보면 여러 동시성 문제들을 만나고 동시성 제어를 도와주는 여러 시스템들(데이터베이스 시스템, JPA 시스템 등) 을 보게됩니다.
    특히나 많은 사용자가 있는 애플리케이션일 수록 동시성 문제가 더 중요하고 이를 제어해주어야 합니다.
    동시성 제어를 도와주는 방법들 중
    이번엔 JPA 시스템에서의 낙관적 잠금(Optimistic Lock), 비관적 잠금(Pessimistic Lock)에 대해 알아보겠습니다.   

## Locking이란 ?
- 애플리케이션을 개발하다보면 여러 동시성 문제들을 만나게 됨.
    - ex) read skew, lost update 등등
- 여러 동시성 문제를 해결하기 위해서 그 중에서도 데이터의 일관성을 지키기 위한 방법 중 하나이다.

## 낙관적 잠금(Optimistic Lock)
- 기본적으로 데이터 갱신시 충돌이 발생하지 않을것이라고 낙관적으로 보는 것.
- 데이터 갱신시 충돌이 발생하지 않을 것으로 예상하기 때문에, 우선적으로 락을 걸지 않는다.
- Version을 사용해 관리한다.
- 어플리케이션 락
- 충돌방지
- 트랜잭션을 커밋하기 전까지는 트랜잭션의 충돌을 알 수 없다.
- 간단히 @Version 어노테이션을 이용해 사용이 가능하다. 단, 다음과 같은 주의사항이 있다.
    - 각 엔티티 클래스에는 하나의 버전 속성 만 있어야한다.
    - 여러 테이블에 매핑 된 엔티티의 경우 기본 테이블에 배치되어야한다다.
    - 버전에 명시할 타입은 int, Integer, long, Long, short, Short, java.sql.Timestamp 중 하나 여야한다.

#### 낙관적 잠금(Optimistic Lock) 처리방법
- 낙관적 잠금은 OptimisticLockException을 발생시킨다.
- Persistence Provider가 Entity에서 낙관적 잠금 충돌을 감지하면 OptimisticLockException을 발생시키게 되고 트랜잭션은 롤백을 처리한다.
- 권장되는 예외처리 방법에서는 Entity를 다시 로드하거나 새로고침하여 업데이트를 재시도하는 방법이다.
- 예외처리시에 발생되는 Exception에서 충돌되는 Entity를 제공해주고 있어 쉽게 처리가 가능하도록 되어있다.

## 비관적 잠금(Pessimistic Lock)
- 기본적으로 데이터 갱신시 충돌이 발생할 것이라고 비관적으로 보고 미리 잠금을 거는 것
- 데이터 갱신시 충돌이 발생하지 않을 것으로 예상하기 때문에, 우선적으로 락을 건다(조회할 때 부터 건다).
- 트랜잭션의 충돌이 발생한다고 가정하고 우선 락을 걸고 보는 방법
- 트랜잭션안에서 서비스로직이 진행되어야함.
- DB 가 제공하는 락 기능을 사용( ex: 공유락- LOCK IN SHARE MODE, 베타락-FOR UPDATE …)
- 무결성의 장점
- 데드락의 위험성
- 데이터를 수정 시 즉시 트랜잭션 충돌을 알 수 있다

## 암시적 잠금과 명시적 잠금

#### 암시적 잠금 (Implicit Lock)
- JPA에서는 @Version이 붙은 필드가 존재하거나 @OptimisticLocking 어노테이션이 설정되어 있을 경우 자동적으로 충돌감지를 위한 잠금이 실행된다.
- 이는 Annotation만 설정해주면 별도의 추가 설정이 없어도 JPA에서 자동적으로 행하여주는 잠금이다.
- 추가로 삭제 쿼리가 발생할시에는 암시적으로 해당 로우에 대한 행 배타잠금(Row Exclusive Lock)을 제공하여준다.

명시적 잠금 (Explicit Lock)

- 프로그램을 통해 의도적으로 잠금을 실행하는 것이 명시적 잠금이다.
- JPA에서 EntityManager를 통하여 엔터티를 조회할 때 LockMode를 직접 지정하거나 select for update 쿼리를 통해서 직접 잠금을 지정할 수 있다.


    Student student = entityManager.find(Student.class, id);
    entityManager.lock(student, LockModeType.OPTIMISTIC);
    
    Student resultStudent = entityManager.find(Student.class, studentId);
    entityManager.lock(resultStudent, LockModeType.PESSIMISTIC_WRITE);

## 정리
- 트랜잭션 충돌이 잘 발생하지 않는 환경에서는 낙관적인 락이 좋고 충돌이 자주 발생하는 환경에서는 비관적인 락이 좋다.
- 낙관적인 락(Optimistic Lock)은 성능에서 이점을 갖는다.
- 비관적인 락(Pessimistic Lock)은 우선적으로 데이터베이스단에서 잠금을 거므로 데이터에 대한 무결성, 정합성에 이점을 갖는다.
- 결국, locking 기법에 정답이 있는 것은 아니므로 각 상황에 맞게 알맞은 사용법이 필요하다.


### 끝

    낙관적 잠금(Optimistic Lock), 비관적 잠금(Pessimistic Lock)에 대해 알아보았습니다.
    감사합니다. 🙏
    
-------------------------------------------------

Reference

<https://velog.io/@lsb156/JPA-Optimistic-Lock-Pessimistic-Lock>

<http://jaynewho.com/post/43>

<https://www.youtube.com/watch?v=w6sFR3ZM64c&t=406s>

