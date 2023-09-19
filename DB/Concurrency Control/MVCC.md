---
tags: MVCC, concurrency_control
---
MVCC(Multiversion Concurrency Control) 스냅샷을 통해 하나의 데이터에 여러 버전이 관리되는 것을 뜻한다.
여러 버전을 관리하게 됨으로써 여러 트랜잭션이 한 데이터를 동시적으로 작업할 때 무결성과 