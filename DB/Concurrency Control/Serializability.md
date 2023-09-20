Serializability를 직역하면 직렬화 성질이라고 한다.
간단히 말하면 어느한 트랜잭션이 직렬화 된 트랜잭션처럼 될 수 있다고 보는 것이 Serializability이다.

Serializability을 알려면 [[Schedule]] 과 [[Conflict]]를 알아야 우리는 어떤 트랜잭션이 Serializability한 트랜잭션인지 알 수 있다.

위에서 볼 수 있다시피 어떤 Schedule이 Serial Schedule과 Equivalent 한 Schedule이면
그 Schedule이 Serializability한 스케줄이라고 볼 수 있다.

그리고 모든 Schedule이 Serializable한 Schedule로 만드는 것이 [[Concurrency Control]] 이다.
