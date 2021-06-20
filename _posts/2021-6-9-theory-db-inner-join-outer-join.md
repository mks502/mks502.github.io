---
layout: post
title: "💻 [Theory] 데이터베이스 내부 조인과 외부 조인"
category : Theory
tags: [theory, database, db, join]
---

# 💻 [Theory] 데이터베이스 조인(join)

    이번에는 데이터베이스 내부 조인과 외부 조인에 대해 알아보겠습니다.

## 데이터베이스 조인(join)이란 무엇인가?
- join(조인) 또는 결합 구문은 한 데이터베이스 내의 여러 테이블의 레코드를 조합하여 하나의 열로 표현한 것이다. 따라서 조인은 테이블로서 저장되거나, 그 자체로 이용할 수 있는 결과 셋을 만들어 낸다.
- JOIN은 여러개의 테이블에서 각각의 공통값을 이용함으로써 필드를 조합하는 수단이 된다.

## 조인(join)의 종류

![sql-joins](/images/2021-6-9/sql-joins.JPG)

>>ANSI 표준 SQL 은 네 가지 유형의 JOIN을 규정한다.

- INNER JOIN:   내부조인 -> 교집합
- OUTER JOIN:   외부조인 -> 합집합
    - LEFT OUTER JOIN
    - RIGHT OUTER JOIN

각 조인들을 예시와 함께 알아보겠다.

직접 JOIN을 사용하며 알아보기 위해

아래와 같은 테이블 두개를 생성하였다.

![example-tables](/images/2021-6-9/example-tables.JPG)

이제 직접 각 조인들을 실행해보자.

## 1.INNER JOIN
- 교집합에 해당되며 조건에 맞는 공통적인 부분만을 SELECT 한다.

![example-inner-join](/images/2021-6-9/example-inner-join.JPG)

![inner-join](/images/2021-6-9/inner-join.JPG)

```sql
SELECT A.ID, A.KOREAN_NAME, B.ENGLISH_NAME
FROM A INNER JOIN B
ON A.ID = B.ID;
```

## 2.OUTER JOIN
- OUTER JOIN은 조인하는 여러테이블에서 한 쪽에는 데이터가 있고, 한 쪽에는 데이터가 없는 경우, 데이터가 있는 쪽 테이블의 내용을
모두 출력하는 것이다.
즉, <b>조건에 맞지 않아도 해당하는 행을 출력하고 싶을 때</b> 사용할 수 있다.

OUTER JOIN은
- 모든 집합을 출력하는 FULL OUTER JOIN
- 왼쪽 집합에 속하는 것들을 모두 출력하는 FULL LEFT OUTER JOIN
- 오른쪽 집합에 속하는 것들을 모두 출력하는 FULL RIGHT OUTER JOIN 등이 있다.

그 밖에도 LEFT OUTER JOIN 을 통해 차집합 (A-B) 집합을 구할 수 있고
반대로 RIGHT OUTER JOIN을 통해 차집합 (B-A) 집합도 구할 수 있다.

#### 2-1 LEFT OUTER JOIN
- 조인문의 <b>왼쪽에 있는 테이블의 모든 결과를 가져 온 후</b> 오른쪽 테이블의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL로 표시한다.

![example-left-join](/images/2021-6-9/example-left-join.JPG)

![left-join](/images/2021-6-9/left-join.JPG)

```sql
SELECT A.ID, A.KOREAN_NAME, B.ENGLISH_NAME
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID;
```

#### 2-2 RIGHT OUTER JOIN
- 조인문의 <b>오른쪽에 있는 테이블의 모든 결과를 가져온 후</b> 왼쪽의 테이블의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL을 표시한다.

![example-right-join](/images/2021-6-9/example-right-join.JPG)

![right-join](/images/2021-6-9/right-join.JPG)

```sql
SELECT B.ID, A.KOREAN_NAME, B.ENGLISH_NAME
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID;
```

#### 2-3 FULL OUTER JOIN
- LEFT OUTER JOIN 과 RIGHT OUTER JOIN을 합친 것으로, 양쪽 모두 조건이 일치하지 않는 것까지 모두 결합해 출력한다.

![example-right-join](/images/2021-6-9/example-full-join.JPG)

```sql
SELECT B.ID, A.KOREAN_NAME, B.ENGLISH_NAME
FROM A FULL OUTER JOIN B
ON A.ID = B.ID;
```

>※ MySQL 에서는 FULL OUTER JOIN 이 없으므로,
>LEFT OUTER JOIN 과 RIGHT OUTER JOIN 을 UNION 하는 식으로 하여 FULL OUTER JOIN 을 만들어 준다.

### 끝

    데이터베이스 내부 조인과 외부 조인에 대해 알아보았습니다.
    감사합니다. 🙏
    
-------------------------------------------------

Reference

<https://ko.wikipedia.org/wiki/Join_(SQL)>

<https://www.w3schools.com/sql/sql_join.asp>

<https://rh-cp.tistory.com/44>