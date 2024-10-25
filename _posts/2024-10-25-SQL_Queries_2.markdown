---
title: "{ SQL } Basic SQL Queries_2"
date: 2024-10-25 03:00:00 KST
tags: [Oracle, SQL, Queries]
---

# SQL 노트 정리

## 기본 SQL 쿼리

### 테이블 구조 조회

```sql
desc [테이블명];
```

### 동일한 중복 레코드 한번만 출력

```sql
select distinct [필드명] from [테이블명];
```

### 숫자 함수

- **ABS**: 절대값
- **FLOOR**: 소수점 아래 제거
- **ROUND**: `ROUND(대상, 자릿수)`  
  예시) `ROUND(34.5678, 2)`: 소수점 세 번째에서 반올림
- **MOD**: 나머지 연산

### 기존 테이블에 컬럼 추가

```sql
alter table emp201
add LOC varchar2(100);
```

### 데이터 업데이트

```sql
UPDATE emp201 SET LOC='서울' WHERE EMPNO=11;
```

### 데이터 조회

```sql
SELECT * FROM emp201;
```

---

## 동의어 (Synonym)

동의어는 다른 사용자의 데이터베이스 객체에 접근할 때 좀 더 간단하게 참조할 수 있는 별칭 역할을 하는 객체입니다.

### 동의어의 종류

1. **비공개 동의어**: 특정 사용자가 정의하며, 그 사용자가 권한을 부여한 사용자만 접근 가능
2. **공개 동의어**: 모든 사용자가 접근할 수 있으며, DBA 권한을 가진 사용자만 생성 가능

### 동의어 생성 규칙

#### 비공개 동의어 생성

```sql
CREATE synonym [동의어명] FOR [사용자명].[객체명];
```

- 예시: `CREATE synonym dept FOR fintech.dept;`  
  이제 `fintech.dept` 테이블을 `dept`라는 이름으로 참조 가능

#### 공개 동의어 생성

```sql
CREATE PUBLIC synonym [동의어명] FOR [사용자명].[객체명];
```

- 예시: `CREATE PUBLIC synonym dept FOR fintech.dept;`  
  모든 사용자가 `dept`를 통해 `fintech.dept`에 접근 가능

### 동의어 삭제

- **비공개 동의어 삭제**: `drop synonym [동의어명];`
- **공개 동의어 삭제** (DBA 권한 필요): `drop public synonym [동의어명];`

---

## PL/SQL

PL/SQL은 SQL을 절차적 프로그래밍 기능(변수 선언, 조건문, 반복문, 예외 처리 등)으로 확장하여 복잡한 비즈니스 로직을 구현할 수 있습니다.

### 문법 구조

```sql
DECLARE
  -- 선언부 (선택적)
  변수 선언, 상수 선언
BEGIN
  -- 실행부
  SQL 문, 로직
EXCEPTION
  -- 예외 처리부 (선택적)
  예외 처리 로직
END;
```

### 예시 1: 기본 변수 선언 및 출력

```sql
set serveroutput on

DECLARE
  vempno number(38);
  vename varchar2(50);
BEGIN
  vempno := 7788;
  vename := 'fintech';

  DBMS_OUTPUT.PUT_LINE('사원번호 / 사원이름');
  DBMS_OUTPUT.PUT_LINE('===============>');
  DBMS_OUTPUT.PUT_LINE(vempno || ' / ' || vename);
END;
/
```

### 예시 2: 테이블 데이터 조회 후 출력

```sql
set serveroutput on

DECLARE
  vempno emp202.empno%type;
  vename emp202.ename%type;
BEGIN
  DBMS_OUTPUT.PUT_LINE('사원번호 / 사원이름');
  DBMS_OUTPUT.PUT_LINE('==============');

  SELECT empno, ename INTO vempno, vename
  FROM emp202
  WHERE ename = '홍길동';

  DBMS_OUTPUT.PUT_LINE(vempno || ' / ' || vename);
END;
/
```

### 예시 3: 테이블 타입 변수 사용

```sql
set serveroutput on

DECLARE
  -- 테이블 타입 정의: 배열로 선언되어 인덱스 주소 번호로 접근
  TYPE ename_table_type IS TABLE OF emp201.ename%type INDEX BY binary_integer;
  TYPE Loc_table_type IS TABLE OF emp201.LOC%type INDEX BY binary_integer;

  -- 2개의 테이블 타입 변수 정의
  ename_table ename_table_type;
  LOC_table LOC_table_type;

  i binary_integer := 0;
BEGIN
  -- emp201 테이블로부터 사원명과 지역 데이터를 검색해 테이블 타입 변수에 저장
  FOR k IN (SELECT ename, LOC FROM emp201) LOOP
    i := i + 1;
    ename_table(i) := k.ename;
    LOC_table(i) := k.LOC;
  END LOOP;

  DBMS_OUTPUT.PUT_LINE('===============');

  -- 테이블 타입 변수 출력
  FOR j IN 1..i LOOP
    DBMS_OUTPUT.PUT_LINE(ename_table(j) || ' / ' || LOC_table(j));
  END LOOP;
END;
/
```

```sql

emp201 테이블의 ename 컬럼 타입을 기반으로 한 테이블 타입을 정의

TYPE: 새로운 데이터 타입을 정의하는 키워드입니다.
ename_table_type: 새로 정의된 데이터 타입의 이름으로, 나중에 이 타입을 기반으로 변수를 선언할 수 있습니다.
IS TABLE OF emp201.ename%type:
TABLE OF: 테이블 타입을 나타내며, 데이터를 배열 형식으로 저장할 수 있습니다.
emp201.ename%type: emp201 테이블의 ename 열의 데이터 타입을 참조합니다. %TYPE을 사용하여 이 컬럼의 데이터 타입을 그대로 가져옵니다.
INDEX BY binary_integer: 이 배열(테이블 타입)은 정수 인덱스(binary_integer)로 데이터를 저장하고 조회할 수 있습니다.
```

---
