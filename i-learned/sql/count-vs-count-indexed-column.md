# COUNT\(\*\) vs COUNT\(indexed column\)

MySQL에서

COUNT\(\*\) : 단순 행을 세는 역할을 한다. \(MySQL 내부적으로 데이터를 읽지않고 행의 갯수만 흝고 지나간다는 뜻\)

COUNT\(컬럼\) : 행의 값을 세는 역할을 한다.\(데이터를 읽는다는 뜻\)

COUNT\(DISTINCT\(컬럼\)\) &lt; COUNT\(컬럼\) &lt; COUNT\(\*\) 순으로 빠르다. 



### 참고

* [https://www.phpschool.com/gnuboard4/bbs/board.php?bo\_table=tipntech&wr\_id=77484](https://www.phpschool.com/gnuboard4/bbs/board.php?bo_table=tipntech&wr_id=77484)

