---
tags: MVCC, concurrency_control
---
MVCC(Multiversion Concurrency Control) 트랜잭션이 동시적으로 하나의 데이터를 진행할 때스냅샷을 통해 하나의 데이터가 여러 버전으로 관리되는 것을 뜻한다.

MVCC 특징
데이터를 읽을 때 특정 시점 기준으로 가장 최근에 commit된 데이터만 읽는다.
데이터 변화(write) 이력을 관리한다.
read와 write는 서로를 block하지 않는다.

commit된 후 데이터를 어느 기준으로 가져올까?
[[Isolation Level]] 에 따라 다르게 가져온다.
Read Committed : read하는 시간을 기준으로 그 전에 commit된 데이터를 읽는다.
Repeatable Read : tx 시작 시간 기준으로 그 전에 commit된 데이터를 읽는다.
여기서 tx 시작 시간은 처음 tx 시작할 때 이거나 tx에서 최초의 read가 일어나는 때를 기준으로 삼는다. 이 기준은 각 RDBMS에 따라 다르다.




