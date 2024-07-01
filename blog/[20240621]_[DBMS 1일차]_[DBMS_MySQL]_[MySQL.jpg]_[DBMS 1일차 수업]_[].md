# 1. 데이터베이스와 SQL

## 1.1 데이터베이스란?

- 여러 사람들이 공유할 목적으로 통합 관리하는 데이터들의 집합.(데이터 창고)

## 1.2 DBMS란?

- “DataBase Management System”의 약자로 데이터 베이스를 관리하는 시스템 입니다.
- DBMS는 크게 SQL을 사용하는 RDBMS와 그외로 SQL을 사용하지 않는 NO SQL이 있습니다.
    - RDBMS: MySQL, MariaDB, Oracle Database 등등
    - NO SQL: Mongo DB, Redis, HBase 등등

## 1.3 SQL이란?

- “SQL은 Structured Query Language”의 약자로 DB에 저장된 데이터를 조작할때 사용하는 명령어 입니다.
- 데이터베이스에서 데이터를 추가 조회 수정을 하는데 특화 되어있습니다.(Create, Read, Update, Delete에서 앞 글자만 따와서 QRUD라고도 합니다.)
- SQL의 명령어
    1. **DDL(Data Definition Language)**: 데이터베이스나 테이블 등을 생성, 삭제하거나 구조를 변경하는 명령어(주요 명령어: CREATE, ALTER, DROP)
    2. **DML(Data Manipulation Language)**: 데이터베이스에 저장된 데이터를 처리하거나 조회, 검색하기 위한 명령어(주요 명령어: INSERT, UPDATE, DELETE등등)
    3. **DCL(Data Control Language)**: 데이터베이스에 저장된 데이터를 관리하기 위하여 데이터 보안성 및 무결성 등을 제어하기 위한 명령어(주요 명령어: GRANT, REVOKE등등)

# 2. RDBMS

## 관계형 데이터베이스란?

- Relational DataBase Management System의 약자로 관계형 데이터입니다. <br>
- RDBMS는 데이터를 2차원의 행과열 구조로 테이블을 저장 합니다.(엑셀이랑 비슷함)<br>
- 예시

   | ID | 나이 | 성별 |
   | --- | --- | --- |
   | abc | 21 | 남 |
   | abc1 | 22 | 여 |
- 위 테이블은 ID, 나이, 성별로 총 3개의 열과 2개의 행으로 이루어져 있습니다.
- 테이블에서 행은 ID별 정보를 나타내고, 열은 정보의 속성을 나타냅니다.(예시: ID를 기본키 라고 했을 때 ID는 고유 식별자를 나타내고, 나이와 성별은 ID별 정보들을 나타냅니다.)
    
    | ID(기본키) | 이름 | 주소 | 연락처 |
    | --- | --- | --- | --- |
    | 1 | 고객1 | 부산 | 010-1234-5678 |
    | 2 | 고객2 | 울산 | 010-5678-1234 |
## 관계형 데이터베이스 특징

- 관계형 데이터베이스는 각 테이블간의 관계를 정의하여 데이터 사이의 연관성을 표현할 수 있습니다.
- 예시(고객 정보 테이블과 주문 정보 테이블이 있을 때)
    - **고객 정보 테이블**
  
        | ID(기본키) | 이름 | 주소 | 연락처 |
        | --- | --- | --- | --- |
        | 1 | 고객1 | 부산 | 010-1234-5678 |
        | 2 | 고객2 | 울산 | 010-5678-1234 |

    - 주문 정보 테이블
                
        | 주문번호(기본키) | ID | 배송지 | 연락처 | 결제수단 | 총금액 |
        | --- | --- | --- | --- | --- | --- |
        | 1 | 1 | 부산 | 010-1234-5678 | 신용카드 | 10000 |
        | 2 | 1 | 부산 | 010-1234-5678 | 신용카드 | 15000 |
        | 3 | 2 | 울산 | 010-5678-1234 | 체크카드 | 20000 |

    - 고객 정보 에서는 ID를 기본키로 두었지만 하나의 아이디로 여러번 주문을 할 수 있기 때문에 주문 정보 에서는 ID가 아닌 주문 번호를 테이블의 기본키로 설정했습니다.
    - 이런 식으로 하나의 행이 다른 여러개의 행에 대응하는 것을 1:N관계라고 합니다.(1:1 관계는 말 그대로 하나의 행이 다른 하나의 행에 대응하는 것이고 여러개의 행이 다른 여러개의 행에 대응하는것을 N:M 관계라고 합니다.)

# 3. 데이터베이스 용어

## 테이블

* 데이터를 구성하기 위한 가장 기본적인 단위 입니다.
* 2차원의 행과 열로 구성되어있습니다.

## 행

* 행(row)는 테이블의 가로에 배열된 데이터의 집합 입니다.
* 반드시 고유한 식별자인 기본키를 가집니다

## 열

* 열(column)은 행에 저장되는 유형의 데이터 입니다.
* 하나의 테이블이 있을때 열은 각 요소에 대한 속성을 나타내고 무결성을 보장합니다.(열이 이름, 나이, 주소로 이루어져 있을 때 주소와 이름에는 문자열이 들어갈 수 있지만 나이에는 숫자형 데이터만 들어갈 수 있기 때문에 데이터의 무결성을 보장합니다.)

### 행열의 구조 예시

* 아래와 같은 형식으로 행과열을 이루어서 데이터를 나타냅니다

    | 1열(기본키) | 2열     | 3열      |
    |---------|--------|:--------|
    | 1행(1열)  | 1행(2열) | 1행(3열)  |
    | 2행(1열)  | 2행(2열) | 2행(3열)  |

