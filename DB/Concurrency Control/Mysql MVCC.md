---
tags: MVCC
---
Mysql에서 [[MVCC]]는
SS 2PL Protocol과 매우 비슷한 형태로 진행된다.
Mysql MVCC에서는 Repeatable Read만으로는 Lost Update를 해결할 수 없다.
(Mysql Repeatable Read 에서는 Phantom Read - 없던 데이터가 생김이 일어나지 않는다)
Mysql에서는 Locking Read를 통해 Lost Update를 해결하고 Locking Read는 read를 하여도 write-lock을 취득하여 가장 최근의 commit된 데이터를 읽는다.

Serializable Level
tx의 모든 평범한 select문은 암묵적으로 Shared Lock으로 동작한다.

