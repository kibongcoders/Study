트랜잭션은 DB에서 수행되는 작업의 논리적 단위다.
논리적인 이유 여러 SQL 문들을 단일 작업으로 묶어서 나눠질 수 없게 만든 것이 트랜잭션이다.

트랜잭션 사용방법 Mysql 기준

mysql > Start Transaction
mysql > SQL 문으로 작동
mysql > COMMIT
중간에 문제가 있다면 ROLLBACK

만약 문제가 있었다면 알아서 rollback 해준다.
mysql은 autocommit이 기본값이다. SELECT @@AUTOCOMMIT 으로 확인 가능
SET autocommit=0; 으로 autocommit 비활성화 가능

rollback 이란 트랜잭션 시작 이후의 작업을 모두 취소하고 트랜잭션 이전 상태를 되돌리고 종료

DB에 대한 커넥션이 먼저다.
Auto Commit을 false로 해줘야 트랜잭션을 관리 할 수 있겠죠
Auto Commit은 결국 true 해줘야 한다.

@Transactional
애노테이션을 통해 트랜잭션 관련 코드를 숨길 수 있다.
없으면 다 구현해야함

ACID

원자성 (Atomicity): 트랜잭션에 속한 모든 작업은 전부 성공하거나 실패해야 합니다. 
어떤 작업이 실패하면 다른 모든 작업도 롤백되어야 합니다. ALL OR NOTHING
commit시 저장과 rollback의 이전 상태로 돌리는 건 DBMS가 해야할일
언제 commit과 언제 rollback을 해야하는지는 개발자가 해야할일
에러가 나도 rollback을 안할 수 도 있다.

일관성 (Consistency): 트랜잭션이 시작하기 전과 끝난 후에 데이터베이스는 일관된 상태여야 합니다. 트랜잭션이 일부 작업을 수행한 후에 데이터베이스가 일관성을 유지하지 못한다면 트랜잭션은 롤백되어야 합니다.
모든 트랜잭션이 DB의 규칙에 따라야 일관되게 동작을 하니까 이거를 일관성이라고 표현
DB의 정의된 룰들을 위반하면 롤백해야 한다. DBMS는 commit 전에 확인하고 알려준다.
어플리케이션에서 룰을 잘 깨지 않도록 잘 관리 해야한다.

고립성 (Isolation): 동시에 여러 트랜잭션이 수행될 때 각각의 트랜잭션은 다른 트랜잭션에 영향을 주지 않도록 격리되어야 합니다. 다른 트랜잭션에서 수행 중인 작업은 다른 트랜잭션에게 보이지 않아야 합니다.
혼자 실행 되는 것처럼 동작하게 만들어야 한다.
isolation level이 높으면 높을 수록 중요하다. isolation level이 높을수록 성능은 떨어진다.
concurrency control

영존성 : 트랜잭션이 성공적으로 완료되면 그 결과는 영구적으로 저장되어야 합니다. 시스템 장애 또는 전원 장애가 발생해도 데이터는 보존되어야 합니다.
하드 디스크에 저장됨을 의미한다.







