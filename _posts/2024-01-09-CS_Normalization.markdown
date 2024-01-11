---
title: "{ CS Concepts } Normalization"
date: 2024-01-10 18:00:00 +07:00
tags: [Normalization, Database]
description: Notes of Normalization
---

## Normalization is an essential process in database design
---

### Purpose 
- to ensure efficient, consistent, and reliable data structures.
- It involves organizing data in a database into tables to minimize redundancy and dependency.

1. **First Normal Form (1NF)**:
   - **Objective**: Ensure that each column in a table is atomic, meaning each field holds only a single value.
   - **Details**: Tables should not contain repeating groups or arrays. Each record needs to be unique, often enforced through a primary key.
   - **Example**: If a database has a table with a column containing multiple values (like a list of phone numbers), it should be modified so that each row has only one phone number.

2. **Second Normal Form (2NF)**:
   - **Based on**: Fulfillment of 1NF requirements.
   - **Objective**: Remove partial dependencies; that is, no non-prime attribute should be dependent on any proper subset of any candidate key of the table.
   - **Details**: Often involves separating data into additional tables and linking them with foreign keys.
   - **Example**: If a table has data where some columns are only dependent on a part of the primary key (in case of composite primary keys), those columns should be moved to a separate table.

3. **Third Normal Form (3NF)**:
   - **Based on**: Fulfillment of 2NF requirements.
   - **Objective**: Remove transitive dependencies; no non-prime attribute should depend on other non-prime attributes.
   - **Details**: Ensures that each non-prime attribute is only dependent on prime attributes (the primary key).
   - **Example**: If a table contains columns that are not dependent directly on the primary key but on another column, this relationship should be moved to a separate table.

4. **Boyce-Codd Normal Form (BCNF)**:
   - **Based on**: Similar to 3NF but with a stronger condition.
   - **Objective**: Address anomalies not handled by 3NF. Every determinant must be a candidate key.
   - **Details**: Sometimes, a table that is in 3NF still has redundancy or modification anomalies. BCNF addresses these issues.
   - **Example**: In cases where a table has multiple candidate keys that are composite and overlapping, BCNF helps in further refining the table structure.

Normalization, through these stages, helps in structuring databases in a way that reduces redundancy, improves data integrity, and enhances database performance, especially in relational databases. Each stage builds upon the previous one, progressively making the database more efficient and logically organized.


### Review (Korean)
- 데이터베이스 정규화는 관계형 데이터베이스 설계에서 중요한 과정
- 데이터를 중복을 줄이고 의존성을 최소화하는 방식으로 구성함으로써 데이터베이스의 무결성과 효율성을 향상

- 1NF: 테이블의 각 열이 고유하고 원자적인 값을 가짐, 각각의 record 의 고유성을 보장
- 2NF: 1NF 를 기본으로, primary key 에 대한 부분 종속성을 제거하는 것, 즉 모든 non-key attributes 가 전체 primary key 에 fully dependent 하게 하는 것을 의미한다. 예를 들어 하나의 테이블을 두개 이상의 테이블로 분할하여 각각의 데이터가 부분이 아닌 전체 키에 의존하게 만드는 것
- 3NF: 2NF 를 기본으로, transitive(전이적) 종속성을 제거하는 것, 즉 키가 아닌 모든 속성들은 primary keys 에만 의존하고 키가 아닌 attributes 에게 종속되게 하지 않는 것


- BCNF: 3NF의 약간의 개선 form.. 테이블에 여러 후보 키가 있고 이들이 서로 중첩되는 경우

| CourseID | ProfessorName | Department |
|----------|---------------|------------|

여기서는 'CourseID'와 'ProfessorName'이 함께 기본 키를 형성합니다. 동일한 과목을 다른 교수가 가르칠 수 있고 교수가 여러 과목을 가르칠 수 있기 때문입니다. 그러나 Department 는 교수 이름에 의해서만 결정된다고 가정해 보면 BCNF 를 충족하지 않음. ProfessorName 가 Department 를 결정하기 때문에

1. **Courses Table**: Contains `CourseID` and `ProfessorName`.
2. **Professors Table**: Contains `ProfessorName` and `Department`.

2 개로 나눔 

여기서 ProfessorName 은 Professors 테이블의 후보키로 Department를 결정한다. 이 구조는 변칙을 해결하고 BCNF를 준수

### 각 정규화 단계는 특정 유형의 중복과 의존성을 해결하며, 이러한 문제들이 방치되면 다양한 데이터 무결성 문제를 야기할 수 있습니다. 정규화는 데이터베이스 설계를 간소화하고 쿼리에 대한 효율성을 높이며 데이터 일관성을 보장하는 데 도움이 됩니다.

---