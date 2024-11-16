---
title: "{ SQL } Queries_해석"
date: 2024-11-08 03:00:00 KST
tags: [SQL]
---

아래는 제공된 SQL 개념과 예시를 체계적으로 정리한 내용입니다.

---

## **SQL 쿼리 해석 순서**

1. **SELECT**: 쿼리의 목적 확인 (조회하고자 하는 데이터).
   - 예: `SUM`, `COUNT`, `AVG` 같은 집계 함수가 있으면 데이터를 요약하려는 목적.
2. **FROM**: 주요 테이블과 열 확인 (데이터의 출처).
3. **WHERE**: 조건문 해석 (필터링 조건).
   - 예: `WHERE SALE_DATE BETWEEN` → 특정 범위 내 데이터 조회.
4. **JOIN**: 테이블 간 관계 확인 (데이터 연결 방식).
   - INNER JOIN: 두 테이블에 모두 존재하는 데이터.
   - OUTER JOIN: 한쪽에만 존재해도 포함.
5. **GROUP BY / HAVING**: 데이터 그룹화와 필터링 이해.
   - GROUP BY: 데이터 그룹화 기준.
   - HAVING: 그룹화된 데이터에 추가 필터링.
6. **ORDER BY / LIMIT**: 데이터 정렬 및 출력 제한.

---

## **예시 해석**

### 예시 1: 고객별 총 주문 금액 조회

```sql
SELECT C.CUSTOMER_NAME, SUM(O.AMOUNT) AS TOTAL_AMOUNT
FROM CUSTOMERS C
INNER JOIN ORDERS O ON C.CUSTOMER_ID = O.CUSTOMER_ID
WHERE C.COUNTRY = 'SOUTH KOREA' AND O.ORDER_DATE >= '2023-01-01'
GROUP BY C.CUSTOMER_NAME
HAVING SUM(O.AMOUNT) > 5000
ORDER BY TOTAL_AMOUNT DESC
LIMIT 10;
```

#### 해석:

1. **목적**: 한국 고객 중 2023년 이후 주문 금액이 5000 초과한 고객의 상위 10명.
2. **FROM & JOIN**: 고객 테이블과 주문 테이블 연결 (CUSTOMER_ID 기준).
3. **WHERE**: 한국 거주, 2023년 이후 주문 필터링.
4. **GROUP BY / HAVING**: 고객 이름별로 그룹화, 총금액 5000 초과 필터링.
5. **ORDER BY / LIMIT**: 총금액 기준 내림차순 정렬, 상위 10명 출력.

---

### 예시 2: 부서별 직원 판매 순위

```sql
SELECT E.EMPLOYEE_NAME, E.DEPARTMENT_ID, S.SALE_AMOUNT,
       RANK() OVER(PARTITION BY E.DEPARTMENT_ID ORDER BY S.SALE_AMOUNT DESC) AS SALE_RANK
FROM EMPLOYEES E
INNER JOIN SALES S ON E.EMPLOYEE_ID = S.EMPLOYEE_ID
WHERE S.SALE_DATE BETWEEN '2023-01-01' AND '2023-12-31';
```

#### 해석:

1. **목적**: 부서별로 2023년 판매 금액에 따른 직원 순위 계산.
2. **FROM & JOIN**: 직원 테이블과 판매 테이블 연결 (EMPLOYEE_ID 기준).
3. **WHERE**: 2023년 내 판매 데이터 필터링.
4. **RANK()**: 부서별로 직원의 판매 금액 순위 산출 (`PARTITION BY DEPARTMENT_ID`).

---

## **조건 조합 쿼리 해석**

- **AND**: 두 조건 모두 만족 (`&&`).
- **OR**: 두 조건 중 하나만 만족 (`||`).
- **NOT**: 조건의 부정 (`!`).

### 예시 1: 복합 조건

```sql
SELECT * FROM employees
WHERE (department = 'Sales' OR department = 'Marketing')
  AND salary > 5000
  AND NOT (job_title = 'Intern' OR job_title = 'Contractor');
```

#### 해석:

1. **조건**:
   - 부서가 Sales 또는 Marketing.
   - 연봉이 5000 초과.
   - 직업이 Intern 또는 Contractor가 아님.
