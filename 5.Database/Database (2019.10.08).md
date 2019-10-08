# Database (2019.10.08)

## 데이터베이스의 구성 요소

> 개체(entity)와 그들이 가지는 속성(attribute), 그리고 개체 사이의 관계(relation)

- entity: 하나의 열

- attribute: 어떤 데이터들의 속성

- relation



## 데이터베이스의 장점

- 데이터 중복 최소화

- 데이터 무결성

- 데이터 일관성

- 데이터 독립성

- 데이터 표준화

- 데이터 보안 유지



## RDBMS

> 관계형 모델을 기반으로 하는 데이터베이스의 관리시스템이다. 모두 SQL이라는 문장을 사용하여 데이터를 관리한다.

하고자 하는 목표는 똑같으나 프로그램이 다르다.

SQL을 배우는 것은 SQL을 마스타하는 것이 아닌, 데이터의 흐름을 알기 위한 것이다.

Django는 SQL에 직접 접근하여 데이터를 사용한다.



### Relational DB

> 개체와 개체사이의 관계를 표현하기 위한 2차원 표



### SQLite

> 간단한 DB 구성을 할 수 있다.

Django는 SQLite 3버전을 사용한다.



### Database Ranking

> 구글 검색 해보자. Database Model을 확인해보자.

Relational은 SQL 문법이 적용가능하다. 보편적으로 사용된다.

나머지는 noSQL로 분류된다. noSQL은 점점 증가하는 편이다. 상황에 따라, 필요에 따라 배우게 될 예정이다.

MongoDB가 최근 핫하다.



### 스키마 (schema)

> 실제 데이터베이스는 아니지만 데이터베이스를 사용하기 위한 규격이나 정의라고 할 수 있다.

규격을 정하는 관계를 말한다. 중간 번역단계라고 할 수 있다.



### 테이블 (table)

스키마를 테이블(table)로 구현한다.

Model의 개수가 table의 개수가 된다.



### 열 (Columm, Attribute)

고유한 데이터 형식이 지정된다.



### 행 (row, 레코드)

하나의 데이터를 관리한다.



### PK (기본키, Primary Key)

> SQL에서 필요로하기 때문에 자동으로 생성되는 값이다. 각 행의 고유값이다.



## SQL (Structured Query Language)

> 데이터를 생성하고 지우고 수정하고 삭제하기 위한 프로그래밍 언어다. 정의, 조작, 제어



### DDL (데이터 정의 언어)

> table, schema

테이블 생성하거나 삭제하는 역할을 한다.



### DML (데이터 조작 언어)

> 데이터를 조작한다. 본질적으로 많이 사용하는 코드이다.



### DCL (데이터 제어 언어)

> 권한 제어를 위해 사용하는 언어다. 우리는 잘 사용하지 않을 것 같다.



### SQL Keywords

테이블에 데이터 삽입 (새로운 행 추가) insert

데이터 삭제 (행 제거) delete

데이터 갱신 update

데이터 검색 select



## SQLite3

`bit.do/hello_db`

student 파일안에 `sqlite.zip`을 저장한다.

내 PC -> 속성 -> 고급 시스템 설정 -> 환경변수 -> student Path에 `C:\Users\student\sqlite` 추가한다.

sqlite 폴더에서 vs code 실행후 터미널에서 sqlite3을 실행하면 command 창이 생성된다.

```sqlite
$ sqlite3
sqlite> .exit
```

`.exit`를 작성하면 나갈 수 있다.



```sqlite
$ sqlite3.text.sqlite3

sqlite> .databases
main: C:\Users\student\sqlite\test.sqlite3

sqlite> .mode csv
sqlite> .import hellodb.csv examples
sqlite> SELECT * FROM examples; # 키워드(SELECT문) 키워드(FROM table)
1,"길동","홍",600,"충청도",010-2424-1232

sqlite> .headers on
sqlite> .mode column
sqlite> SELECT * FROM examples;
id          first_name  last_name   age         country     phone
----------  ----------  ----------  ----------  ----------  -------------
1           길동          홍           600         충청도         010-2424-1232
```

`.headers on`과 `.mode column`을 사용하면 데이터를 표 형태로 만들어준다.



## 00_intro.sql

```sql
-- 데이터베이스 생성
.database

-- csv파일을 읽어오기
.mode csv
.import hellodb.csv examples

-- 예쁘게 보기
.headers on
.mode column

-- 테이블 조회
SELECT * FROM examples; -- SQL문법이기 때문에 ';'을 붙여야 한다.
```

```sqlite
$ sqlite3
sqlite> .read 00_intro.sql
main:
id          first_name  last_name   age         country     phone
----------  ----------  ----------  ----------  ----------  -------------
1           길동          홍           600         충청도         010-2424-1232
```



## 01_DDL.sql

### 구조 정의 & 목록 조회

```sql
-- DDL(데이터 정의 언어)
-- 구조를 정의
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT
);

-- 테이블 목록 조회
.tables
```

```sqlite
$ sqlite3
SQLite version 3.29.0 2019-07-10 17:32:03
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .read 01_DDL.sql
classmates  examples
```



### 스키마 조회

```sql
-- 스키마 조회
.schema classmates
```

```sqlite
sqlite> .read 01_DDL.sql
Error: near line 3: table classmates already exists
classmates  examples
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT
);
```



### 테이블 삭제

```sql
-- 테이블 삭제
DROP TABLE classmates;

.tables
```

```sqlite
sqlite> .read 01_DDL.sql
Error: near line 3: table classmates already exists
classmates  examples
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT
);
examples
```



### Datatype

> SQLite는 동적 데이터 타입이다. BOOLEAN은 정수 0, 1로 저장된다.



### 예제