## 기본키(primary key)

* 행을 대표하여 구분할 수 있는 식별자 입니다.
* 각 테이블 마다 기본키는 반드시 있어야 합니다.
* 기본키는 null값이 들어갈 수 없고, 키의 값이 수정될 수 없는 유효한 값이어야 합니다.

## 쿼리

* 데이터베이스에서 데이터를 조회, 삭제, 생성, 수정 같은 처리를 하기 위해 사용하는 명령문 입니다.

# SQL 기본 명령어

## DML(조작)

* Data Manipulation Language의 약자로 데이터를 조작하는데 사용합니다.
* 데이터를 조회, 삽입, 수정, 삭제하기 위한 문법입니다.
    1. SELECT: 데이터 조회
    2. INSERT: 데이터 삽입
    3. UPDATE: 데이터 수정
    4. DELETE: 데이터 삭제

### 조회

* SELECT, FROM, WHERE 세가지 키워드로 조회 합니다
    1. SELECT: 조회할 열을 지정(모두 지정하려면 SELECT *을 작성해주면 됨)
    2. FROM: 조회할 테이블을 지정
    3. WHERE: 조회할 데이터를 필터링(특정 조건에 해당하는 데이터 고름)

* 모든열 조회: 모든 열을 조회하고 싶은 경우 select 뒤에 *을 붙힙니다.

    ```sql
    SELECT * FROM 테이블명;
    ```

* 특정 열을 조회: select 뒤에 붙은 열에 해당하는 데이터만 불러옵니다.

    ```sql
    SELECT 열이름1, 열이름2 FROM 테이블 명;
    ```

* 별칭으로 열 이름 변경: AS 를 사용해서 특정 열의 이름을 별칭을 만든 후 해당 열의 데이터를 불러옵니다.

    ```sql
    SELECT 열이름1 AS 별칭1, 열이름2 AS 별칭2 FROM 테이블 명;
    ```

* 조건을 사용하여 데이터 필터: **`WHERE 조건`**을 작성하여

    ```sql
    SELECT * FROM 테이블명 WHERE 조건;
    ```

* 중복된 행 제거: **`SELECT DISTINCT 열이름`** 을 이용해서 해당열에 있는 데이터 중 중복된 값을 제거한 후 출력해 줍니다.

    ```sql
    SELECT DISTINCT 열이름 FROM 테이블명;
    ```

  * 예시

    ```sql
    SELECT DISTINCT name FROM 
    ```


### 삽입

* **`INSERT INTO`** : 새로운 행을 삽입할 때 사용합니다.

### 데이터 생성 및 삽입

* name, age, address로 구성된 studens 테이블에 데이터 삽입 후 출력

    ```sql
    CREATE TABLE students
    (
        name    varchar(255) NOT NULL ,
        age     int NOT NULL ,
        address varchar(255) NOT NULL
        -- NOT NULL을 제약조건으로 줬기 때문에 NULL값을 
    );
    
    INSERT INTO students(name, age, address)
    values ('조준호', 24, '부산'),
           ('김승조', 30, '서울');
    ```

* 결과

    <img src="blog/TIL/6:21/데이터 삽입.png" alt="#">


### 한 행 삽입

* 위의 예시에서 `('김성주', 35, '서울');` 삽입 후 출력

    ```sql
    INSERT INTO students(name, age, address)
    values ('김성주', 35, '서울');
    select *FROM students;
    ```

* 결과

  <img src="blog/TIL/6:21/데이터추가.png" alt="#">


### 한 행 + 특정열만 삽입

* name과 age열만 삽입 합니다.(address열은 null값이 들어감)

    ```sql
    INSERT INTO students(name, age)
    values ('이름1', 12);
    ```

* 결과

  <img src="blog/TIL/6:21/한행만추가.png" alt="#">


### 조회 후 삽입

```sql
INSERT INTO students(name,age,address)
SELECT name,age,address FROM students WHERE address = '부산';
```

* 결과

  <img src="blog/TIL/6:21/조회 후 삽입.png" alt="#">


### 여러 행 삽입

* 한번에 여러행을 삽입 합니다.

    ```sql
    INSERT INTO students(name, age, address)
    values ('학생2', 28, '서울'),
           ('학생3', 22, '서울');
    select *FROM students;
    ```
* 결과

  <img src="blog/TIL/6:21/여러행추가.png" alt="#">


### 수정(INSERT)

* UPDATE: 수정할 테이블을 지정<br>

* SET: 데이터 수정<br>

* WHERE: 수정할 데이터 필터링<br>

* students 테이블의 age 열에서 30이하인 행들만 address 열의 데이터를 ‘강원도’로 변경<br>

    ```sql
    UPDATE students
    SET address = '강원도'
    WHERE age <= 30;
    ```

* 결과

  <img src="blog/TIL/6:21/수정.png" alt="#">


### 삭제(DELETE)

DELETE FROM: 삭제할 테이블 지정

WHERE: 삭제할 데이터를 필터링

* student 테이블의 age가 25 이상인 행들 삭제

    ```sql
    DELETE
    FROM students
    WHERE age >= 25;
    SELECT * FROM students;
    ```

* 삭제 전

  <img src="blog/TIL/6:21/여러행추가.png" alt="#">

* 결과(삭제 후)

  <img src="blog/TIL/6:21/삭제후.png" alt="#">


# 회고

* MySQL을 사용해본 경험이 있어서 수업내용을 따라가는거에 큰 지장은 없는거같음(바로 스프링으로 넘어갔으면 좀 힘들었을거 같음…)