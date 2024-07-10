---
description: >-
  인덱스를 생성할 때 설정한 정렬 규칙에 따라서 인덱스의 키 값은 항상 오름차순이거나 내림차순으로 정렬되어 저장된다. 하지만 어떤 인덱스가
  오름차순으로 생성됐다고 해서 그 인덱스를 오름차순으로만 읽을 수 있다는 뜻은 아니다. 인덱스를 어느 방향으로 읽을지는 쿼리에 따라
  옵티마이저가 실시간으로 만들어내는 실행 계획에 따라 결정된다.
---

# 인덱스의 정렬 및 스캔 방향

### 인덱스의 정렬

MySQL 8.0 버전부터는 다음과 같은 형태의 정렬 순서를 혼합한 인덱스를 생성할 수 있다.&#x20;

```sql
CREATE INDEX ix_teamname_userscore ON employees (team_name ASC, user_score DESC);
```



### 인덱스 스캔 방향

first\_name 칼럼에 대한 인덱스가 포함된 employees 테이블에 대해 다음 쿼리를 실행하는 과정을 한번 살펴보자. MySQL은 이 쿼리를 실행하기 위해 인덱스를 처음부터 오름차순으로 끝까지 읽어 first\_name이 가장 큰 값 하나를 가져오는 것일까?

```sql
SELECT * FROM employees ORDER BY first_name DESC LIMIT 1;
```

\
그렇지 않다. 인덱스는 항상 오름차순으로만 정렬돼있지만 MySQL 옵티마이저는 최대 값부터 내림차순으로 읽으면 더 효율적으로 동작한다는 것을 알고 있다. 때문에 오름차순으로 생성된 인덱스를 정순으로 읽으면 출력되는 결과 레코드는 자동으로 오름차순으로 정렬된 결과가 되고, 역순으로 읽으면 그 결과는 내림차순으로 정렬된 상태가 되는 것이다. \
\
쿼리의 ORDER BY 처리나 MIN() 또는 MAX() 함수 등의 최적화가 필요한 경우에도 MySQL 옵티마이저는 인덱스의 읽기 방향을 전환해서 사용하도록 실행 계획을 만들어 낸다.

```sql
// 아래 좌측 이미지
SELECT * FROM employees WHERE first_name >= 'Anneke' ORDER BY first_name ASC LIMIT 4;

// 아래 우측 이미지
SELECT * FROM employees ORDER BY first_name DESC LIMIT 5;
```

<figure><img src="../../.gitbook/assets/mysql-scandirect.drawio (1).png" alt=""><figcaption></figcaption></figure>



