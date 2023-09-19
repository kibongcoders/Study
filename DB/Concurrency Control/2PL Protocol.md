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

