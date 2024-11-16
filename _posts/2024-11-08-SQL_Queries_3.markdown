---
title: "{ SQL } Queries_해석"
date: 2024-11-08 03:00:00 KST
tags: [SQL]
---

## 해석 순서

1. 쿼리 목적 파악 (SELECT): 쿼리가 궁극적으로 어떤 데이터를 얻고자 하는지 확인
   select 뒤에 위치한 어떤 열과 집계함수가 있는지 대략적으로 파악
   예를들어 sum,count,avg 같은 집계 함수가 있으면 데이터를 요약하거나 집계하는 목적일 가능성이 크다

2. 주요 테이블과 열 확인(FROM): 각 테이블과 열의 역할을 파악
   from 절에서 사용하는 테이블 이름을 보고 데이터의 주요 주제를 추정가능
   예를들어 CUSTOMERS, ORDERS, SALES 같은 테이블이 포함되어 있다 => 이 쿼리는 고객의 구매나 판매와 관련된 데이터이구나 유추 가능

3. 조건문 분석(WHERE): 데이터를 필터링하는 조건을 해석
   WHERE 절에서 어떤 조건으로 필터링을 하는지 추청
   예를들어 SALE_DATA BETWEEN ~ AND ~
   ~부터 ~까지의 SALS_DATA 를 구하려는 거구나

4. 연결 테이블 분석(JOIN): 테이블 간의 관계를 확인해서 데이터를 연결
   테이블 간의 관계를 파악 가능
   쿼리가 데이터를 어떤 방식으로 연결해 보여주려고 하는지 확인 가능

5. 그룹화 및 집계(GROUP BY, HAVING) 분석: 데이터 그룹화와 필터링을 이해
   GROUP BY 절을 통해 데이터가 어떻게 요약되는지 파악 => 쿼리의 주제를 잘 나타낸다
   예를들어 GROUP BY CUSTOMER_ID 가 있으면 쿼리가 각 고객별로 데이터를 요약하고 있을 가능성이 있음

6. 정렬 및 제한(ORDER BY, LIMIT) 확인: 결과 정렬 및 출력 수 제한
   ORDER BY: 쿼리가 어떠한 결과를 정렬하려 하는지
   LIMIT: 출력 수 제한

## 예시 1.

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

1. 쿼리 목적 파악 (SELECT): 고객명과 총금액을 얻으려 함
2. 주요테이블과 열 확인 (FROM): 고객 테이블에서 고객명과 총금액
3. 조건문 분석 (WHERE): 한국에 살고 주문날짜가 2023-01-01 보다 크거나 같은
4. 연결 테이블 분석 (JOIN): 주문 테이블을 연결하려하고 기준은 CUSTOMER_ID 열을 기준으로 해서
   INNER JOIN 이기 때문에 두 테이블에 모두 존재하는 CUSTOMER_ID 값만 선택
   => 주문 내역만 있는 고객들의 주문 총 금액의 결과를 가져오려고 하는거구나
5. 그룹화 및 집계 (GROUP BY, HAVING): 고객 이름별로 주문 데이터가 그룹화되고 총금액이 5000를 초과하는 고객만 필터링
   => 총 주문금액이 5000을 초과하는 고객의 결과
6. 정렬 및 제한 (ORDER BY, LIMIT): 총 금액을 기준으로 내림차순으로 상위 10명 고객만 표시

#### 최종 해석: 한국에 거주하고 2023-01-01 이후 주문한 고객 들중에 주문 금액이 5000을 초과하는 상위 10명 고객을 조회

## 예시 2.

```sql
SELECT E.EMPLOYEE_NAME, E.DEPARTMENT_ID, S.SALE_AMOUNT,
       RANK() OVER(PARTITION BY E.DEPARTMENT_ID ORDER BY S.SALE_AMOUNT DESC) AS SALE_RANK
FROM EMPLOYEES E
INNER JOIN SALES S ON E.EMPLOYEE_ID = S.EMPLOYEE_ID
WHERE S.SALE_DATE BETWEEN '2023-01-01' AND '2023-12-31';
```

### AND, OR, NOT, () 조합 쿼리 해석

- AND = && (두가지 조건 모두)
- OR = || (두가지 중 하나라도 = 각각)
- NOT = ! (조건이 아닌)

