---
title: "{ SQL } Basic SQL Queries_2"
date: 2024-10-25 03:00:00 KST
tags: [Oracle, SQL, Queries]
---

아래는 요청하신 내용을 간결하게 정리한 SQL 노트입니다.

---

## **기본 SQL 쿼리**

### **테이블 구조 조회**

```sql
DESC [테이블명];
```

### **중복 레코드 제거**

```sql
SELECT DISTINCT [필드명] FROM [테이블명];
```

---

## **숫자 함수**

- `ABS`: 절대값 반환.
- `FLOOR`: 소수점 이하 제거.
- `ROUND`: 지정한 자릿수에서 반올림.  
  예: `ROUND(34.5678, 2)` → 34.57
- `MOD`: 나머지 반환.  
  예: `MOD(10, 3)` → 1

---

## **테이블 관리**

### **컬럼 추가**

```sql
ALTER TABLE emp201
ADD LOC VARCHAR2(100);
```

### **데이터 업데이트**

```sql
UPDATE emp201
SET LOC = '서울'
WHERE EMPNO = 11;
```

### **데이터 조회**

```sql
SELECT * FROM emp201;
```

---

## **동의어 (Synonym)**

동의어는 다른 사용자 데이터베이스 객체의 별칭 역할을 합니다.

### **동의어 생성**

1. **비공개 동의어**

   ```sql
   CREATE SYNONYM [동의어명] FOR [사용자명].[객체명];
   ```

   예: `CREATE SYNONYM dept FOR fintech.dept;`

2. **공개 동의어**
   ```sql
   CREATE PUBLIC SYNONYM [동의어명] FOR [사용자명].[객체명];
   ```
   예: `CREATE PUBLIC SYNONYM dept FOR fintech.dept;`

### **동의어 삭제**

- 비공개: `DROP SYNONYM [동의어명];`
- 공개: `DROP PUBLIC SYNONYM [동의어명];` (DBA 권한 필요)

---

## **PL/SQL**

PL/SQL은 SQL을 확장하여 절차적 프로그래밍(변수, 조건문, 반복문, 예외 처리)을 제공합니다.

### **PL/SQL 구조**

```sql
DECLARE
  -- 변수 선언부
BEGIN
  -- 실행부
EXCEPTION
  -- 예외 처리부
END;
```

---

### **예제 1: 기본 변수 선언**

```sql
SET SERVEROUTPUT ON;

DECLARE
  vempno NUMBER(38);
  vename VARCHAR2(50);
BEGIN
  vempno := 7788;
  vename := 'fintech';

  DBMS_OUTPUT.PUT_LINE('사원번호: ' || vempno || ', 사원이름: ' || vename);
END;
/
```

---

### **예제 2: 테이블 데이터 조회**

```sql
SET SERVEROUTPUT ON;

DECLARE
  vempno emp201.empno%TYPE;
  vename emp201.ename%TYPE;
BEGIN
  SELECT empno, ename INTO vempno, vename
  FROM emp201
  WHERE ename = '홍길동';

  DBMS_OUTPUT.PUT_LINE('사원번호: ' || vempno || ', 사원이름: ' || vename);
END;
/
```

---

### **예제 3: 테이블 타입 변수 사용**

```sql
SET SERVEROUTPUT ON;

DECLARE
  -- 테이블 타입 정의
  TYPE ename_table_type IS TABLE OF emp201.ename%TYPE INDEX BY BINARY_INTEGER;
  TYPE loc_table_type IS TABLE OF emp201.loc%TYPE INDEX BY BINARY_INTEGER;

  ename_table ename_table_type; -- 사원명 배열
  loc_table loc_table_type;     -- 지역 배열

  i BINARY_INTEGER := 0; -- 인덱스 초기화
BEGIN
  -- 데이터 조회 및 배열 저장
  FOR record IN (SELECT ename, loc FROM emp201) LOOP
    i := i + 1;
    ename_table(i) := record.ename;
    loc_table(i) := record.loc;
  END LOOP;

  -- 데이터 출력
  FOR j IN 1..i LOOP
    DBMS_OUTPUT.PUT_LINE(ename_table(j) || ' / ' || loc_table(j));
  END LOOP;
END;
/
```

---

### 테이블 타입 변수 설명

- **`TYPE`**: 새로운 데이터 타입 정의.
- **`TABLE OF`**: 배열 타입 선언.
- **`INDEX BY BINARY_INTEGER`**: 정수 인덱스로 데이터 접근.

예:

```sql
TYPE ename_table_type IS TABLE OF emp201.ename%TYPE INDEX BY BINARY_INTEGER;
```

- **`emp201.ename%TYPE`**: `ename` 컬럼의 데이터 타입을 참조.
- **`BINARY_INTEGER`**: 정수형 인덱스를 사용.

---
