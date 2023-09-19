[[트랜잭션의 이상 현상]]을 모두 발생하지 않게 만들수 있지만 제약 사항이 많아져서 동시 가능한 트랜잭션 수가 줄어들어 결국 DB의 전체 처리량이 하락되기 때문에 Isolation Level에 따라 제약조건을 허용해 성능을 관리하는 방법

| 

이상한 현상을 허용하는 몇가지 레벨을 만들자
몇개를 허용하는지에 대해서 레벨이 다르다

Read uncommitted
Read committed
Repeatable Read
Serializable
