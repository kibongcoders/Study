---
tags: 2PL Protocol
---
2PL Protocol(Two Phase Locking Protocol) 은 2개의 단계가 있는 프로토콜이다.
locking operation(Expanding phase)과 unlock operation(Shrinking phase)로 단계를 나누고 트랜잭션에서 모든 locking operation이 unlock operation보다 먼저 수행되도록 하는 것을 2PL Protocol이라고 부른다.

## 2PL Protocol 문제점

### DeadLock
DB에서 여러 트랜잭션이 상호적으로 다른 트랜잭션을 기다리는 상태에 빠져 어떠한 트랜잭션도 진행할 수 없는 상태를 DeadLock이라고 한다.

### 성능
2PL로 구현을 하면 문제점이 있다
Read-Lock과 Write-Lock을 통해서 구현하기 때문에

|  | Read-Lock | Write-Lock |
| --- | --- | --- |
| Read-Lock | O | X |
| Write-Lock | X | X |

해당 표와 같이 Read-Lock 에서 Read-Lock만 허용해 주기 때문에 전체적인 처리량이 좋지 않다.
Write-Lock -> Write-Lock은 그렇다쳐도 Write-Lock -> Read-Lock, Read-Lock -> Write-Lock이라도 해결하려고 나온 방법이 [[MVCC]]이다.

## 2PL Protocol 종류
### Conservative 2PL
모든 Lock을 취득한 뒤 트랜잭션 시작
DeadLock-Free DeadLock 이 발생되지 않음
실용적이지 않음 -> 모든 락을 못가져오는 경우도 생겨 실용적이지 않음

### Strict 2PL
Strict Schedule을 보장하는 2PL
recoverability 보장합니다.
write-lock commit/rollback 될 때 unlock

### Stong Strict 2PL
좀 더 강한 제약이 있는 Strict 2PL
read-lock , write-lock commit/rollback 될 때 unlock
S2PL 보다 구현이 쉽습니다.
Lock을 오래 잡고 있어야한다는 문제점이 있습니다.