```sql
-- 예제
CREATE TABLE classmates (
    name TEXT,
    age INT,
    address TEXT
);

.tables
.schema classmates
```

```sqlite
sqlite> .read 01_DDL.sql
classmates  examples
CREATE TABLE classmates (
    name TEXT,
    age INT,
    address TEXT
);
```



## 02_CRUD.sql

### Insert (data 추가)

```sql
CREATE TABLE classmates (
    name TEXT,
    age INT,
    address TEXT
);

-- CREATE
INSERT INTO classmates (name, age, address)
VALUES ('오창희', 11, '광주');

-- 확인
SELECT * FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
Error: near line 1: table classmates already exists
name        age         address
----------  ----------  ----------
오창희         11          광주
```



기본 설정은 값이 들어가지 않아도 된다.

```sql
-- CREATE
INSERT INTO classmates (name, address)
VALUES ('오창희', '광주');

-- 확인
SELECT * FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
Error: near line 1: table classmates already exists
name        age         address
----------  ----------  ----------
오창희         11          광주
sqlite> .read 02_CRUD.sql
Error: near line 1: table classmates already exists
name        age         address
----------  ----------  ----------
오창희         11          광주
오창희         11          광주
name        age         address
----------  ----------  ----------
오창희         11          광주
오창희         11          광주
오창희                     광주
```



### 예제

```sql
-- CREATE
-- INSERT INTO classmates (name, age, address)에서 (name, age, address)는 생략가능하다.
INSERT INTO classmates
VALUES ('홍길동', 30, '서울');

-- 확인
SELECT rowid, * FROM classmates; -- 'rowid,'를 '*'앞에 붙이면 id를 출력한다.

DROP TABLE classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
name        age         address
----------  ----------  ----------
오창희         11          광주
name        age         address
----------  ----------  ----------
오창희         11          광주
오창희                     광주
rowid       name        age         address
----------  ----------  ----------  ----------
1           오창희         11          광주
2           오창희                     광주
3           홍길동         30          서울
```

데이터 무결성의 원칙에 위배된다. id와 NOT NULL이 설정되지 않았다.



### 데이터 무결성

#### 오류1 (NOT NULL)

```sql
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);

INSERT INTO classmates (name, address)
VALUES ('김규리', '광주'); -- age와 id에 대한 정보를 넣지 않는다.

SELECT rowid, * FROM classmates;

DROP TABLE classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
Error: near line 40: NOT NULL constraint failed: classmates.age
```



#### 오류2

```sql
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);

INSERT INTO classmates
VALUES ('김규리', 24, '광주'); -- id에 대한 정보를 넣지 않는다.

SELECT rowid, * FROM classmates;

DROP TABLE classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
Error: near line 40: table classmates has 4 columns but 3 values were supplied
```



#### 오류 X

```sql
CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);

INSERT INTO classmates
VALUES (1, '김규리', 24, '광주');

SELECT rowid, * FROM classmates;

DROP TABLE classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
id          id          name        age         address
----------  ----------  ----------  ----------  ----------
1           1           김규리         24          광주
```



#### rowid 사용

```sql
-- 기본으로 제공하는 rowid를 사용
CREATE TABLE classmates (
    -- id INTEGER PRIMARY KEY, -- PRIMARY KEY는 INTEGER로 써야한다.
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);

INSERT INTO classmates (name, age, address)
VALUES ('김규리', 24, '광주');

SELECT rowid, * FROM classmates;

DROP TABLE classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name        age         address
----------  ----------  ----------  ----------
1           김규리         24          광주
```



### SELECT

#### 특정 column 가져오기

```sql
CREATE TABLE classmates (
    -- id INTEGER PRIMARY KEY, -- PRIMARY KEY는 INTEGER로 써야한다.
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
);

INSERT INTO classmates (name, age, address)
VALUES
('홍길동', 24, '서울'),
('김철수', 25, '대전'),
('박나래', 26, '광주'),
('이요셉', 27, '구미');

-- 특정 column 가져오기
SELECT rowid, name FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name
----------  ----------
1           홍길동
2           김철수
3           박나래
4           이요셉
```



#### LIMIT

```sql
-- LIMIT
SELECT rowid, name FROM classmates LIMIT 1;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name
----------  ----------
1           김규리
```



#### OFFSET (앞의 몇개를 skip)

```sql
-- OFFSET (앞의 몇개를 skip)
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name
----------  ----------
3           박나래
```



#### WHERE

```sql
-- WHERE
SELECT rowid, name FROM classmates WHERE address="서울";
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name
----------  ----------
1           홍길동
```



#### 중복제거

```sql
-- 중복제거
SELECT DISTINCT age FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
age
----------
24
25
26
27
```



### DELETE

```sql
-- DELETE
DELETE FROM classmates WHERE rowid=4;
SELECT rowid, * FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name        age         address
----------  ----------  ----------  ----------
1           홍길동         24          서울
2           김철수         25          대전
3           박나래         26          광주
```



### DELETE & INSERT

```sql
-- DELETE
DELETE FROM classmates WHERE rowid=4;
INSERT INTO classmates VALUES ('김이박', 30, '광주');
SELECT rowid, * FROM classmates;
```

```sqlite
sqlite> .read 02_CRUD.sql
rowid       name        age         address
----------  ----------  ----------  ----------
1           홍길동         24          서울
2           김철수         25          대전
3           박나래         26          광주
4           김이박         30          광주
```



### UPDATE

```sql
-- UPDATE
UPDATE classmates
SET name="홍길동", address="제주도"
WHERE rowid=4;
SELECT * FROM classmates;
```

```sql
sqlite> .read 02_CRUD.sql
name        age         address
----------  ----------  ----------
홍길동         24          서울
김철수         25          대전
박나래         26          광주
홍길동         27          제주도
```

