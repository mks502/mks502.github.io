---
layout: post
title: 📖 [Theory] 💻 Transaction isolation level 트랜잭션 격리 수준
category : Theory
tags: [theory, transaction, db]
---

# 💻 Transaction isolation level

    안녕하세요. 👋
    
    오늘은 트랜잭션 격리수준 ( Transaction isolation level ) 에 대해
    
    알아보려고 합니다.

## What is Transaction isolation level? (트랜잭션 격리 수준이란?)

    트랜잭션 격리 수준이란 여러 트랜잭션이 동시에 수행이 될 때 

    특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회한 데이터를 

    볼 수 있도록 허용 여부를 결정하는 것입니다.

## 트랜잭션 격리 수준의 필요성?

-   트랜잭션은 ACID 특성을 가짐
-   트랜잭션이 원자적이면서도 독립적인 수행을 해야함
-   트랜잭션이 수행되는 동안 다른 트랜잭션이 관여하지 못하게 막는 Locking 이 등장함
-   무조건적인 Locking은 동시에 많은 트랜잭션을 처리할 때 DB 성능이 떨어짐
-   효울적인 Locking 방법이 필요함

## 트랜잭션 격리 수준의 종류

### level 0 - READ UNCOMMITTED
![UNCOMMITTED](/images/2020-5-11/UNCOMMITTED.png)

    트랜잭션 A가 특정 컬럼 데이터를 변경하고 있는 중에 (COMMIT은 아직 되지 않음)
    트랜잭션 B가 read하면 트랜잭션 A가 변경하기 전 데이터를 읽어온다.
    만약 트랜잭션 A가 데이터 변경 후 커밋하게 되면 트랜잭션 B는 변경된 데이터를 읽어온다.

#### 문제점
- dirty read 발생
    
    트랜잭션이 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있게 되는 현상
    변경되고 있는 데이터를 읽었는데 트랜잭션에 문제가 생겨 ROLLBACK 이 된다면
    변경 전 데이터로 돌아가게 됨.
    
    이때 변경되고 있던 데이터를 읽은 것과 ROLLBACK으로 인해 변경 전 데이터로 돌아간 것이 불일치함. 

------------------------------------------------


### level 1 - READ COMMITTED
![COMMITTED](/images/2020-5-11/COMMITTED.png)

    트랜잭션 A가 특정 컬럼 데이터를 변경하고 있는 중에(COMMIT 전)
    트랜잭션 B가 read하면 트랜잭션 A가 변경하기 전 데이터(이전 COMMIT)를 읽어온다.
    만약 트랜잭션 A가 데이터 변경 후 커밋하게 되면 트랜잭션 B는 변경된 데이터를 읽어온다.

#### 문제점
- Non-repeatable Read 발생

    트랜잭션 시작 후, 반복적인 조회작업에서 다른 트랜잭션에서 데이터가 변경되는 경우, 조회 시 데이터가 일치하지 않는 문제가 발생하는 현상을 말함.

    똑같은 데이터를 다시 조회했을때 일치하지가 않을수 있음.


------------------------------------------------

### level 2 - REPEATABLE READ
![REPEATABLE READ](/images/2020-5-11/REPEATABLE.png)

    트랜잭션 내에서 다른 트랜잭션이 데이터를 변경 및 커밋했더라도,
    트랜잭션 시작 시점의 데이터를 보여주는 트랜젝션 레벨입니다.

#### 문제점
- Phantom Read 발생

    Unrepeatable Read 에서는 데이터의 변경, 삭제, 추가 작업이 가능.
    반면 Phantom Read 는 데이터 추가만 가능하다.

    추가가 가능하기 때문에, 트랜잭션 시작 후, 반복적인 조회작업에서 다른 트랜잭션에서 데이터가 추가되는 경우, 조회 시 데이터가 일치하지 않는 문제가 발생하는 한 현상을 말함.
    
    한 트랜잭션 내에서 같은 쿼리를 두 번 수행했는데, 첫 번째 쿼리에서 없던 유령(Phantom) 레코드가 두 번째 쿼리에서 나타나는 현상을 말함.

------------------------------------------------

### level 3 - SELIALIZABLE

    가장 높은 isolation level 이다.
    트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리므로
    다른 사용자는 해당 데이터에 변경, 삭제, 추가 작업 모두 하지 못함.
    완벽한 일관성 제공!

#### 문제점
- Lock이 점유하고 있어서 DB의 성능이 저하된다.



------------------------------------------------

### 끝

![table](/images/2020-5-11/table.jpg)


    트랜잭션의 격리 수준과
    해당 격리 수준마다의
    문제점을 알아보았습니다.
    
    감사합니다. 🙏

-------------------------------------------------

Reference

https://lng1982.tistory.com/287

http://www.dbguide.net/db.db?cmd=view&boardUid=148216&boardConfigUid=9&boardIdx=138&boardStep=1
