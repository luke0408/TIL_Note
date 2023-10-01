## 서브쿼리란?

서브쿼리(subquery)란 다른 쿼리 내부에 포함되어 있는 SELETE 문을 의미한다.

서브쿼리를 포함하고 있는 쿼리를 외부쿼리(outer query)라고 부르며, 서브쿼리는 내부쿼리(inner query)라고도 부른다.

[서브쿼리 실행 순서]

(2) 서브쿼리 실행 > (1) 메인 쿼리 실행

```SQL
select * -- (1)
from table_name as a
where a.column_name in ( -- (2)
	select b.column_name
	from table_name as b
	where b.column_name < 500;
)
```

(1) 메인 쿼리 - 서브쿼리의 칼럼 이용 불가능
(2) 서브쿼리 - 메인 쿼리의 칼럼 이용 가능

> 참고:
> 객체지향(ex. java)의 상속과 똑같은 개념
> 상속당한 자식 객체는 부모 객체의 인스턴스를 사용할 수 있고, 부모는 자식객체의 인스턴스를 사용할 수 없다.

## 서브쿼리의 위치에 따른 명칭

```SQL
select col, (select ...) -- 스칼라 서브쿼리
from (select ...)        -- 인라인 뷰 서브쿼리
where col = (select ...) -- 일반 서브쿼리
```

### 중첩 서브쿼리 (Nested Subquery)
> WHERE 문에서 사용하는 서브쿼리

- 예시: 조건 값을 select로 사용할 때 (단, 서브쿼리의 결과값이 하나여야 하는 경우)
	```SQL
	select name, height
	from users
	where height > (
		select height
		from users
		where name in ('홍길동')
	);
	```
- 예시: 조건 값을 여러 개 사용하고 싶을 때
	```SQL
	select name, height
	from users
	where height any ( -- 조건 값 중 하나만 맞으면 됨 (or)
		select height
		from users
		where address in ('서울')
	);
	```
	```SQL
	select name, height
	from users
	where height all ( -- 조건 값을 모두 만족시켜야 함 (and)
		select height
		from users
		where address in ('서울')
	);
	```

### 인라인 뷰 (Inline View)
> FROM 문에서 사용하는 서브쿼리

- 예시:  서브쿼리가 FROM 절에 사용될 경우 AS 별칭을 지정해주어야 함.
	```SQL
	select a.name, a.salary
	from (
		select *
		from employee as b
		where b.office_worker = '사원'
	) as a;
	```

### 스칼라 서브쿼리 (Scalar Subquery)
> SELECT 문에서 사용하는 서브쿼리

- 주의점
	- 다른 테이블에서 값을 가져올 때 쓰임
	- 하나의 값만 리턴이 가능하며, 두 개 이상의 값을 리턴 할 수 없음
	- 일치하는 데이터가 없더라도 NULL값을 리턴할 수 있다.

- 예시
	```SQL
	select b.deptno, (
		select min(empno)
		from emp
		where deptno = d.deptno) as empno
	from dept d
	order by d.deptno;
	```

## 서브쿼리 실행 조건

1. 서브쿼리는 SELECT문으로만 작성 가능함.
2. 괄호 안에 작성되어야 함.
3. 괄호 끝에 ;(세미콜론)을 쓰지 않는다.
4. ORDER BY를 사용할 수 없다.

### 서브쿼리 사용 가능 위치

- SELECT
- FROM
- WHERE
- HAVING
- ORDER BY
- INSERT문의 VALUES 부분 대체제
- UPDATE문의 SET 부분 대체제