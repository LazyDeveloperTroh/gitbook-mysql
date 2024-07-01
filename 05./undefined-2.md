# 인덱스와 잠금

InnoDB 에서 인덱스와 잠금은 중요한 연관 관계가 있다. 아래 employees 테이블에는 first\_name 칼럼만 멤버로 담긴 인덱스만 존재한다. 이 때 employees 테이블을   first\_name  = 'Georgi'  조회한다면 253 건의 데이터가 검색되고 first\_name='Georgi' and last\_name='Klassen' 조건으로 조회한다면 1건의 데이터가 검색된다고 해보자.

```sql
// result = 253
select count(*) from employees where first_name='Georgi';

// result = 1
select count(*) from employees where first_name='Georgi' and last_name='Klassen';

update employees set hire_date=now() where first_name='Georgi' and last_name'Klassen';
```



그럼 위의 업데이트 문을 실행하면 몇 개의 레코드가 잠기게 될까? <mark style="background-color:green;">**InnoDB 의 잠금은 레코드를 잠그는 것이 아니라 인덱스를 잠그는 방식으로 처리된다**</mark>. 위의  경우 인덱스는 first\_name 컬럼에만 생성되어 있기 때문에 first\_name='Georgi' and last\_name='Klassen' 조건의 <mark style="background-color:green;">**1 건을 변경하기 위해 253 건의 인덱스를 조회하며 레코드를 잠그게 되는 것이다. 만약 인덱스가 없는 테이블이였다면 전체 레코드에 대해 잠금이 발생했을 것이다.**</mark>