- 우선순위: NOT -> AND -> OR
- () 괄호를 사용하여 조작 가능

1. 괄호를 우선적으로 계산합니다.
2. 그 다음 NOT 조건을 처리합니다.
3. AND 조건을 평가합니다.
4. 마지막으로 OR 조건을 평가합니다.

## 예시 1.

```sql
SELECT * FROM employees
WHERE (department = 'Sales' OR department = 'Marketing')
  AND salary > 5000
  AND NOT (job_title = 'Intern' OR job_title = 'Contractor');
```

1. 부서가 세일즈거나 마케팅인 직원이고
2. 연봉이 5000 보다 크고
3. 직업이 인턴이거나 건설자인 => NOT => 둘 다 아닌 직원

- 부서가 세일즈 또는 마케팅인 직원이고 5000 이상이고 직업이 인턴또는 건설자가 아닌 직원들을 조회한다.

## 예시 2.

```sql
SELECT *
FROM EMP
WHERE (DEPTNO = 10 AND JOB = 'CLERK')
   OR (DEPTNO = 20 AND JOB = 'CLERK')
   OR SAL > 3000;
```

1. 부서번호가 10이고 CLERK 인 직원 또는
2. 부서번호가 20이고 CLERK 인 직원 또는
3. 연봉이 3000보다 큰 직원

- 위 3가지 조건중 하나라도 만족하는 직원들을 조회한다.

```sql
SELECT *
FROM EMP
WHERE (DEPTNO = 10 OR DEPTNO = 20)
AND JOB = 'CLERK' OR SAL > 3000;
```

1. 부서번호가 10이거나 20이고
2. 직원이 CLERK이고
3. 연봉이 3000보다 큰 직원

** 괄호 ** 에 따라서 결과가 크게 달라질 수 있다는 점

### GROUP BY 절

- 각 행을 특정 조건에 따라 그룹을 분리함
- GROUP BY 뒤에 컬럼을 여러 개 전달 가능
- 컬럼 순서들은 출력에 영향을 주지 않음
- 절 뒤에 컬럼 별칭을 사용할 수 없음
- GROUP BY 뒤의 첫번째 값이 UNIQUE 한 경우 뒤에 어떤 컬럼을 추가해도 그룹 수는 변화 없다
- GROUP BY에 사용하지 않은 컬럼은 집계함수와 함께 SELECT 절에 사용 가능함

### GROUPING SETS(A,B,...): A, B 별

- A별, B별 그룹 연산 결과 출력 - 보이는 것만 출력
- 나열 순서는 중요하지 않다.
- 기본 출력에 전체 총계는 출력되지 않는다.
- NULL 혹은 () 사용해서 전체 총계 출력 가능

예제)

```sql
SELECT DEPTNO, JOB, SUM(SAL)
FROM EMP
GROUP BY GROUPING SETS(DEPTNO, JOB, ());
```

### ROLLUP(A,B): A , (A,B) , 전체총계

- A별,(A,B)별, 전체 그룹 별(맨 마지막) 순서로 연산 결과 출력
- 나열 대상의 순서 중요
- 전체 총 계가 출력됨

### CUBE(A,B): A, B, (A,B), 전체총계

- A별, B별,(A,B)별, 전체 그룹 연산 결과 출력
- 그룹으로 묶을 대상의 나열 순서 중요하지 않음
- 전체 총 계가 출력됨

### ORDER BY 절

- ORDER BY 절을 사용하지 않으면 데이터는 입력된 순서대로 출력
- 기본 정렬 순서는 오름차순(ASC) 이다
- ORDER BY 절에는 컬럼 별칭을 사용할 수 있다 (유일하게 사용 할 수 있는 구문)
- ORDER BY 절에는 컬럼명과 숫자를 동시에 사용할 수 있다
  (숫자를 사용하게 되면 SELECT 절에 명시한 컬럼의 순번이 된다)

### INNER JOIN: JOIN 조건에 성립하는 데이터만 출력

### OUTER JOIN: JOIN 조건에 성립하지 않는 데이터도 출력

- LEFT / RIGHT / FULL OUTER JOIN 으로 나뉜다
- LEFT: FROM 절에 나열된 왼쪽 테이블에 해당하는 데이터를 읽은 후 우측 테이블에서 JOIN 대상을 읽어온다.

  - '왼쪽 테이블이 기준' 이 되어 오른쪽 테이블 데이터를 채우는 방식
  - 우측 값에서 같은 값이 없는 경우 NULL 값으로 출력

