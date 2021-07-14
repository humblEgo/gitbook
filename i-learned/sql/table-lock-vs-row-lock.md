---
description: '실무에서 실수하지 않는 것이 목표이며, 시나리오 중심으로 개념을 공고히 합니다.'
---

# Table lock vs Row lock

## 개념 정리

구문에 따라서 lock되는 데이터의 범위 & 양이 달라질 수 있다.

### Table locks

* 테이블 전체를 잠근다.
* 데드락이 발생하지 않는다.
* **Row-level locking을 하더라도** **조건 필드에 인덱스가 없으면 전체 테이블에 lock이 걸린다.**
  * * MySQL이 조회된 행을 기억하는 방법이 인덱스 범위를 기억하는 것이기 때문.
* 많은 수의 Row-level locking을 시도할 때 Table locking이 더 효율적이라면, 전체 테이블에 lock이 걸릴 수도 있다. 이는 `Lock Escalation` 이라고 한다.

### Single-row locks

* 트랜잭션 격리 레벨에 따라 다르게 적용된다.
* 데드락이 발생하지 않도록 주의해야한다. 특히 innoDB나 BDB 타입의 테이블들은 deadlock-free하지 않으므로 주의할 것.

## 사용

### Table locks

#### 사용 방법

READ lock인지 WRITE lock인지 설정할 수 있다.

```sql
-- MySQL
LOCK TABLES table_name [READ | WRITE];
```

Rails에서는 Table lock을 공식적으로 지원하지는 않는다. 그래서 아래처럼 직접 SQL문을 넣어줘야한다.

```ruby
ActiveRecord::Base.transaction do
  ActiveRecord::Base.connection.execute('LOCK TABLES table_name READ')
  ...
end
```

#### 사용 시나리오

1. Table Lock된 상태에서 ALTER TABLE을 사용하면 잠금이 해제될 수 있다.
2. 






## 참고

* [Scope of locks](https://docs.oracle.com/javadb/10.6.2.1/devguide/rdevconcepts8424.html)
* [MySQL locking - Table-Level Locking, Row-Level Locking, Optimistic Locking](https://offbyone.tistory.com/225)
* [https://www.sqlshack.com/locking-sql-server/](https://www.sqlshack.com/locking-sql-server/)
* [https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/](https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/)
* [https://myinfrabox.tistory.com/75](https://myinfrabox.tistory.com/75)
* [https://stackoverflow.com/questions/24587403/rails-activerecord-how-can-i-lock-a-table-for-reading](https://stackoverflow.com/questions/24587403/rails-activerecord-how-can-i-lock-a-table-for-reading)
* [https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)



