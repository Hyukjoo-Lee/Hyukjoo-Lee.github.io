---
title: "{ SQL } Queries_해석방법"
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

#### 최종 해석: 한국에 거주하고 2023-01-01 이후 주문한 고객 들중에 주문 금액이 5000을 초과하는 상위 10명 고객을 조ㅠㅠㅠㅠㅠㅠ회

## 예시 2.

```sql
SELECT E.EMPLOYEE_NAME, E.DEPARTMENT_ID, S.SALE_AMOUNT,
       RANK() OVER(PARTITION BY E.DEPARTMENT_ID ORDER BY S.SALE_AMOUNT DESC) AS SALE_RANK
FROM EMPLOYEES E
INNER JOIN SALES S ON E.EMPLOYEE_ID = S.EMPLOYEE_ID
WHERE S.SALE_DATE BETWEEN '2023-01-01' AND '2023-12-31';
```

### AND, OR, NOT, () 조합 쿼리 해석

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