```sql
   SELECT *
      FROM STUDENT S, PROFESSOR P
   WHERE S.PORFNO = P.PROFNO(+)
   AND   S.GRADE IN (1,4);
   -- ORACLE 에서는 LEFT OUTER JOIN 을 기술하지 않고 WHERE 절에서 기준이 되는 테이블 (STUDENT) 반대 테이블 조건 뒤에 (+)를 붙힌다

   -- 결과: 학점이 1,4 인 학생 중에 지도교수가 없는 학생 정보도 출력
```

- RIGHT: LEFT JOIN 과 반대
- FULL: 두 테이블 전체 기준으로 결과 생성해서 중복 데이터는 삭제 후 리턴
  - LEFT OUTER JOIN + UNION + RIGHT OUTER JOIN

### 그 외 JOIN

- NATURAL: (조인 조건 생략 시) 두 테이블에 같은 이름으로 '자연 연결' 되는 조인 (컬럼명이 같은 컬럼끼리 값이 같은 조인을 완성)

  - 왼쪽 테이블 기준 오른쪽 테이블의 중복 값을 출력에 포함한다
  - NULL 은 값이 같다고 볼 수 없다

- CROSS: (조인 조건 생략 시) 두 테이블의 발생 가능한 모든 행을 출력 (카타시안곱)
- SELF: 하나의 테이블을 두번 이상 참조하여 연결하는 조인

### 집계함수 NULL 처리방법

- COUNT(\*): 컬럼 모두 지정할 경우 행의 값 모두가 NULL 이 아니면 무조건 1건이 출력
- SUM(COL1+COL2_COL3): 각 행에 하나라도 null 이 있으면 null 이 반환됨

### 집합 연산자 종류

- 일반 집합 연산자: UNION, INTERSECTION, DIFFERENCE, PRODUCT(CROSS JOIN)

- 순수 관계 연산자: SELECT, DIVISION(현재 사용x), JOIN, PROJECT

### 서브쿼리

- 다중행 서브쿼리는 동등비교, 대소비교 연산자를 사용할 수 없고 IN, ANY, ALL 이 대체한다.
- 스칼라 서브쿼리는 각 행마다 하나씩 매칭이 되어야 한다.
- 다중컬럼 서브쿼리의 SELECT 절에 사용된 컬럼은 메인쿼리 SELECT 절에 들어갈 수 없다.

```sql
   -- 불가능 한 쿼리
   SELECT (A,B) FROM 테이블1 WHERE (A,B) IN (SELECT A,B FROM 테이블2)
```

### 집합 연산자

- 집합 연산자

  - SELECT 문 결과를 하나의 집합으로 간주, 그 집합에 대한 합집합, 교집합, 차집합 연산

- 유의사항

  - 두 집합의 컬럼 수, 순서, 데이터 타입 일치
  - 컬럼의 사이즈는 달라도 됨
  - 개별 SELECT 문에 ORDER BY 사용 불가
  - GROUP BY 는 전달 가능 (전체 결과에 대해선 가능)

- 종류
  - 합집합 (UNION, UNION ALL)
    - UNION: 중복된 데이터 한번만 출력
    - 제거하기 위해 내부적으로 정렬 자동 수행
    - 없을 경우는 UNION ALL 사용 권고 (불필요한 정렬을 피하기 위해)
  - 교집합 (INTERSECT)
    - 두 집합 사이에 교집합(공통으로 있는 행) 출력
  - 차집합 (MINUS)
    - 두 집합 사이에 MINUS
    - 한 쪽 집합에만 존재 하는 행 출력
    - A-B, B-C 는 다르므로 순서 주의

### PARTITION BY

- 출력할 총 데이터 수 변화 없이 그룹연산 수행할 GROUP BY 컬럼
- RANGE가 기본옵션 (동순위 인정, 순위 같은 값 묶음)
- ORDER BY, RANK 의 경우 필수

A 10
A 20
A 20
B 30
B 40
B 40