2. **목적**: 부서, 연봉, 직업 조건을 모두 만족하는 직원 조회.

---

### 예시 2: 복합 조건과 괄호 사용

```sql
SELECT *
FROM EMP
WHERE (DEPTNO = 10 AND JOB = 'CLERK')
   OR (DEPTNO = 20 AND JOB = 'CLERK')
   OR SAL > 3000;
```

#### 해석:

1. **조건**:
   - 부서번호 10에서 직업이 CLERK.
   - 부서번호 20에서 직업이 CLERK.
   - 연봉이 3000 초과.
2. **목적**: 위 조건 중 하나라도 만족하는 직원 조회.

---

## **GROUP BY, ROLLUP, CUBE**

### GROUP BY

- 데이터를 그룹화하여 집계 연산 수행.
- SELECT 절에 사용된 열은 GROUP BY에 반드시 포함되어야 함.

### ROLLUP

- 계층적 요약 데이터 생성 (A별, A+B별, 전체 합계).
- 순서 중요.

### CUBE

- 가능한 모든 조합에 대해 요약 데이터 생성 (A별, B별, A+B별, 전체 합계).
- 순서 중요하지 않음.

---

## **JOIN 종류**

1. **INNER JOIN**: 두 테이블 모두 조건에 만족하는 데이터만 출력.
2. **LEFT OUTER JOIN**: 왼쪽 테이블 기준, 오른쪽에 없는 데이터는 NULL로 표시.
3. **RIGHT OUTER JOIN**: 오른쪽 테이블 기준, 왼쪽에 없는 데이터는 NULL로 표시.
4. **FULL OUTER JOIN**: 양쪽 테이블의 모든 데이터를 출력, NULL 포함.
5. **CROSS JOIN**: 두 테이블의 모든 조합(카테시안 곱).
6. **SELF JOIN**: 하나의 테이블을 두 번 참조하여 JOIN.

---

## **집계 함수와 NULL 처리**

1. **COUNT**:
   - `COUNT(*)`: NULL 포함 행의 개수.
   - `COUNT(열)`: 열 값이 NULL이 아닌 행만 개수.
2. **SUM / AVG**: NULL은 연산에서 제외.
3. **NVL**: NULL 값 치환.
   - 예: `NVL(열, 0)` → NULL 값을 0으로 치환.

---

## **서브쿼리**

1. **단일 행 서브쿼리**:
   - 하나의 값만 반환 (e.g., `=`).
2. **다중 행 서브쿼리**:
   - 여러 값 반환 (e.g., `IN`, `ANY`, `ALL`).
3. **스칼라 서브쿼리**:
   - SELECT 절에서 하나의 값 반환.

---

## **PIVOT / UNPIVOT**

### PIVOT

- 행 데이터를 열로 변환.

```sql
SELECT *
FROM EMP
PIVOT (
  COUNT(EMPNO) FOR DEPTNO IN (10, 20, 30)
);
```

### UNPIVOT

- 열 데이터를 행으로 변환.

```sql
SELECT *
FROM EMP
UNPIVOT (
  VALUE FOR DEPT IN (DEPT10, DEPT20, DEPT30)
);
```

---

## **날짜 함수**

1. **SYSDATE**: 현재 날짜와 시간 반환.
2. **ADD_MONTHS**: 지정한 월 수 더하기.
   ```sql
   SELECT ADD_MONTHS(SYSDATE, 3) FROM DUAL;
   ```
3. **TRUNC**: 날짜 특정 단위로 버림.
   ```sql
   SELECT TRUNC(SYSDATE, 'MONTH') FROM DUAL;
   ```
4. **ROUND**: 날짜 반올림.
   ```sql
   SELECT ROUND(SYSDATE, 'MONTH') FROM DUAL;
   ```

---

## **NULL 처리 함수**

1. **NVL(값, 대체값)**: NULL을 대체값으로 치환.
2. **NVL2(값, 값1, 값2)**: 값이 NULL이 아니면 값1, NULL이면 값2 반환.
3. **COALESCE(값1, 값2, ...)**: NULL이 아닌 첫 번째 값을 반환.

---
