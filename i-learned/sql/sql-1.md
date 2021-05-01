---
description: ORM만 쓰다보니 헷갈리는 SQL 개념을 잡는다.
---

# 📔 모두의 SQL

### 조인\(Join\)

조인 기법의 종류

* 곱집합\(cartesian product\)
  * 가능한 모든 행을 조인
* 동등 조인\(equi join or inner join\)
  * 조인 조건이 정확히 일치하는 경우에 결과 출력
* 비동등 조인\(non equi join\)
  * 조인 조건이 정확히 일치하지 않는 경우에 결과를 출력
* 외부 조인\(outer join\)
  * 조인 조건이 정확히 일치하지 않아도 모든 결과를 출력
* 자체 조인\(self join\)
  * 자체 테이블에서 조인하고자 할 때 사용

#### 동등 조인 예시

```text
-- employees 테이블과 departments 테이블과 locations 테이블을 조인 예시
​
SELECT  A.employee_id, A.department_id, B.department_name, C.location_id, C.city
FROM employees A, departments B, locations C
WHERE A.department_id = B.department_id
AND B.location_id = C.location_id;
```

#### 외부 조인

외부 조인은 조건을 만족하지 않은 행도 모두 출력하기 위한 조인 기법.

```text
-- employees 테이블과 departments 테이블을 department_id로 외부 조인하여 department_id가 null 값인 Kimberely Grant도 함께 출력해라.
​
SELECT A.employee_id, A.first_name, A.last_name, B.department_id, B.department_name
FROM employees A, departemnts B
WHERE A.department_id = B.department_id(+)
ORDER BY A.employee_id;
```

아래처럼 쉽게 생각해보자.

1. 양쪽 테이블 중 전부 출력하고 싶은 테이블 쪽을 먼저 생각한다.
2. \(+\)는 다른 쪽 테이블 쪽 조인 조건에 붙인다.

#### 자체 조인

자체 조인을 사용하려면 별칭을 사용해야 한다.

```text
-- employees 테이블을 자체 조인하여 직원별 담당 매니저가 누구인지 조회하자.
SELECT A.employee_id, A.first_name, A.last_name, A.manager_id, B.first_name || ' ' B.last_name manger_name
FROM employees A, employees B
WHERE A.manager_id = B.employee_id
ORDER BY A.employee_id;
```

### 집합 연산자

집합 연산자를 이용해도 테이블을 연결할 수 있다. 간단하게 합집합, 교집합, 차집합이라 생각하면 된다.

종류

* UNION
  * SELECT 문의 조회 결과의 합집합. 중복되는 행은 한 번만 출력한다. \(합집합\)
* UNION ALL
  * SELECT 문의 조회 결과의 합집합. 중복되는 행도 그대로 출력한다. \(합집합\)
* INTERSET
  * SELECT 문의 조회 결과의 교집합. 중복되는 행만 출력한다. \(교집합\)
* MINUS
  * 첫 번째 SELECT 문의 조회 결과에서 두 번째 조회 결과를 뺀다. \(차집합\)

```text
-- employees 테이블의 department_id 집합과 departments 테이블의 department_id 집합을 UNION 연산자를 이용해 합쳐 보세요.
​
SELECT department_id
FROM employees
UNION
SELECT department_id
FROM departments;
```

### 서브쿼리

서브쿼리의 결과는 메인 쿼리의 조건으로 사용된다.

메인 쿼리와 서브 쿼리의 연결 형태는 연산자에 따라 의미가 다르다.

| 연산자 구분 | 종류 | 사용처 |
| :--- | :--- | :--- |
| 단일 행 연산자 | =, &gt;, &gt;=, &lt;, &lt;=, &lt;&gt;, != | 단일 행 서브쿼리, 다중 열 서브쿼리 |
| 다중 행 연산자 | IN, NOT IN, EXISTS, ANY, ALL | 다중 행 서브쿼리, 다중 열 서브쿼리 |

#### 단일 행 서브쿼리

```text
-- employees 테이블의 last_name이 'De Haan'인 직원과 salary가 동일한 직원에는 누가 있는지 단일 행 서브쿼리를 이용해서 출력해보자.
​
SELECT *
FROM employees A
WHERE A.salary = (
                  SELECT salary
                  FROM employees
                  WHERE last_name = 'De Haan'
                  )
```

#### 다중 행 서브쿼리

```text
-- employees 테이블에서 department_id별로 가장 낮은 salary가 얼마인지 찾아보고, 찾아낸 salary에 해당하는 직원이 누구인지 다중 행 서브쿼리를 이용해 찾아보자.
​
SELECT *
FROM employees A
WHERE A.salary = IN (
                    SELECT MIN(salary) 최저급여
                    FROM employees
                    GROUP BY department_id
                    )
ORDER BY A.salary DESC;
```

#### 다중 열 서브쿼리

```text
-- employees 테이블에서 job_id별로 가장 낮은 salary가 얼마인지 찾아보고, 찾아낸 job_id별 salary에 해당하는 직원이 누구인지 다중 열 서브쿼리를 이용해 찾아보자.
​
SELECT *
FROM employees A
WHERE (A.job_id, A.salary) IN  (
                                SELECT job_id, MIN(salary) 그룹별급여
                                FROM employees
                                GROUP BY job_id
                                )
ORDER BY A.salary DESC;
```

#### FROM 절 서브쿼리: 인라인 뷰

FROM 절에서도 서브쿼리를 사용할 수 있다!

```text
-- 직원 중에서 department_name이 IT인 직원의 정보를 인라인 뷰를 이용해서 출력해 보라.
​
SELECT *
FROM employees AS A,
                  ( 
                  SELECT department_id
                  FROM departments
                  WHERE department_name = 'IT'
                  ) AS B
WHERE A.departemnt_id = B.department_id;
```