```sql
SELECT SUM(COL2)
OVER(PARTITION BY COL1 ORDER BY COL2 ASC)
AS RESULT FROM TAB1;
-- A,B 로 묶어서 COL2 오름차 정렬로 한 것을 더해서 출력
```

10
50 (순위 같은 값 미리 더해서 묶음)
50
30
110
110

- ROWS, RANGE 차이
  - ROWS: 값이 같더라도 각 행씩 연산
  - RANGE: 같은 값의 경우 하나의 RANGE 로 묶어서 동시 연산

### TOP N 쿼리

- 페이징 처리를 효과적으로 수행하기 위해 사용
- WITH TIES 를 사용해서 동순위까지 함께 출력 가능
- 전체 결과에서 '특정 N개' 추출 후에 동순위가 더 있으면 N개가 있더라고 4,5개가 출력 될 수 있다.

### 계층형 질의

- 행끼리 연결관계가 있을 경우 연결관계를 표현하는 기법
- 행의 연결관계는 CONNECT BY 에 의해 전달
- LEVEL은 항상 가장 먼저 시작하는 행이 1을 갖음 (1부터 순차적으로 증가)
- 같은 레벨일 경우 추가 정렬을 원한다면 ORDER SIBLINGS BY 를 사용

### PIVOT과 UNPIVOT: 데이터 구조 변경 기능

PIVOT과 UNPIVOT은 데이터를 변환하여 테이블의 구조를 바꾸는 데 사용됩니다. SQL에서는 주로 **행 데이터를 열로 변환(PIVOT)**하거나, **열 데이터를 행으로 변환(UNPIVOT)**하는 데 활용됩니다.

---

### 1. **PIVOT**

- 데이터를 **"행"에서 "열"로 변환**하여 교차표(피벗 테이블)를 생성하는 기능.
- 주로 집계 함수(`COUNT`, `SUM`, `AVG` 등)와 함께 사용.
- **사용 목적**: 값이나 속성을 기준으로 그룹화된 요약 데이터를 열 형태로 표시.

#### 예제 설명

```sql
SELECT *
FROM
PIVOT(
    COUNT(MONTH)         -- 집계 함수 (COUNT) 사용
    FOR WEEKDAYS         -- "WEEKDAYS" 컬럼을 피벗의 기준으로 사용
    IN (월, 화, 수, 목, 금) -- 피벗의 열 값 정의
);
```

- **COUNT(MONTH)**: `MONTH` 컬럼의 값을 기준으로 각 요일(`WEEKDAYS`)에 대해 **값의 개수**를 계산.
- **FOR WEEKDAYS IN (월, 화, 수, 목, 금)**:
  - `WEEKDAYS`의 값이 `월, 화, 수, 목, 금`일 때를 기준으로 열을 만듦.
  - 이 컬럼들은 결과 테이블의 **열 이름**으로 변환됨.

#### 결과 예시

| MONTH | 월  | 화  | 수  | 목  | 금  |
| ----- | --- | --- | --- | --- | --- |
| 01    | 10  | 20  | 30  | 40  | 50  |
| 02    | 39  | 45  | 45  | 20  | 21  |

---

### 2. **UNPIVOT**

- 데이터를 **"열"에서 "행"으로 변환**하여 테이블을 다시 펼치는 기능입니다.
- **사용 목적**: 열 형태로 요약된 데이터를 다시 행으로 변환하여 분석 가능성을 높임.

#### 예제 설명 (UNSTACK)

다음과 같은 구조를 UNPIVOT(스택)으로 변환한다고 가정합니다.

**기존 데이터 (열 구조)**:
| MONTH | 월 | 화 | 수 | 목 | 금 |
|-------|-----|-----|-----|-----|-----|
| 01 | 10 | 20 | 30 | 40 | 50 |
| 02 | 39 | 45 | 45 | 20 | 21 |

**UNPIVOT 사용 결과 (행 구조)**:

```sql
SELECT *
FROM
UNPIVOT(
    VALUE FOR WEEKDAYS IN (월, 화, 수, 목, 금)
);
```

결과:
| MONTH | WEEKDAYS | VALUE |
|-------|----------|-------|
| 01 | 월 | 10 |
| 01 | 화 | 20 |
| 01 | 수 | 30 |
| 01 | 목 | 40 |
| 01 | 금 | 50 |
| 02 | 월 | 39 |
| 02 | 화 | 45 |
| 02 | 수 | 45 |
| 02 | 목 | 20 |
| 02 | 금 | 21 |

