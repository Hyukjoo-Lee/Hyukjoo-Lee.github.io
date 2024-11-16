아래는 제공된 SQL 기본 쿼리와 관련 내용을 정리한 요약본입니다.

---

## **SQL 기본 쿼리**

### **전체 데이터 조회**

```sql
SELECT * FROM employees;
```

### **데이터 개수 조회**

```sql
SELECT COUNT(*) FROM employees;
```

### **특정 열 데이터 조회**

```sql
SELECT employee_id, first_name, last_name FROM employees;
```

---

## **문자열 결합 및 연산**

### **열 데이터 결합**

```sql
SELECT first_name || ' ' || last_name FROM employees;
```

### **연산 적용**

```sql
SELECT salary * 4 FROM employees;
```

### **별칭 사용**

```sql
SELECT first_name || ' ' || last_name AS full_name FROM employees;
```

### **문자열 결합 + 텍스트 추가**

```sql
SELECT first_name || ' ' || last_name || '''s salary is ' || salary AS salary_info FROM employees;
```

---

## **조건을 이용한 조회**

### **특정 조건 조회**

```sql
SELECT * FROM employees
WHERE salary > 5000
AND (department_id = 50 OR department_id = 80);
```

### **`IN` 연산자를 사용한 조회**

```sql
SELECT * FROM employees
WHERE department_id IN (50, 80, 90);
```

### **부정 조건 조회**

```sql
SELECT * FROM employees
WHERE NOT salary > 5000
AND NOT department_id IN (50, 80, 90);
```

### **패턴 검색**

- **포함된 문자열**:
  ```sql
  SELECT * FROM employees WHERE phone_number LIKE '%1234%';
  ```
- **특정 문자열로 시작**:
  ```sql
  SELECT * FROM employees WHERE phone_number LIKE '1234%';
  ```
- **특정 문자열로 끝남**:
  ```sql
  SELECT * FROM employees WHERE phone_number LIKE '%1234';
  ```

---

## **서브쿼리**

### **서브쿼리 사용 예시**

```sql
SELECT * FROM employees
WHERE hire_date <
(SELECT hire_date FROM employees WHERE employee_id = 100);
```

---

## **Quiz 문제 정리**

### Quiz 1

1. **2002년 이후 입사한 사원**

   ```sql
   SELECT * FROM employees WHERE hire_date > '01/01/02';
   ```

2. **5월 입사자 정보**

   ```sql
   SELECT employee_id, first_name, last_name, hire_date
   FROM employees
   WHERE hire_date LIKE '%/05/%';
   ```

3. **부서 30, 50, 80, 90번, 급여 10000 이상인 사원**
   ```sql
   SELECT * FROM employees
   WHERE department_id IN (30, 50, 80, 90)
   AND salary >= 10000;
   ```

### Quiz 2

1. **사장(100번)과 같은 부서, 급여 5000 이상인 사원**

   ```sql
   SELECT * FROM employees
   WHERE salary >= 5000
   AND department_id =
   (SELECT department_id FROM employees WHERE employee_id = 100);
   ```

2. **FROM 절에 서브쿼리 사용 (급여 5000 이상)**

   ```sql
   SELECT * FROM (SELECT * FROM employees WHERE salary >= 5000);
   ```

3. **이름이 Steven인 사람과 같은 직무**

   ```sql
   SELECT * FROM employees
   WHERE job_id =
   (SELECT job_id FROM employees WHERE first_name = 'Steven');
   ```

4. **Nancy보다 급여가 많은 사원**

   ```sql
   SELECT * FROM employees
   WHERE salary >
   (SELECT salary FROM employees WHERE first_name = 'Nancy');
   ```

5. **사장보다 입사일이 빠른 사람들과 같은 직무**
   ```sql
   SELECT * FROM employees
   WHERE job_id IN
   (SELECT job_id FROM employees
    WHERE hire_date <
    (SELECT hire_date FROM employees WHERE employee_id = 100));
   ```

---

## **조건 및 키워드 정리**

- **조건 연산자**:

  - `=`: 값 일치.
  - `>`: 값보다 큼.
  - `<`: 값보다 작음.
  - `LIKE`: 패턴 매칭.
  - `IN`: 값 집합 포함 여부.

- **NULL 처리**:

  - NULL은 연산 시 무시.
  - `COUNT`: NULL 무시, 결과가 없으면 0 반환.

- **별칭 사용**:
  - `AS` 키워드 사용 또는 생략 가능.

---
