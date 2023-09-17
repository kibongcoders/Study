여러 트랜잭션들이 동시에 실행 될 때 각 트랜잭션에 속한 오퍼레이션들의 실행 순서

스케줄은 이렇게 표현할 수 있다.
r1(K) - w1(K) - r2(H) - w2(K) - c2 - r1(H) - w1(H) - c1
read, write, commit의 앞자를 따서 표현하고 하나 하나를 오퍼레이션이라고 부른다.

## Schedule 종류
스케줄은 2가지 종류가 있다.
**Serial Schedule** 트랜잭션들이 겹치지 않고 한번에 한 트랜잭션씩 실행되는 스케줄
무결성은 유지되나 좋은 성능을 유지 할 수 없는 방법

**Nonserial Schedule** 트랜잭션들이 겹처서 실행 되는 스케줄
트랜잭션들이 겹처서 실행되기 때문에 동시성이 높아져서 같은 시간 동안 더 많은 트랜잭션들을 처리할 수 있다.
단점은 무결성 보장이 되지 않을 수 있다.

좋은 성능을 유지하면서 무결성을 유지하는 방법
Serial schedule과 동일한 Nonserial schedule을 실행하면 된다!?!?

이것을 해결하려면 [[Conflict]] Serialzable한 스케줄을 찾으면 된다.

Unrecoverable Schedule
트랜잭션 1하고 트랜잭션 2하고 맞물려서 동시에 진행될 때
트랜잭션 2가 끝나기전에 트랜잭션1이 Commit을 한 뒤 트랜잭션 2에서 문제가 생기는 경우
이미 Commit을 했기 때문에 트랜잭션의 영존성으로 트랜잭션 1에 로직에서 회복이 불가능한 경우가 생긴다.
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c1 - c2 을 한경우
r1(H) 에서 이미 w2(H)한 값을 읽었기 때문에 c1을 한뒤 에러가 일어나면 ROLLBACK 을 해도 이전상태로 회복이 불가능해 질 수 있다.
이런 Schedule은 DBMS에서 허용하면 안된다.

Recoverable Schedule
의존성이 있는 트랜잭션이 종료 될 때까지 기다려야한다.
스케줄 내에서 어떤 트랜잭션도 트랜잭션이 읽은 해당 write 트랜잭션이  먼저 commit/rollback 전까지는 commit 하지 않는 경우
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c2 - c1 으로 변경 되어야 하며
만약 c2하기 전에 문제가 생긴다면 모두다 abort(무효화) 시켜야 한다.

Cascadeless Schedule
r1(K) - w1(K) - r2(H) - w2(H) - r1(H) - w1(H) - c2 - c1
이 상황에서 c2가 문제가 생겨 ROLLBACK 하게 되면 모두 다 ROLLBACK 해야하는데 연쇄적으로 ROLLBACK 하게 되면 비용이 많이 발생하게 된다.
이것을 해결하기위해
스케줄내에 COMMIT 되지 않은 트랜잭션의 write를 그 어떤 트랜잭션도 read하지 않는 경우 
Cascadeless Schedule라고 한다.
r1(K) - w1(K) - r2(H) - w2(H) - c2 - r1(H) - w1(H) - c1
해당 순서를 이렇게 변경한다면 Cascadeless Schedule한 스케줄 이라고 볼 수 있다.

하지만 Cascadeless Schedule에는 이슈가 있을 수 있는데
read만 안하고 write만 두번한다면 Cascadeless Schedule은 적용되지만 이 때 에러가 날 시 모두 다 ROLLBACK 되어 나중에 write한 데이터가 없어지게 되는 장애가 발생하게 된다.


Strict Schedule
스케줄내에 COMMIT 되지 않은 트랜잭션의 write를 그 어떤 트랜잭션도 read 또는 write 하지 않는 경우 
r1(K) - w1(K) - r2(H) - w2(H) - c2 - r1(H) - w1(H) - c1
아니 근데 



