---
tags:
  - DB
  - Lock
---
여러 트랜잭션들이 동시에 같은 데이터에 접근하는 것을 DB LOCK 이라고 한다.

## 왜 DB LOCK이 있어야하는가?
DB LOCK을 하지않는다면 한개의 데이터에 여러 트랜잭션이 Write를 하는 경우 결과가 이상하게 출력 될 수 있습니다.
따라서, 데이터의 일관성과 무결성을 위해 DB LOCK이 필요합니다.

## Lock 종류
Lock은 2개의 종류가 있습니다.

- Read-Lock
- Write-Lock
