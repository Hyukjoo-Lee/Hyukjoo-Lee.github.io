---
title: "{ MySQL } LEFT JOIN"
date: 2023-04-26 14:00:00 +07:00
tags: [MySQL, Left Join, PHP]
---

### LEFT JOIN

- It is used to combine data from two or more tables based on related column among them
- right 테이블과 match 되는 모든 rows 를 리턴 
- 일치 되는 데이터가 없는 경우 오른쪽 테이블 칼럼에 대해서 NULL 을 리턴

- 예시
- orders
  - order_id (primary key)
  - customer_id (foreign key)
  - order_date

- customers
  - customer_id (primary key)
  - customer_name
  - email

```sql
SELECT column1, column2, ... 
FROM table1
LEFT JOIN table2 ON table1.related_column = table2.related_column
[LEFT JOIN table3 ON ...] -- Optional: additional LEFT JOINs can be chained
[WHERE conditions] -- Optional: filter results based on certain conditions
[GROUP BY column(s)] -- Optional: group results by one or more columns
[HAVING conditions] -- Optional: filter groups based on certain conditions
[ORDER BY column(s)] -- Optional: order the results by one or more columns
[LIMIT number] -- Optional: limit the number of rows returned
;

SELECT: 검색을 원하는 컬럼들을 정의
FROM: LEFT table
LEFT JOIN: 관련 column 을 기준으로 LEFT 및 RIGHT 테이블의 데이터를 결합
ON: The condition to match rows between the left and right tables

// 특정 customenr 의 오더 정보 리스트를 얻고 싶음
SELECT orders.order_id, orders.order_date, customers.customer_name, customers.email
FROM orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id;


// All rows from the "orders" table.
// Corresponding customer data (customer_name and email) from the
// "customers" table when a matching customer_id is found.

Optional clauses:
WHERE: Filter the results based on certain conditions.
GROUP BY: Group the results by one or more columns.
HAVING: Filter groups based on certain conditions.
ORDER BY: Order the results by one or more columns.
LIMIT: Limit the number of rows returned.

```