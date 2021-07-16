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

Table lock은 전체 테이블에 대한 데이터 변경이 있을 경우 사용한다. 테이블을 제어하는 DDL 구문을 사용할 때 Lock이 걸린다고 하여 DDL Lock이라고도 한다.

운영 중인 테이블을 복제\(CREATE SELECT\)하거나 다른 테이블로 옮길 경우\(INSERT SELECT\) **Transaction Isolation Level을 READ COMMITTED 변경하고 작업하기를 권장한다.**

* 그렇지 않으면 관련된 Table은 Exclusive Lock이 걸리고, 관련 Query들이 대기 상태로 빠지면서 시스템 장애가 발생할지도 모르기 때문.

### Row locks

#### 사용방

`FOR UPDATE` 를 붙여주면 Exclusive lock을 건다. 이 때 MySQL은 Row 전체에 lock을 거는 것이 아니라 인덱스에 건다고 하니 참고.

```sql
-- MySQL
SELECT * FROM table_name WHERE id=10 FOR UPDATE;
```

lock 모드를 SHARE MODE로 걸 수도 있다.

```sql
-- MySQL
SELECT * FROM table_name WHERE id=10 LOCK IN SHARE MODE
```

ActiveReocrd에서는 `lock` 구문이나 `with_lock` 구문을 이용하면, 위 `FOR UPDATE` SQL 구문을 생성해준다.

```ruby
# lock
user = User.lock.find_by(id: params[:id])
user.reward = user.reward + params[:reward].to_i
user.save!

# with_lock
user = User.find_by(id: params[:id])
user.with_lock do
  user.reward = user.reward + params[:reward].to_i
  user.save!
end
```

#### 사용 시나리오

특정 user의 reward 컬럼 값을 100만큼 증가시키는 API가 있다고 하자.  그리고 `user.reward`가 원래 0원인 상태에서 200원이 되도록 하기 위해 이 API를 동시에 2차례 호출한다고 생각해보자. 그리고 이 API 호출을 각각 1번 API 호출, 2번 API 호출로 명명해보겠다.

만약 이 API에 lock 처리가 되어있지 않은 상태라면, 이 1번 호출, 2번 호출이 동시에 진행되었을 때 문제가 생길 수 있다.

* 1번 호출과 2번 호출에서 user 값을 read해올 때, 아직 update가 되기 전의 값을 read 해올 수 있기 때문.
* 구체적으로는 `user.reward`를 0원으로 불러오고, 결과적으로 두 요청 모두 `user.reward` 값을 0원에서 100원으로 증가시킨 뒤 최종적으로 100원이 된 상태에서 `user.save` 를 호출하게 되어, 0원에서 200원이 되는 것이 아니라 0원에서 100원이 될 수 있다.

대신 Exclusive lock 처리를 하면, 1번 호출, 2번 호출이 동시에 진행되더라도 문제를 방지할 수 있다.

* 1번 호출과 2번 호출에서 user 값을 read해올 때, 다른 호출에서 lock을 들고 있으면, lock이 풀릴 때까지 기다렸다가 값을 read해오기 때문에, 순차적으로 reward 값을 증가시킬 수 있기 때문이다.

## 참고

* [Scope of locks](https://docs.oracle.com/javadb/10.6.2.1/devguide/rdevconcepts8424.html)
* [MySQL locking - Table-Level Locking, Row-Level Locking, Optimistic Locking](https://offbyone.tistory.com/225)
* [https://www.sqlshack.com/locking-sql-server/](https://www.sqlshack.com/locking-sql-server/)
* [https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/](https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/)
* [https://myinfrabox.tistory.com/75](https://myinfrabox.tistory.com/75)
* [https://stackoverflow.com/questions/24587403/rails-activerecord-how-can-i-lock-a-table-for-reading](https://stackoverflow.com/questions/24587403/rails-activerecord-how-can-i-lock-a-table-for-reading)
* [https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)
* [https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html)
* [http://labs.brandi.co.kr/2019/06/19/hansj.html](http://labs.brandi.co.kr/2019/06/19/hansj.html)
* [https://www.javatpoint.com/mysql-table-locking](https://www.javatpoint.com/mysql-table-locking)
* [https://xpertdeveloper.com/row-locking-with-mysql/](https://xpertdeveloper.com/row-locking-with-mysql/)



