# Table of contents

## 04. 아키텍처

* [엔진](README.md)
  * [InnoDB 스토리지 엔진](04./engine/innodb.md)
  * [InnoDB 스토리지 엔진 - 버퍼풀](04./engine/innodb-1.md)
* [스레딩 구조](04./undefined.md)
* [메모리](04./undefined-1.md)
* [쿼리 실행 구조](04./undefined-2.md)

## 05. 트랜잭션과 잠금

* [트랜잭션](<README (1).md>)
* [잠금](05./undefined-1.md)
* [인덱스와 잠금](05./undefined-2.md)

## 인덱스 <a href="#index" id="index"></a>

* [읽기방식](index/disk.md)
* [인덱스](index/b-tree/README.md)
  * [B-Tree 인덱스](index/b-tree/b-tree-balanced-tree.md)
  * [인덱스 사용에 영향을 미치는 요소](index/b-tree/undefined.md)
  * [B-Tree 인덱스를 통한 데이터 읽기](index/b-tree/b-tree-1.md)
  * [인덱스의 정렬 및 스캔 방향](index/b-tree/undefined-1.md)
  * [인덱스의 가용성과 효율성](index/b-tree/undefined-2.md)
  * [클러스터링 인덱스](index/b-tree/undefined-3.md)

## 옵티마이저 & 힌트

* [옵티마이저 & 힌트](and/and.md)
* [데이터 처리 방법](and/undefined/README.md)
  * [풀 테이블 스캔과 풀 인덱스 스캔](and/undefined/undefined.md)
  * [ORDER BY](and/undefined/order-by.md)
