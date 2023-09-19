---
tags: 2PL Protocol
---
2PL Protocol(2Phase Protocol) 은 2개의 단계가 있는 프로토콜이다.
locking operation 과 unlock operation로 단계를 나누고 트랜잭션에서 모든 locking operation이 unlock operation보다 먼저 수행되도록 하는 것이 

Conservative 2PL
모든 lock을 취득한 뒤 트랜잭션 시작
DeadLock-Free DeadLock 이 발생되지 않음
실용적이지 않음 -> 모든 락을 못가져오는 경우도 생겨 실용적이지 않음

Strict Schedule을 보장하는 2PL
recoverability 보장
write-lock commit/rollback 될 때 반환

Stong Strict 2PL
strict schedule 에서 
read-lock , write-lock commit/rollback 될 때 반환
S2PL 보다 구현이 쉽다.
Lock을 오래 잡고 있어야함

2PL로 구현을 하면 문제점이 있다
Read-Lock과 Write-Lock을 통해서 구현하기 때문에

|  | Read-Lock | Write-Lock |
| --- | --- | --- |
| Read-Lock | O | X |
| Write-Lock | X | X |

해당 표와 같이 Read-Lock 에서 Read-Lock만 허용해 주기 때문에 전체적인 처리량이 좋지 않다.
Write-Lock -> Write-Lock은 그렇다쳐도 Write-Lock -> Read-Lock, Read-Lock -> Write-Lock이라도 해결하려고 나온 방법이 MVCC이다



