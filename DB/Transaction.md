트랜잭션은 DB에서 수행되는 작업의 논리적 단위다.
논리적인 이유 여러 SQL 문들을 단일 작업으로 묶어서 나눠질 수 없게 만든 것이 트랜잭션이다.

트랜잭션 사용방법 Mysql 기준

mysql > Start Transaction
mysql > SQL 문으로 작동
mysql > COMMIT

만약 문제가 있었다면 알아서 rollback 해준다.
mysql은 autocommit이 기본값이다. SELECT @@AUTOCOMMIT 으로 확인 가능
SET autocommit=0; 으로 autocommit 비활성화 가능
