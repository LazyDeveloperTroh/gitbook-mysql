---
description: 논리적인 작업 단위를 모두 완벽하게 처리하거나 처리하지 못할 경우에는 모두 원 상태로 복구하여 데이터의 정합성을 보장하는 것
---

# 트랜잭션

### 트랜잭션 사용 시 주의사항

#### 1. 트랜잭션은 최소한의 코드에만 적용시키는 것이 좋다.

* why? 데이터베이스 커넥션은 개수가 제한적이기 때문에 커넥션을 소유하는 시간이 길어질수록 사용 가능한 커넥션 여유 커넥션의 개수가 줄어들기 때문이다.

#### 2. 트랜잭션 내 다른 네트워크 연결이 필요한 작업은 배제하는 것이 좋다.

* why? 트랜잭션을 시작한 후 다른 네트워크 연결을 시도했을 때 네트워크 이슈로 인해 연결이 지연된다면 커넥션을 소유하는 시간이 길어지고 이로 인해 다른 곳에서 커넥션을 사용할 수 없는 문제가 발생할 수 있다.



### 격리수준

<mark style="background-color:green;">**트랜잭션의 격리수준이란 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정**</mark>하는 것이다. "READ UNCOMMITTED", "READ COMMITTED", "REPEATABLE READ", "SERIALIZABLE" 4가지로 나뉘며 아래는 가 수준 별로 발생할 수 있는 문제점이다.

|                   | DIRTY READ | NON-REPEATABLE-READ | PHANTOM-READ |
| ----------------- | ---------- | ------------------- | ------------ |
| READ UNCOMMITETED | O          | O                   | O            |
| READ COMMITTED    | X          | O                   | O            |
| REPEATABLE READ   | X          | X                   | O(InnoDB 제외) |
| SERIALIZABLE      | X          | X                   | X            |



#### READ UNCOMMITTED

<mark style="background-color:green;">**각 트랜잭션에서의 변경 내용이 COMMIT 이나 ROLLBACK 여부에 상관없이 다른 트랜잭션에서 보이는 격리 수준이다.**</mark> 어떤 트랜잭션이 처리한 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있는 문제점(더티 리드)이 발생할 수 있다.



#### READ COMMITED

<mark style="background-color:green;">**어떤 트랜잭션에서 변경한 내용이 커밋되기 전까지는 다른 트랜잭션에서는 변경 내역을 조회할 수 없는 격리 수준이다.**</mark> 한 트랜잭션에서 같은 SELECT 쿼리를 실행했을 때 다른 결과가 나올 수 있는 NON-REPEATABLE READ 문제가 발생할 수 있다.&#x20;



#### REPEATABLE READ

MySQL 의 InnoDB 스토리지 엔진에서 기본으로 사용되는 격리 수준이다. <mark style="background-color:green;">**동일 트랜잭션 내에서는 항상 동일한 결과를 보장하는 격리 수준이다. 이를 위해 InnoDB 스토리지 엔진은 MVCC 를 이용한다**</mark>. 한 트랜잭션에서 조회한 데이터를 다른 트랜잭션에서 변경한다면 트랜잭션 번호가 변경되고 기존 트랜잭션에서 다시 데이터를 조회하면 언두 영역의 데이터를 반환하도록 동작한다.

* READ COMMITED 과  차이점? \
  READ COMMITTED 도MVCC 를 이용하여 COMMIT 이전의 버전을 보여주지만 두 격리 수준의 차이점은 언두 영역에 백업된 레코드의 여러 버전 가운데 몇 번 째 이전 버전까지 찾아야 하는지에 대한 차이가 있다.



#### SERIALIZABLE

가장 엄격한 격리 수준으로 <mark style="background-color:green;">**모든 트랜잭션을 순차적으로 처리한다**</mark>. 때문에 처리 성능이 떨어지는 단점이 있다.
