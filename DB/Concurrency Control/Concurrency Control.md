여러개 트랜잭션이 하나의 데이터에 접근할 때 어떻게 무결성과 일관성을 보장하고 성능까지 보장하며 트랜잭션을 제어하는 방법
Concurrency Control은 [[Serializability]]와 [[Recoverability]]를 제공합니다. 
이러한 속성들로 동시성을 관리하고 데이터의 일관성을 보장합니다.

## Concurrency Control 과 Isolation
Concurrency Control은 Isolation과 연관이 있고
너무 엄격하게 Isolation 하게 된다면 성능이 떨어지므로 Isolation의 레벨을 두어서 유연하게 하는 것이 [[Isolation Level]]이라고 합니다.

Concurrency Control 구현 방법
Concurrency Control은 [[DB Lock]]과 [[MVCC]]으로 구현이 가능합니다.