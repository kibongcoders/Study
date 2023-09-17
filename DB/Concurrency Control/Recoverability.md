트랜잭션의 스케줄이 장애 또는 오류로 실패한 경우 일관되게 이전 상태로 복원할 수 있는지를 나타내는 개념

## Unrecoverable Schedule
트랜잭션 1하고 트랜잭션 2하고 맞물려서 동시에 진행될 때
트랜잭션 2가 끝나기전에 트랜잭션1이 Commit을 한 뒤 트랜잭션 2에서 문제가 생기는 경우
이미 Commit을 했기 때문에 트랜잭션의 영존성으로 트랜잭션 1에 로직에서 회복이 불가능한 경우가 생긴다.

EX)
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c1 - c2 을 한경우
r1(H) 에서 이미 w2(H)한 값을 읽었기 때문에 c1을 한뒤 에러가 일어나면 ROLLBACK 을 해도 이전상태로 회복이 불가능해 질 수 있다.
이런 Schedule은 DBMS에서 허용하면 안된다.

## Recoverable Schedule
**<font color="#4f81bd">의존성이 있는 트랜잭션이 종료 될 때까지 기다려야한다.</font>**
스케줄 내에서 어떤 트랜잭션도 트랜잭션이 읽은 해당 write 트랜잭션이  먼저 commit/rollback 전까지는 commit 하지 않아야 한다.

EX)
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c2 - c1 으로 변경 되어야 하며
<font color="#4f81bd">tx1이 tx2를 의존하고 있기에 w2(H)가 commit하기 전까지는 c1을 COMMIT 해서는 안된다.</font>
만약 c2하기 전에 문제가 생긴다면 모두다 abort(무효화) 시켜야 한다.

## Cascadeless Schedule
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c2 - c1
<font color="#4f81bd">이 상황에서 c2가 문제가 생겨 ROLLBACK 하게 되면 모두 다 ROLLBACK 해야하는데 연쇄적으로 </font><font color="#4f81bd">ROLLBACK 하게 되면 비용이 많이 발생하게 된다.</font>
이것을 해결하기위해
<font color="#4f81bd">스케줄내에 COMMIT 되지 않은 트랜잭션의 write를 그 어떤 트랜잭션도 read하지 않는 경우 </font>
Cascadeless Schedule라고 한다.
r1(K) - w1(K) - r2(H) - w2(H) - c2 - r1(H) - w1(H) - c1
이렇게 변경한다면 tx2만 변경하면 된다.
해당 순서를 이렇게 변경한다면 Cascadeless Schedule한 스케줄 이라고 볼 수 있다.

하지만 Cascadeless Schedule에는 이슈가 있을 수 있는데
read만 안하고 write만 두번한다면 Cascadeless Schedule은 적용되지만 이 때 에러가 날 시 모두 다 ROLLBACK 되어 나중에 write한 데이터가 없어지게 되는 장애가 발생하게 된다.
w1(K) - w2(K) - c1 - c2의 경우 c1에서 에러가 나면 모두 롤백되어 모든 데이터가 저장되지 않는 경우가 생긴다.

## Strict Schedule
스케줄내에 COMMIT 되지 않은 트랜잭션의 write를 그 어떤 트랜잭션도 read 또는 write 하지 않는 경우 
r1(K) - w1(K) - r2(H) - w2(H) - c2 - r1(H) - w1(H) - c1
Strict Schedule이 Serial Schedule과 같아 질 수 도 있다. 하지만 모든 경우에서 그런건 아니다.
w1(K) - c1 - w2(K) - c2
이것이 Serial Schedule과 같아 보인다. 하지만 r1(K) - w1(K) - r2(H) - w2(H) - c2 - r1(H) - w1(H) - c1의 예제를 보더라도 그렇지 않다는 것을 알 수있다.