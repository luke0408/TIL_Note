> 참고: https://www.w3schools.com/sql/sql_null_values.asp

## NULL Value란?

필드의 값이 null 인 경우 그 필드는 값이 없는 필드라고 함.
테이블의 필드가 선택적이면 그 필드에 값을 추가하지 않고 새 레코드를 삽입하거나 업데이트 할 수 있음. 그 다음 해당 필드가 nulll 값으로 지정됨.


> 참고: NULL 값이 0 또는 공백이 있는 필드와는 다른 의미를 지닌다는 것을 이해해 해야함.
> NULL 값이 있는 필드는 값 자체가 없는 것을 의미 함.


## NULL 비교 연산

> NULL은 일반적인 비교 연산자로 비교할 수 없으며 `is null` 또는 `is not null`을 이용해야 한다.
### 비교 연산자

- 기본적으로 NULL 값이 포함되는 연산의 return 값은 NULL이다.

- OR
	```SQL
	null or true = true
	null or false = unknown
	null or null = unknown
    ```
- AND
	```SQL
	null and true = unknown
	null and false = false
	null and null = unknown
	```
- NOT
	```SQL
	not null = unknown
	 ```

### IS NULL

- 예시: 특정 column이 null인 경우
	```SQL
	select column_name
	from table_name
	where column_name is null;
	```

### IS NOT NULL

- 예시: 특정 column이 null이 아닌 경우
	```SQL
	select column_name
	from  table_name
	where column_name is not null;
	```
