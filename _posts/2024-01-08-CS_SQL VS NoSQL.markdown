---
title: "{ CS Concepts } SQL vs NoSQL"
date: 2024-01-08 16:00:00 +07:00
tags: [Database, SQL, NoSQL]
description: Notes of the difference between SQL and NoSQL
---

## SQL vs NoSQL Databases

---

### SQL (Structured Query Language) - Relational Databases

- **Data Structure**: Utilizes structured, tabular formats with predefined schemas (tables, columns, rows); relationships are defined using primary and foreign keys.
- **Scalability**: Primarily suited for vertical scaling (enhancing a single server's power).
- **Schema Flexibility**: Requires a predefined schema; schema changes can be complex and disruptive.
- **Use Cases**: Ideal for applications that require complex queries, transactional integrity, and a consistent data structure.

### NoSQL Databases - Non-Relational Databases

- **Data Structure**: Features a variety of structures such as document-oriented, key-value, wide-column stores, or graphs; handles unstructured or semi-structured data.
- **Scalability**: Designed for horizontal scaling (adding more servers); efficiently handles large data volumes and high traffic.
- **Schema Flexibility**: Offers schema-less or flexible schemas; enables easier data model modifications.
- **Use Cases**: Suitable for rapidly changing data formats, large-scale applications, and handling diverse or large volumes of data.

### Key Differences

- **Structure and Schema**: SQL employs structured, rigid schemas, while NoSQL offers flexibility with varied data structures.
- **Scalability**: SQL is typically more vertically scalable, whereas NoSQL is tailored for horizontal scalability.
- **Flexibility**: SQL necessitates a predefined schema, whereas NoSQL is more adaptable to changes in data models.
- **Applicability**: SQL is optimal for complex query processing and ensuring data integrity; NoSQL excels in scalability and managing diverse data types.

### Review (Korean)

- **구조의 차이**: SQL은 structured, tabular format과 predefined된 schemas를 사용하며, foreign key와 primary key를 통해 테이블 간 관계가 정의됩니다. 반면, NoSQL은 json, graph 같은 다양한 데이터 포맷을 사용하며, 데이터는 독립적으로 저장됩니다.
- **작동 방식의 차이**: SQL을 사용하여 데이터 쿼리, 추가, 삭제, 업데이트, 검색이 수행됩니다. NoSQL은 key-value pair, document-oriented (JSON, XML), graph 등 다양한 형식으로 데이터를 저장하고, 유연한 API나 쿼리 언어를 사용합니다. 예를 들어, MongoDB는 document-oriented 쿼리 언어를 사용합니다.

### 비교하였을 때 SQL의 장점

- Complex queries에 적합합니다 (예: Join을 사용한 효율적인 데이터 검색 가능).
- 모든 데이터가 동일한 형식과 규칙을 준수하도록 함으로써, transactional integrity (ACID)와 일관된 데이터 구조 (예: 날짜 YYYY-MM-DD) 유지에 적합합니다. (See 'ACID properties')

### NoSQL의 장점

- Unstructured large data volumes을 유연하게 다루며, 데이터 확장 시 빠른 처리가 가능합니다.
- 데이터 변경이나 다양한 데이터 형식의 조작에 유연합니다.

---
