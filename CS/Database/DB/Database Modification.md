## 데이터 수정 (Update)

```SQL
UPDATE table_name
SET column_1 = data_1, column_2 = data_2, ...
WHERE column_name = data; -- 조건식 안쓰면 테이블 전체가 바뀜
```

## 데이터 삽입 (Insert)

- 반드시 모든 필드의 값을 가져야 할 필요는 없으며, 빈 값은 자동으로 NULL로 채워짐.

```SQL
-- 필드 몇개만 정해서 넣을 때
INSERT INTO table_name(column_1, column_2, ...)
VALUES (data_1, data_2, ...)

-- 필드 전체를 넣을때 (필드명 생략 가능)
INSERT INTO table_name
VALUES (data_1, data_2, ...)
```

## 데이터 삭제 (delete / truncate)

### delete (휴지통)
- 트랜잭션 로그를 기록해서 속도가 느림.
- 지운거 복구가능.
- 테이블 자체 용량은 줄어들지 않음.

```SQL
DELETE FROM table_name
WHERE column_name = data; -- 없으면 모든 데이터가 삭제됨.
```

### truncate (영구 삭제)
- 테이블 구조만 남기고 데이터 값 삭제
- 복구 불가

```SQL
TRUNCATE TABLE table_name;
```