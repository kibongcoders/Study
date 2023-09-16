트랜잭션은 DB에서 수행되는 작업의 논리적 단위다.
논리적인 이유 여러 SQL 문들을 단일 작업으로 묶어서 나눠질 수 없게 만든 것이 트랜잭션이다.

## 트랜잭션 사용방법 Mysql 기준

mysql > Start Transaction
mysql > SQL 문으로 작동
mysql > COMMIT
중간에 문제가 있다면 ROLLBACK

만약 SQL 문 실행 도중 문제가 있었다면 알아서 DBMS에서 ROLLBACK 해줍니다.
mysql은 autocommit이 기본값이다. SELECT @@AUTOCOMMIT 으로 확인 가능
SET autocommit=0; 으로 autocommit 비활성화 가능

## 어플리케이션에서 트랜잭션 관리방법(SPRING 기준)

- 먼저 DB에 대한 커넥션을 해야한다.
- Auto Commit을 false로 해줘야 트랜잭션을 관리 할 수 있다. 만약 하지않으면 SQL문 실행 할 때마다 Commit을 해줄 것이다.
- transaction.begin(), transaction.commit(), transaction.rollback()을 통해 트랜잭션을 관리 가능하다.
- 이것을 감추고 싶다면 @Transactional 애노테이션을 사용하면 된다. (Spring AOP) 아니면 다 코드로 구현해야한다.
- 마지막으로 Auto Commit을 true로 다시 변경해줘야한다.

## ACID
트랜잭션의 속성을 4가지로 정의할 수 있다.

### 원자성 (Atomicity)

트랜잭션에 속한 모든 작업은 전부 성공하거나 실패해야 합니다.  어떤 작업이 실패하면 다른 모든 작업도 롤백되어야 합니다. ALL OR NOTHING

### 일관성 (Consistency)

트랜잭션이 시작하기 전과 끝난 후에 데이터베이스는 일관된 상태여야 합니다. 
트랜잭션이 일부 작업을 수행한 후에 데이터베이스가 일관성을 유지하지 못한다면 트랜잭션은 롤백되어야 합니다.
(모든 트랜잭션이 DB의 규칙에 따라야 일관되게 동작을 하니까 이거를 일관성이라고 표현)
DB의 정의된 룰들을 위반하면 롤백해야 한다. DBMS는 commit 전에 확인하고 알려준다.
어플리케이션에서 룰을 잘 깨지 않도록 잘 관리 해야한다.

### 고립성 (Isolation)

동시에 여러 트랜잭션이 수행될 때 각각의 트랜잭션은 다른 트랜잭션에 영향을 주지 않도록 격리되어야 합니다. 
다른 트랜잭션에서 수행 중인 작업은 다른 트랜잭션에게 보이지 않아야하고 혼자 실행 되는 것처럼 동작하게 만들어야 한다.
isolation level이 높으면 높을 수록 중요하다. isolation level이 높을수록 성능은 떨어진다.
여러개의 트랜잭션이 들어 올 때 성능 관리 및 무결성을 보장하는 것이 [[Concurrency Control]]이라고 한다.

### 영존성 (Durability)

트랜잭션이 성공적으로 완료되면 그 결과는 영구적으로 저장되어야 합니다.
시스템 장애 또는 전원 장애가 발생해도 데이터는 보존되어야 합니다.
하드 디스크에 저장됨을 의미한다.







