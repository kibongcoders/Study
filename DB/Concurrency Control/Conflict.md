Conflict  두개의 오퍼레이션이 3가지의 조건을 만족하면 Conflict하다라고 한다.
- 서로 다른 트랜잭션 소속 
  -> 같은 트랜잭션을 비교해봤자 어떻게 무결성을 지킬수 있을까?
- 같은 데이터에 접근 
  -> 같은 데이터의 오퍼레이션이여야 비교가 가능할것이다.
- 최소 하나는 write 오퍼레이션 순서는 상관이 없다 
  -> r-w, w-r, w-w

하나는 읽고 하나는 쓰는 Conflict를 read-write conflict라고 부른다.
하나는 쓰고 하나도 쓰는 Conflict를 write-write conflict라고 부른다.

Conflict  Operation은 순서가 바뀌면 결과도 바뀐다.
원래 w-r 였던 Conflict Operation이 r-w로 바뀐다면 당연히 결과도 바뀔수 밖에없다.
왜냐하면 2개가 다른 트랜잭션이여서 r-w로 바뀌게 된다면 반영이 되지 않을것이다.

Conflict Equivalent 두개의 스케줄이 2가지의 조건을 만족하면 Conflict Equivalent하다라고 한다.
- 두 스케줄은 같은 트랜잭션을 가져야함 
  -> 같은 트랜잭션을 가지지 않으면 비교를 왜 할까요?
- 어떤 Conflict Operation의 순서도 양쪽 스케줄 모두 동일하다. 
  -> 예를 들어 두개의 스케줄이 있는데
  하나의 스케줄이 w2(데이터)-r1(데이터) 이렇게 되어있으면 다른 스케줄도 w2(데이터)-r1(데이터)의 순서로 스케줄이 되어있어야한다. 만약 r1(데이터)-w2(데이터) 되어 있다면 이것은 Conflict Equivalent하지 않는다.
  (두 스케줄 사이의 모든 Conflict Operation이 모두 이 조건을 성립야한다.)

Conflict Serializable
Nonserial Schedule 과 Serial Schedule Conflict equivalent일 때 이것을 Conflict Serializable한다라고 합니다.

구현이슈 
트랜잭션을 할 때 마다 모두 비교해가며 Conflict Serializable 하지는 않는다.
하지만 Schedule Conflict Serializable 하도록 보장하는 프로토콜이 있다!