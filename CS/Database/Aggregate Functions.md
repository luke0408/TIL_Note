
## 집계 함수(Aggregate Funtion)란?

여러 행으로부터 하나의 결괏값을 반환하는 함수로 select 구문에서만 이용됨.
주로 평균, 합, 최대, 최소 등을 구하는데 이용

## COUNT

특정 열의 개수를 세는 함수로 COUNT\(\*\)로 작성하면 테이블에 존재하는 row의 개수를 반환하며 특정 column에 대해 수행하면 `해당 column이 null이 아닌 row의 개수를 반환한다.`

- 예시
	```SQL
	select count(*) from table_name;
	select count(column_name) from table_name;
	select count(distinct column_name) from table_name; -- 중복 제거 후 집계
	```
## MIN / MAX

말 그대로 최솟값과 최댓값을 구하는 함수.
숫자 외의 타입에서도 사용 가능하다.

| 타입 | MAX                          | MIN    |
|:----:|:---------------------------- |:------ |
| 숫자 | 최솟값                       | 최댓값 |
| 문자 | 사전 순으로 가장 뒤의 데이터 | 사전 순으로 가장 앞선 데이터       |

- 예시
	```SQL
	select MIN(column_name) from table_name;
	select MAX(column_name) from table_name;
	```

## AVG / SUM

AVG는 `선택한 column의 평균`을 계산하며, SUM `선택한 column의 합`을 계산함.
MIN/MAX와 다르게 숫자인 값에 대해서만 연산이 가능하며, NULL은 무시한다.

> 참고: 만약 NULL을 0으로 취급하여 계산하고 싶다면 NULL을 0으로 지정하는 작업을 해줘야한다.

- 예시: NULL 무시
	```SQL
	select avg(column_name) from table_name;
	select sum(column_name) from table_name;
	```
- 예시: NULL 값 변환 (IFNULL 사용)
	```SQL
	-- IFNULL(column_name, 값) 해당 데이터가 NULL인 경우 값으로 변환
	select avg(ifnull(column_name, 0)) from table_name;
	select sum(ifnull(column_name, 0)) from table_name;
	```

## GROUP BY

이전까지의 집계 함수가 테이블의 모든 행에 대한 계산을 했다면,
GROUP BY를 통해 테이블의 일부 행을 대상으로 집계 함수를 적용할 수 있다.

- 예시
	- GROUP BY 뒤에 적은 열(Column) 값들로 그룹을 묶어서 각 그룹별로 SUM을 구해주는 방식이다.
	```SQL
	-- 각 나라별 age의 합
	select country, sum(age)
	from table_name
	group by country;
	```

## HAVING
GROUP BY와 함께 쓰이는 조건문으로 집계 함수를 사용할 수 없는 WHERE 절과 다르게 집계 함수 사용이 가능하다.

- 예시
	```SQL
	-- 평균 나이가 23세 이상인 나라들
	select country, avg(age)
	from table_name
	group by country
	having avg(age) >= 23;
	```