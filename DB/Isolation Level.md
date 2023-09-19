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
특정 시점에서의 현상

EX) tx1 : x가 y에 40을 이체한다. tx2: y에 100을 입금한다. x=50, y=50
r1(x) => 50, w1(x) => 10, r2(y) => 50, w2(y) => 150, c2, r1(y) => 50, w1(y) => 90
SNAPSHOT ISOLATION은 해당 스케주