---
tags:
  - MVCC
---
PostgreSQL MVCC

first-updater-win

모두 Read Committed 일 때 Lost Update 이상현상이 발생할 수 있다.
이 문제를 연관 있는 트랜잭션의 Isolation level을 Repeatable Read로 변경하는 것으로 해결 가능하다.
이 Repeatable Read에서 여러 트랜잭션이 같은 데이터를 update한 뒤 commit되게 된다면 나중 트랜잭션은 rollback 되게 된다.


