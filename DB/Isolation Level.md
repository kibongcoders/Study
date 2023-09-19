[[트랜잭션의 이상 현상]]을 모두 발생하지 않게 만들수 있지만 제약 사항이 많아져서 동시 가능한 트랜잭션 수가 줄어들어 결국 DB의 전체 처리량이 하락되기 때문에 Isolation Level에 따라 제약조건을 허용해 성능을 관리하는 방법

| Isolation Level | Dirty Read | Non-repeatable Read | Phantom Read |
| ----------------| ---------- | --------------------| ------------ |
| Read Uncommitted|     O      | O| O |
| Read Committed  |X | O | O
| Repeatable Read |X | X | O |
| Serializable    |X| X | X |

이 내용을 비판하는 내용이 있는데
standard SQL 92 isolation level에 대한 비판

1. 세가지 이상 현상의 정의가 모호하다.
2. 이상 현상은 세가지 외에도 더 있다.
3. 상업적인 DBMS에서 사용하는 방법을 반영해서 isolation level를 구분하지 않았다.

해당 내용을 비판하면서 소개하는 레벨이 SNAPSHOT ISOLATION 이다.

## SNAPSHOT ISOLATION
MVCC의 종류 중 하나
Concurrency Control의 구현 방식에 따라서 정의된 레벨이다.
tx 시작 전에 commit 된 데이터만 보이게 된다.
그리고 먼저 commit된 것이 반영된다.

EX) tx1 : x가 y에 40을 이체한다. tx2: y에 100을 입금한다. x=50, y=50
r1(x) => 50, w1(x) => 10, r2(y) => 50, w2(y) => 150, c2, r1(y) => 50, w1(y) => 90

SNAPSHOT ISOLATION은 해당 스케줄로 동작하게 되는데 여기서 SNAPSHOT은 특정 시점에서의 현상을 뜻한다. 

tx1의 SNAPSHOT은 tx1이 시작할 때를 기준으로 데이터를 임시 관리하고 있다. 즉 x=50, y=50이던 기준이다.
w1(x)를 할 때에는 tx1의 스냅샷에는 x=10, y=50으로 저장이 되게 된다. 

w2(y) 할 때 tx2의 스냅샷에는 y=50이 저장되어 있던 데이터를 150으로 변경한 뒤 c2에서 commit하게 된다. 여기서 tx1의 스냅샷에는 영향을 미치지 않는다.

그 뒤 r1(y) 오퍼레이션이 작동하게 되면 스냅샷안에 있는 데이터는 트랜잭션이 시작했던 기준인 x=50, y=50인 기준으로 데이터를 읽게 된다.
따라서 w1(y)은 150이 아닌 50의 데이터에서 40을 더해줘 90으로 변경되고 스냅샷에서는 x=10, y=90으로 변경된다.

이 때 c1을 하려고 할 때 y의 대한 값을 보면 tx1, tx2둘 다 y의 데이터를 변경하게 되므로 먼저 commit 된 c2만 변경되고 c1는 abort 하게 된다.