---

---

**서브쿼리내 SELECT 뒤 선택 컬럼은 COUNT와 FOR 안에 있는 컬럼 이외로 들어 갈 수 없다**

```sql
SELECT * FROM
(SELECT EMPNO, DEPTNO, FROM EMP)
PIVOT(COUNT(EMPNO) FOR DEPTNO IN (10,20,30));
```

```sql
--- 잘못된 쿼리 예시
SELECT * FROM
(SELECT EMPNO, DEPTNO, ENAME FROM EMP)
PIVOT(COUNT(EMPNO) FOR DEPTNO IN (10,20,30));

--- ENAME 은 들어 갈 수 없음
```

### 날짜 연산 종류

- SYSDATE: 현재 날짜와 시간 리턴
- ADD_MONTHS: 날짜에서 n개월 수 리턴
  예시) SELECT ADD_MONTH(SYSDATE,3)
- TO_DATE: 날짜로 변환
  ... 등등

예시) SELECT ADD_MONTHS(TO_DATE('2024/08/24 10:00:00', 'YYYY/MM/DD HH24:MI:SS') - 10, -3) + 10/24/60 FROM DUAL;

주어진 SQL 쿼리와 날짜 연산을 통해 **2024년 8월 24일 10시**에서 -10일과 -3개월을 빼고 **`10 / 24 / 60`**을 추가적으로 빼는 연산을 설명해 보겠습니다.

```sql
SELECT ADD_MONTHS(TO_DATE('2024/08/24 10:00:00', 'YYYY/MM/DD HH24:MI:SS') - 10, -3) + 10/24/60 FROM DUAL;
```

### 쿼리 해석

1. **-10일 연산**:

   ```sql
   TO_DATE('2024/08/24 10:00:00', 'YYYY/MM/DD HH24:MI:SS') - 10
   ```

   - 이 연산은 2024년 8월 24일 10시에서 10일을 뺍니다.
   - 결과: **2024년 8월 14일 10시**

2. **-3개월 연산**:

   ```sql
   ADD_MONTHS(..., -3)
   ```

   - 결과: **2024년 5월 14일 10시**

3. **`10 / 24 / 60` 추가 연산**:
   ```sql
   + 10 / 24 / 60
   ```
   - `10 / 24 / 60`은 시간을 나타내는 표현으로, 이는 **10분**을 의미합니다.

### 날짜 함수

1. TRUNC(날짜, 자리수): 날짜 특정 자리에서 버림
   e.g. TRUNC(SYSDATE, 'MONTH'): MONTH 이후부터 버림

```sql
SELECT TRUNC(TO_DATE('2024/05/14', 'YYYY/MM/DD'), 'MONTH') FROM DUAL;
-- 결과: 2024/05/01
```

2. ROUND: 지정된 단위로 반올림 (15일까지 버림 / 16일 부터 반올림)

```sql
SELECT ROUND(TO_DATE('2024/08/15', 'YYYY/MM/DD'), 'MONTH') FROM DUAL;
-- 결과: 2024/08/01

SELECT ROUND(TO_DATE('2024/08/16', 'YYYY/MM/DD'), 'MONTH') FROM DUAL;
-- 결과: 2024/09/01
```

4. LAST_DAY 함수는 주어진 날짜가 속한 월의 마지막 날을 반환

```sql
LAST_DAY(TO_DATE('2024/05/14', 'YYYY/MM/DD')) FROM DUAL;
-- 결과: 2024/05/31
```

### NULL 치환함수

- NVL,ISNULL(대상,치환값): 대상이 NULL 이면 치환값으로 치환
- NVL2(대상,치환값1,치환값2): 대상이 NULL 이면 치환값2 아니면 치환값 1로 치환
- COALESCE(대상1,대상2,..., 그외리턴): 대상들 중 NULL 이 아닌 값 출력(첫번째부터), 모두가 널이면
  그외리턴값이 리턴
  e.g. COALESCE(DEPTNO1,DEPTNO2,0)
  => DEPTNO1과 DEPTNO중 널이 아닌 값 출력, 둘 다 널이면 0 출력, 둘다 널이 아니면 맨앞 DEPTNO1 출력
