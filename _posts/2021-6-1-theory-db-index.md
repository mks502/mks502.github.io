---
layout: post
title: "💻 [Theory] 데이터베이스 인덱스(index)"
category : Theory
tags: [theory, database, db, index]
---

# 💻 [Theory] 데이터베이스 인덱스(index)

    이번에는 데이터베이스의 인덱스(index)에 대해 알아보겠습니다.

## 데이터베이스 인덱스(index)란 무엇인가?
>인덱스(영어: index)는 데이터베이스 분야에 있어서 테이블에 대한 동작의 속도를 높여주는 자료 구조를 일컫는다. 

인덱스란 추가적인 쓰기 작업과 저장 공간을 활용하여 데이터베이스 테이블의 <b>검색 속도를 향상</b>시키기 위한 자료구조이다.

만약 우리가 책에서 원하는 내용을 찾는다고 하면, 책의 모든 페이지를 찾아 보는것은 오랜 시간이 걸린다.

그렇기 때문에 책의 저자들은 책의 맨 앞 또는 맨 뒤에 색인을 추가하는데, 데이터베이스의 index 는 책의 색인과 같다.

데이터베이스에서도 테이블의 모든 데이터를 검색하면 시간이 오래 걸리기 때문에 데이터와 데이터의 위치를 포함한 자료구조를 생성하여 빠르게 조회할 수 있도록 돕고 있다.

![index](/images/2021-6-1/index.png)

### 인덱스(index)의 필요성?

그럼 인덱스(index)의 필요성은 무엇일까?

인덱스의 의미와 밀접하다. 바로 <b>검색 속도를 향상</b> 시키는 것이 중요하기 때문이다.

왜냐하면 데이터베이스에서 가장 많이 사용되는 연산은 검색 연산이다.

즉, 그만큼 데이터를 조회할 일이 가장 많기 때문에 인덱스를 통해 빠른 조회를 가능하도록 사용하는 것이다.

적은 데이터에서는 상관이 없겠지만 대용량 데이터에서 모든 테이블을 탐색하는 Full Table Scan은 비효율적이므로

등장하게 되었다.

### 인덱스(index)의 관리

DBMS는 index를 항상 최신으로 정렬된 상태로 유지해야 원하는 값을 빠르게 탐색할 수 있다. 그렇기 때문에 인덱스가 적용된 컬럼에 INSERT, UPDATE, DELETE가 수행된다면 각각 다음과 같은 연산을 추가적으로 해주어야 하며 그에 따른 오버헤드가 발생한다.

- INSERT : 새로운 데이터에 대한 인덱스를 추가함
- DELETE : 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업을 진행함
- UPDATE : <b>딱히 UPDATE 개념이 없음</b>!!! 기존의 인덱스를 '사용하지 않음' 처리하고, 갱신된 데이터에 대해 인덱스를 추가함. (DELETE+INSERT)

### 인덱스(index)의 장점과 단점 

장점
- 테이블을 조회하는 속도와 그에 따른 성능을 향상시킬 수 있다.

- 전반적인 시스템의 부하를 줄일 수 있다.

단점
- 인덱스를 관리하기 위해 DB의 약 10%에 해당하는 저장공간이 필요하다.

- 인덱스를 관리하기 위해 추가 작업이 필요하다.

- 인덱스를 잘못 사용할 경우 오히려 성능이 저하되는 역효과가 발생할 수 있다.

- DML에 취약

>만약 CREATE, DELETE, UPDATE가 빈번한 속성에 인덱스를 걸게 되면 인덱스의 크기가 비대해져서 성능이 오히려 저하되는 역효과가
발생할 수 있다. 그러한 이유 중 하나는 DELETE와 UPDATE 연산 때문이다. 앞에서 설명한대로, UPDATE와 DELETE는 기존의 인덱스를
삭제하지 않고 '사용하지 않음' 처리를 해준다고 하였다. 만약 어떤 테이블에 UPDATE와 DELETE가 빈번하게 발생된다면
실제 데이터는 10만건이지만 인덱스는 100만 건이 넘어가게 되어, SQL문 처리 시 비대해진 인덱스에 의해 오히려 성능이 떨어지게 될 것이다. 

### 인덱스(index)를 사용하면 좋은 경우
- 규모가 작지 않은 테이블
- INSERT, UPDATE, DELETE가 자주 발생하지 않는 컬럼
- JOIN이나 WHERE 또는 ORDER BY에 자주 사용되는 컬럼
- 데이터의 중복도가 낮은 컬럼

>인덱스를 사용하는 것 만큼이나 생성된 인덱스를 관리해주는 것도 중요하다. 그렇기 때문에 사용하지 않는 인덱스는 바로 제거를 해주어야 한다. 

## 인덱스(index)의 대표적인 자료구조

인덱스를 구현하기 위해 사용할 수 있는 대표적인 자료구조는 해시 테이블과 B+Tree 가 있다.

### 해시 테이블 (Hash Table)

해시 테이블은 (Key, Value)로 데이터를 저장하는 자료구조 중 하나로 <b>빠른 데이터 검색</b>이 필요할 때 유용하다.
해시 테이블은 해시 함수를 이용해 고유한 index(key) 를 생성하여 그 index에 저장된 값을 꺼내오는 구조이다.
고유한 index(key)를 통해 값을 꺼내오므로 O(1)의 매우 빠른 시간복잡도로 검색이 가능하다.

하지만 DB 인덱스에서 해시 테이블이 사용되는 경우는 제한적인데, 그러한 이유는 해시가 등호(=) 연산에만 특화되었기 때문이다.
해시 함수는 값이 1이라도 달라지면 완전히 다른 해시 값을 생성하는데, 이러한 특성에 의해

<b>부등호 연산(>, <)이 자주 사용되는 데이터베이스 검색을 위해서는 해시 테이블이 적합하지 않다.</b>

즉, 예를 들면 "나는"으로 시작하는 모든 데이터를 검색하기 위한 쿼리문은 인덱스의 혜택을 전혀 받지 못하게 된다.
이러한 이유로 데이터베이스의 인덱스에서는 B+Tree가 일반적으로 사용된다.

### B+Tree

B+Tree는 DB의 인덱스를 위해 자식 노드가 2개 이상인 B-Tree를 개선시킨 자료구조이다. B+Tree는 모든 노드에 데이터(Value)를 저장했던 BTree와 다른 특성을 가지고 있다.

리프노드(데이터노드)만 인덱스와 함께 데이터(Value)를 가지고 있고, 나머지 노드(인덱스노드)들은 데이터를 위한 인덱스(Key)만을 갖는다.
리프노드들은 LinkedList로 연결되어 있다.
데이터 노드 크기는 인덱스 노드의 크기와 같지 않아도 된다.
데이터베이스의 인덱스 컬럼은 부등호를 이용한 순차 검색 연산이 자주 발생될 수 있다. 이러한 이유로 BTree의 리프노드들을 LinkedList로 연결하여 순차검색을 용이하게 하는 등 BTree를 인덱스에 맞게 최적화하였다. (물론 Best Case에 대해 리프노드까지 가지 않아도 탐색할 수 있는 BTree에 비해 무조건 리프노드까지 가야한다는 단점도 있다.)

이러한 이유로 비록 B+Tree는 O(log2n) 의 시간복잡도를 갖지만 해시테이블보다 인덱싱에 더욱 적합한 자료구조가 되었다.

### 끝

    데이터베이스의 인덱스(index)에 대해 알아보았습니다.
    감사합니다. 🙏
    
-------------------------------------------------

Reference

<https://mangkyu.tistory.com/96>