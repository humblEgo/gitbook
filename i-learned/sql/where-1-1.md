# WHERE 1=1 구문에 대하여

## 상황

팀원이 자연스럽게 `WHERE 1=1` 구문을 써서 쿼리를 날리는 것을 보았다. 아래는 예시 쿼리문.

```text
select * from ads
where 1=1
and example_id in (42)
and status = 0;
```

### 쓰는 이유

1. 쿼리 디버깅 시, 주석처리가 편하다.
   * 한 줄씩 AND 조건문을 적기 편하기 때문.
2. 동적쿼리에서 특정상황마다 WHERE절을 다르게 작성해줘야 할때 편리하다.

#### 동적쿼리란?

* 조건에 따라서 결과값이 달라지는 쿼리를 말한다.
* 대표적으로, 파라미터를 받아서 조건을 거는 경우가 있다.
* 참고로, 정적쿼리는 `SELECT * FROM users WHERE users.id = 1` 처럼

### 주의 사항

조회\(select\) 쿼리에서는 WHERE 1=1은 훌륭한 전략이 될 수도 있다. 매번 WHERE 절을 컨트롤할 필요 없기 때문이다.

그러나 갱신\(Update\) 쿼리 또는 삭제\(Delete\) 쿼리에서는 문제가 생길 수 있다. Java 파일에 Stringbuffer로 아래처럼 쿼리문을 생성했다고 가정하자.

```java
StringBuffer sql = new StringBuffer();

sql.append("\n DELETE FROM test_tbl");
sql.append("\n WHERE 1=1 ");

if( first != null ){
	sql.append("\n AND first = '1' ");
}
if( second != null ){
	sql.append("\n AND second = '1' ");
}
```

만약 first와 second가 null이라면, 아래 코드와 동일해지면서 `test_tbl` 이 모두 날아간다.

```text
DELETE *
FROM test_tbl
WHERE 1=1;
```

## 참고

* [https://jdm.kr/blog/7](https://jdm.kr/blog/7)
* [https://hyjykelly.tistory.com/5](https://hyjykelly.tistory.com/5)

