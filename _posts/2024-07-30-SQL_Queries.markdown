---
title: "{ SQL } Basic SQL Queries"
date: 2024-07-3O 03:00:00 KST
tags: [Oracle, SQL, Queries]
---

# SQL 노트 정리

## 기본 SQL 쿼리

### 전체 데이터 조회

```sql
SELECT * FROM employees;
```

### 데이터 개수 조회

```sql
SELECT COUNT(*) FROM employees;
```

### 특정 열 데이터 조회

```sql
SELECT employee_id, first_name, last_name FROM employees;
```

## 문자열 결합 및 간단한 연산

### 열 데이터 결합

```sql
SELECT first_name || ' ' || last_name FROM employees;
```

### 열 데이터에 연산 적용

```sql
SELECT salary * 4 FROM employees;
```

### 열 또는 테이블에 별칭 부여

```sql
SELECT first_name || ' ' || last_name AS full_name FROM employees;
```

별칭 생략 가능:

```sql
SELECT first_name || ' ' || last_name full_name FROM employees;
```

### 문자열 결합과 텍스트 추가

```sql
SELECT first_name || ' ' || last_name || '''s salary is ' || salary AS salary_info FROM employees;
```

## 조건을 이용한 데이터 조회

### 특정 조건을 만족하는 데이터 조회

```sql
SELECT * FROM employees
WHERE salary > 5000
AND (department_id = 50 OR department_id = 80);
```

### `IN` 연산자를 이용한 조건 조회

```sql
SELECT * FROM employees
WHERE salary > 5000
AND department_id IN (50, 80, 90);
```

### 부정 조건 조회

```sql
SELECT * FROM employees
WHERE NOT salary > 5000
AND NOT department_id IN (50, 80, 90);
```

### 특정 패턴의 데이터를 조회 (`LIKE` 연산자)

```sql
SELECT * FROM employees
WHERE phone_number LIKE '%1234%'; -- 1234가 포함된 경우
```

### 특정 문자열로 시작하는 데이터 조회

```sql
SELECT * FROM employees
WHERE phone_number LIKE '1234%';
```

### 특정 문자열로 끝나는 데이터 조회

```sql
SELECT * FROM employees
WHERE phone_number LIKE '%1234';
```

## Quiz 1

1. **입사일 기준 2002년 이상 입사한 사원 정보 조회**

   ```sql
   SELECT * FROM employees
   WHERE hire_date > '01/01/02';
   ```

2. **입사일 기준 5월에 입사한 사원의 사원번호, 이름, 성, 입사일 조회**

   ```sql
   SELECT employee_id, first_name, last_name, hire_date
   FROM employees
   WHERE hire_date LIKE '%/05/%';
   ```

3. **부서번호가 30, 50, 80, 90이고 급여가 10000 이상인 사원 조회**
   ```sql
   SELECT * FROM employees
   WHERE department_id IN (30, 50, 80, 90)
   AND salary >= 10000;
   ```

## 서브쿼리

### 서브쿼리 사용 예시

**사원 ID가 100인 사람보다 입사일이 빠른 사원의 모든 정보 조회**

```sql
SELECT * FROM employees
WHERE hire_date <
(SELECT hire_date FROM employees WHERE employee_id = 100);
```

## Quiz 2

1. **사장(사원번호 100번)과 부서번호가 같은 사원들 중 급여가 5000 이상인 사원 정보 조회**

   ```sql
   SELECT * FROM employees
   WHERE salary >= 5000
   AND department_id =
   (SELECT department_id FROM employees WHERE employee_id = 100);
   ```

2. **급여가 5000 이상인 사원 조회 - FROM 절에 서브쿼리 사용**

   ```sql
   SELECT * FROM (SELECT * FROM employees WHERE salary >= 5000);
   ```

3. **이름이 Steven인 사람과 직무가 같은 사원 정보 조회**

   ```sql
   SELECT * FROM employees
   WHERE job_id =
   (SELECT job_id FROM employees WHERE first_name = 'Steven');
   ```

   또는

   ```sql
   SELECT * FROM employees
   WHERE job_id IN
   (SELECT job_id FROM employees WHERE first_name = 'Steven');
   ```

4. **이름이 Nancy인 사원보다 급여를 많이 받는 사원 조회**

   ```sql
   SELECT * FROM employees
   WHERE salary >
   (SELECT salary FROM employees WHERE first_name = 'Nancy');
   ```

5. **사장보다 입사일이 빠른 사람들과 직무가 같은 사원들 조회**
   ```sql
   SELECT * FROM employees
   WHERE job_id IN
   (SELECT job_id FROM employees
    WHERE hire_date <
    (SELECT hire_date FROM employees WHERE employee_id = 100));
   ```
