여러개 트랜잭션이 하나의 데이터에 접근할 때 어떻게 무결성과 일관성을 보장하며 트랜잭션을 제어하는 방법

Concurrency Control의 목적은 모든 [[Schedule]]이 [[Conflict]] Serializable으로 만드는 것이다.
Concurrency Control은 Isolation과 연관이 있고
너무 엄격하게 Isolation 하게 된다면 성능이 떨어지므로 Isolation의 레벨을 두어서 유연하게 하는 것이
Isolation Level이라고 한다.