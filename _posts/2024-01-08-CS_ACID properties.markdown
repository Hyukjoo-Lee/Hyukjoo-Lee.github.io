---
title: "{ CS Concepts } ACID properties"
date: 2024-01-08 16:30:00 +07:00
tags: [ACID, SQL, Database]
description: Notes of ACID properties
---

## ACID Properties in Database Transactions
---

### Purpose
- The ACID properties ensure that database transactions are processed in a reliable and safe manner.

### Components of ACID
1. **Atomicity**
   - Transactions are treated as a single unit.
   - They either complete in their entirety or not at all, ensuring no partial transactions.

2. **Consistency**
   - This property ensures that all database rules and constraints are adhered to during transactions.
   - It maintains data integrity throughout the transaction process.

3. **Isolation**
   - Transactions are executed independently from one another.
   - This prevents interference and conflicts from concurrent transactions.

4. **Durability**
   - Once a transaction is committed, it is permanent, even in the event of a system failure.
   - This guarantees the persistence of transaction results.

### Transactions Involving Multiple Databases
- Transactions can involve operations across two or more databases, treating them as a single cohesive database.
- This approach requires careful management to ensure that the ACID properties are maintained across all involved databases.

### Review (Korean)
- ACID 속성은 데이터베이스 트랜잭션이 안전하고 신뢰할 수 있게 처리되도록 보장하는 규칙과 조건들이다.
- 여기서 데이터베이스 트랜잭션은 두 개 이상의 데이터베이스를 하나의 통합된 데이터베이스처럼 다루는 작업 단위를 의미한다.
- 이 때, 모든 관련된 데이터베이스에서 ACID 속성이 유지되도록 관리되어야 한다.

- 4가지 특성:
1. **A(Atomicity - 원자성)**: 트랜잭션은 하나의 단위로 취급되며, 모든 단계가 완벽하게 수행되거나 전혀 수행되지 않는다.
2. **C(Consistency - 일관성)**: 트랜잭션 중 모든 데이터베이스 규칙과 제약 조건이 지켜지며, 데이터의 일관성이 유지된다.
3. **I(Isolation - 독립성)**: 각 트랜잭션은 다른 트랜잭션과 독립적으로 실행되어, 서로 간섭하지 않는다. (See 'Transaction isolation levels')
4. **D(Durability - 영속성)**: 트랜잭션이 완료되고 확정되면, 시스템 오류가 발생해도 변경사항이 영구적으로 유지된다.

---
