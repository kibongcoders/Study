여러 트랜잭션들이 동시에 실행 될 때 각 트랜잭션에 속한 오퍼레이션들의 실행 순서

r1(K) - w1(K) - r2(H) - w2(K) - c2 - r1(H) - w1(H) - c1
r숫자(객체) 몇번째 읽기
w숫자(객체) 몇번째 읽기
c숫자 몇번째 commit

serial schedule 트랜잭션들이 겹치지 않고 한번에 하나씩 실행되는 스케줄
무결성은 유지되나 좋은 성능을 유지 할 수 없는 방법

nonserial schedule 트랜잭션들이 겹처서 실행 되는 스케줄
트랜잭션들이 겹처서 실행되기 때문에 동시성이 높아져서 같은 시간 동안 더 많은 트랜잭션들을 처리할 수 있다.
단점은 무결성 보장이 되지 않는다.

좋은 성능을 유지하면서 무결성을 유지하는 방법
serial schedule과 동일한 nonserial schedule을 실행하면 된다!?!?

schedule이 동일한다

Conflict 두개의 오퍼레이션에 해당
만족 조건
서로 다른 트랜잭션 소속
같은 데이터에 접근
최소 하나는 write 오퍼레이션

하나는 읽고 하나는 쓰고 read-write conflict
write write conflict
conflict operation은 순서가 바뀌면 결과도 바뀐다.

Conflict equivalent
두 조건을 만족
두 스케줄은 같은 트랜잭션을 가져야함
어떤 conflict operation의 순서도 양쪽 스케줄 모두 동일하다.

Conflict Serializable
시리얼 스케줄과 Conflict equivalent일 때

구현이슈 
할 때 마다 Conflict Serializable 하지는 않는다.
하도록 보장하는 프로토콜이 있다!!
