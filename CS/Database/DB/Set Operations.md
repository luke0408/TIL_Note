## 집합 연산자의 종류

|  연산자   |  의미  | 설명                                                         |
|:---------:|:------:|:------------------------------------------------------------ |
|   UNION   | 합집합 | 중복을 제거한 결과의 합을 검색                               |
| UNION ALL | 합집합 | 중복을 포함한 결과의 합을 검색                               |
| INTERSECT | 교집합 | 양쪽 모두에서 포함된 행을 검색                               |
|  EXCEPT   | 차집합 | 첫 번째 검색 결과에서 두 번째 검색 결과를 제외한 나머지 검색 | 

## 합집합

- 예시: union
	```SQL
	(select course_id
	from section
	where 
		sem = 'Fall' and
		year = 2009)
	UNION
	(select coure_id
	from section
	where
		sem = 'Spring' and
		year = 2010);
	```
- 예시: union all
	```SQL
	(select course_id
	from section
	where 
		sem = 'Fall' and
		year = 2009)
	UNION ALL
	(select coure_id
	from section
	where
		sem = 'Spring' and
		year = 2010);
	```

## 교집합
> 참고자료: https://yahwang.github.io/posts/52

- 예시: intersect - mysql에서는 사용 불가능
	```SQL
	(select course_id
	from section
	where 
		sem = 'Fall' and
		year = 2009)
	INTERSECT
	(select coure_id
	from section
	where
		sem = 'Spring' and
		year = 2010);
	```

- 예시: join을 통한 교집합 구현
	```SQL
	select a.curse_id
	from section as a
	inner join (
		select b.curse_id
		from section as b
		where
			b.sem = 'Spring' and
			b.year = 2010 ) 
		as b on a.curse_id = b.curse_id
	where
		sem = 'Fall' and
		year = 2009;
	```

## 차집합
> 참고자료: http://intomysql.blogspot.com/2011/01/mysql-minus-intersect.html

성능: LEFT JOIN > NOT EXISTX > NOT IN 순서로 성능이 좋다고 함.

- 예시: except - mysql에서 사용 불가능
	```SQL
	(select course_id
	from section
	where 
		sem = 'Fall' and
		year = 2009)
	EXCEPT
	(select coure_id
	from section
	where
		sem = 'Spring' and
		year = 2010);
	```
- 예시: NOT IN을 이용하는 방법
	```SQL
	select distinct a.curse_id
	from section as a
	where 
		sem = 'Fall' and 
		year = 2009 and
		(a.curse_id) not in (
			select coure_id
			from section
			where
				sem = 'Spring' and
				year = 2010);
	```
- 예시: NOT EXISTS를 이용하는 방법
	```SQL
	select distinct a.curse_id
	from section as a
	where 
		sem = 'Fall' and 
		year = 2009 and
		not exists (
			select coure_id
			from section
			where
				sem = 'Spring' and
				year = 2010);
	```
- 예시: LEFT(OUTER) JOIN을 이용하는 방법
	```SQL
	select distinct a.curse_id
	from section as a
	left join (
		select b.curse_id
		from section as b
		where
			b.sem = 'Spring' and
			b.year = 2010 ) 
		as b on b.curse_id = a.curse_id
	where
		sem = 'Fall' and 
		year = 2009;
	```