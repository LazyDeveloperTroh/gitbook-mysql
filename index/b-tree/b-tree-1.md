---
description: MySQL 이 어떻게 인덱스를 이용해서 실제 레코드를 읽어 내는 방법
---

# B-Tree 인덱스를 통한 데이터 읽기

### 인덱스 레인지 스캔

인덱스 레인지 스캔은 인덱스의 접근 방법 가운데 가장 대표적인 방법으로 검색해야 할 인덱스의 범위가 결정됐을 때 사용하는 방식이다.&#x20;

```sql
SELECT * FROM employees WHERE first_name BETWEEN 'Ebbe' AND 'Gad';
```

<div align="left">

<figure><img src="../../.gitbook/assets/mysql-rangescan.drawio.png" alt=""><figcaption></figcaption></figure>

</div>

<mark style="background-color:blue;">인덱스 레인지 스캔은 B-Tree 인덱스에서 루트와 브랜치 노드를 이용해 스캔 시작 위치를 검색하고, 그 지점부터 필요한 방향으로 인덱스를 읽어 나가는 방식으로 동작한다. 중요한 것은 해당 인덱스를 구성하는 칼럼의 정순 또는 역순으로 정렬된 상태로 레코드를 가져온다.</mark> (이미 정렬된 상태이기 때문에)\
\
또한 아래와 같이 <mark style="background-color:blue;">리프 노드에서 검색 조건에 일치하는 건들은 데이터 파일에서 레코드를 읽어오는 과정이 필요하다. 이 때 레코드 1건 단위로 랜덤 디스크 I/O 가 발생하기 때문에 인덱스를 통해 데이터를 읽는 것이 높은 비용이 든다</mark>는 것이다.

<figure><img src="../../.gitbook/assets/mysql-rangescan2.drawio.png" alt=""><figcaption></figcaption></figure>

#### 인덱스 레인지 스캔의 동작 단계

1. 인덱스에서 조건을 만족하는 값이 저장된 위치를 찾는다 (인덱스 탐색)
2. 1번에서 탐색된 위치부터 필요한 만큼 인덱스를 차례대로 쭉 읽는다. (인덱스 스캔)
3. 2번에서 읽어들인 인덱스 키와 레코드 주소를 이용해 레코드가 저장된 페이지를 가져오고, 최종 레코드를 읽는다.

#### 커버링 인덱스

쿼리가 필요로 하는 데이터에 따라 데이터 파일을 탐색하는 과정이 필요하지 않을 수 있는데 이를 커버링 인덱스라고 한다.&#x20;
