## Rename - as

- SQL에서 이름을 변경할 때 `as`라는 Operation을 사용한다.
	```SQL
	old-name as new-name
	```

- Ex. 'Comp. Sci'의 강사보다 높은 월급을 가진 강사들의 이름을 찾으시오
	```SQL
	select distinct T.name
	from 
		instructor as T,
		instructor as S
	where 
		T.salary > S.salary and
		S.dept_name = 'Comp. Sci';
	```

## String - Patterns

- SQL에서는 String을 표현할 때 '' 또는 ""를 이용한다.

### LIKE
> 문자열 패턴 일치 검사

| 기호 | 설명                      |
|:----:|:------------------------- |
|  %   | 0개 이상의 문자를 대체함. |
|  _   | 1개의 문자를 대체함.       |

```SQL
slect * from table
where 필드명 like "_영_" -- 가운데 글자가 영 인 사람
where 필드명 like "이%" -- 성이 이씨인 사람 
where 필드명 like "%이%" -- 이름에 이가 들어가는 사람
where 필드명 like "_종신" -- 종신 성씨 아무거나 
where 필드명 like "20__" -- 2000,2002 같은 네자리 숫자만. 20000 안됨.
```

### REGEXP
> mysql에서 정규 표현식 사용

|  **패턴**  | **설명**                                           |
|:----------:| -------------------------------------------------- |
|     .      | 줄 바꿈 문자(\n)를 제외한 임의의 한 문자를 의미함. |
|     *      | 해당 문자 패턴이 0번 이상 반복됨.                  |
|   **+**    | 해당 문자 패턴이 1번 이상 반복됨.                  |
|   **^**    | 문자열의 처음을 의미함.                            |
|   **$**    | 문자열의 끝을 의미함.                              |
|   **\|**   | 선택을 의미함.(OR)                                 |
| **[...]**  | 괄호([]) 안에 있는 어떠한 문자를 의미함.           |
| **[^...]** | 괄호([]) 안에 있지 않은 어떠한 문자를 의미함.      |
|  **{n}**   | 반복되는 횟수를 지정함.                            |
| **{m,n}**  | 반복되는 횟수의 최솟값과 최댓값을 지정함.          |

```SQL
select * from Reservation
where Name REGEXP '^홍|산$'; -- '홍'으로 시작하거나 or '산'으로 끝나는 문자열 검색
```

```SQL
select * from Reservation
where Name NOT REGEXP '^홍|산$'; -- '홍'으로 시작하거나 or '산'으로 끝나지 않는 문자열 검색
```

## Ordering

- select 문을 사용할 때 출력되는 데이터를 오름차 혹은 내림차로 정렬할 때 이용

| 정렬 규칙 | 설명                   |
|:---------:|:---------------------- |
|    asc    | 오름차순 정렬(기본 값) |
|   desc    | 내림차순 정렬          |


- 기본 형태
	```SQL
	select * 
	from table_name 
	order by column_name; -- 해당 column 기준으로 오름차순 정렬
	```
- AS 사용
	```SQL
	select sal + comm as total -- sal과 commn을 합한 column을 total로 rename
	from emp
	order by total; -- 위에서 만든 column을 기준으로 정렬
	```
- column index 사용
	```SQL
	select *
	from table_name
	order by 3; -- 3번째 column을 기준으로 정렬
	```
- 여러 column 대상으로 사용
	```SQL
	select *
	from instructor
	order by 
		name asc, -- name 기준으로 오름차순 정렬 후
		dept_name desc; -- 같은 name 사이에서 dept_name 기준으로 내림차 정렬
	```

## Between

- select 문에서 범위 검색을 할 때 이용

|        문법         | 설명                                                     |
|:-------------------:|:-------------------------------------------------------- |
|   between A and B   | A와 B사이의 값 출력 (이때, A의 값이 항상 B보다 작아야함) |
| not between A and B | A와 B범위를 벗어나는 값 출력                             | 

- 숫자 범위 검색
	```SQL
	select name
	from member
	where 
		salary between 5000 and 10000;
	```
- 문자열 범위 검색
	```SQL
	select *
	from member
	where
		name between '강감찬' and '홍길동';		
	```
- 날짜 범위 검색
	```SQL
	select name
	from member
	where
		date between '9/11/2001' and '9/11/2019';
	```
- 범위 외의 값 검색
	```SQL
	select *
	from member
	where
		salary not between 5000 and 10000;
	```
