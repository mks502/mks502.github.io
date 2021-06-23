---
layout: post
title: "💻 [Theory] MySQL InnoDB와 MyISAM"
category : Theory
tags: [theory, database, mysql, InnoDB,MyISAM]
---

# 💻 [Theory] MySQL InnoDB와 MyISAM

    이번에는 RDB에서 많이 사용되는 MySQL의 가장 대표적인 스토리지 엔진인
    InnoDB와 MyISAM에 대해 알아보겠습니다.
   
## 스토리지 엔진이란?

- 데이터베이스에 대해 데이터를 삽입, 추출, 업데이트 및 삭제(CRUD 참조)하는 데 사용하는 기본 소프트웨어 컴포넌트이다.
- 즉, 스토리지 엔진은 DB에서 데이터를 어떠한 방식으로 저장하고 접근할 것인지에 대한 <b>기능(CRUD)을 제공</b>한다.
- 스토리지 엔진의 특성에 따라 데이터 접근이 얼마나 빠른지, 얼마나 안정적인지, 트랜잭션 등의 기능을 제공하는지 등의 차이점이 발생한다 
- MySQL의 스토리지 엔진으로 가장 많이 사용하는 엔진으로는 <b>InnoDB</b>와 <b>MyISAM</b>이 있다.

## MyISAM

>MyISAM은 MySQL 관계형 데이터베이스 관리시스템 5.5 버전 이전의 기본 스토리지 엔진이다. 이것은 옛 ISAM 코드를 기반으로 했지만,
더 많은 유용한 확장성 가지고 있었다. MyISAM의 가장 부족한 점은 트랜잭션의 지원 부재였다. MySQL 5.5과 이후 판은 참조 무결성 제한과
더 높은 동시성을 보장하기 위해 InnoDB 엔진으로 전환되었다.

- 항상 테이블에 ROW COUNT를 가지고 있기 때문에 SELECT count(*) 명령시 빠르고, <b>SELECT 명령시에도 빠른 속도를 지원</b>
- <b>full text 인덱스를 지원</b>하는데 그렇기 때문에 Read Only기능이 많은 서비스일 수록 MyISAM이 효율적이라 할 수 있다.
    - full text 인덱스 : 검색 엔진과 유사한 방법으로 자연 언어를 이용해 검색할 수 있는 특별한 인덱스로 모든 데이터 문자열의 단어를 저장
- <b>트랜잭션 미제공</b>
- <b>table level locking</b> 
    - SELECT INSERT UPDATE DELETE시 해당 Table 전체에 Locking이 걸린다.

## InnoDB

MySQL 5.5 이후의 기본 엔진으로 트랜잭션 지원, 동시성에 높은 성능, 외래키 지원 등 여러 기능들이 추가 되었다.

- <b>ACID 트랜잭션 지원</b>
- <b>데이터 무결성에 대한 보장</b>
- MyISAM과 비슷하지만 ORACLE처럼 많은 기능을 지원한다.
    - commit, rollback, 장애 복구, row-level locking, 외래키 등
- <b>row level locking 지원</b>
    - 트랜잭션 처리가 필요한 대용량의 데이터에 유리한 점이 있어서, 사용자의 CRUD가 많은 서비스에 유리하다.
- 다수의 사용자 동시접속과 퍼포먼스가 증가하여 대용량 데이터를 처리할 때 최대의 퍼포먼스를 내도록 설계
    - 동시적으로 요청이 많이오는 높은 퍼포먼스가 필요한 대용량 사이트에 적합
- full text 인덱스 지원하지 않는다.

## 정리
- 가장 큰 특징으로 <b>트랜잭션의 유무</b>와 <b>locking level의 차이</b>가 있다.
- InnoDB 엔진은 <b>트랜잭션 처리가 필요</b>하고 <b>대용량의 데이터를 다루는 부분에서 효율적</b>이다.
- MyISAM 엔진은 <b>트랜잭션 처리가 필요 없고</b>, <b>Read only 기능이 많은 서비스일수록 효율적</b>이다.
- 한줄로 정리하면, InnoDB는 데이터의 변화가 많은 서비스에, MyISAM은 SELECT가 많은 서비스에 적합하다 할 수 있다. 
- 용도에 따라 InnoDB나 MyISAM 스토리지 엔진을 결정하는데, InnoDB와 MyISAM 테이블을 같이 사용할 경우, 조인시 주의해야한다

### 끝

     MySQL의 가장 대표적인 스토리지 엔진인
     InnoDB와 MyISAM에 대해 알아보았습니다.
     감사합니다. 🙏
    
-------------------------------------------------

Reference

<https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%97%94%EC%A7%84>

<https://velog.io/@gillog/DBInnoDB-VS-MyISAM>