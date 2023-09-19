---
tags:
  - DB
  - Lock
---
여러 트랜잭션들이 동시에 같은 데이터에 접근하는 것을 DB LOCK 이라고 한다.

## 왜 DB LOCK이 있어야하는가?
DB LOCK을 하지않는다면 한개의 데이터에 여러 트랜잭션이 Write를 하는 경우 결과가 이상하게 출력 될 수 있습니다.
따라서, 데이터의 일관성과 무결성을 위해 DB LOCK이 필요합니다.

## Lock 종류
Lock은 2개의 종류가 있습니다.

- Read-Lock : 읽기 전용 Lock 입니다.  데이터를 읽기만 할 수 있습니다.
  Read-Lock을 가져온 경우 같은 데이터를 다른 트랜잭션에서 Read-Lock을 가져올순 있지만 Write-Lock은 가져올 수 없습니다.
- Write-Lock : 쓰기 전용 Lock 입니다. 데이터를 읽기/쓰기할 수 있습니다.
  Write Lock을 가져온 경우 같은 데이터를 다른 트랜잭션에서 Read-Lock과 Write-Lock 모두 가져올 수 없습니다.

해당 내용을 이런 표로 나타낼수 있습니다.

|  | Read-Lock | Write-Lock |
| --- | --- | --- |
| Read-Lock | O | X |
| Write-Lock | X | X |

Read-Lock을 먼저 한다음에 Write-Lock이 오는 경우 Read-Lock을 먼저 UNLOCK 한 다음에 Write-Lock이 실행되게 됩니다.

## DB Lock의 문제점
Lock이 모든것을 해결해 줄 것 같지만 Lock을 사용한다고 하여도 결국 문제는 발생합니다.
Ex) x = 100, y=200, tx1 : x = x+y, tx2 : y=x+y 를 한다고 했을 때

Serial Schedule을 tx1, tx2순으로 실행하면
read-lock(y), r1(y) , unlock(y), write-lock(x), r1(x), w1(x) => x+y 300, unlock(x),
read-lock(x), r2(x) , unlock(x), write-lock(y), r2(y), w1(y) => x+y 500, unlock(y)
으로 스케줄이 실행이 되고
x = 300 y = 500의 결과가 나오게 됩니다.

tx2, tx1순으로 실행하면
read-lock(x), r2(x) , unlock(x), write-lock(y), r2(y), w1(y) => x+y 300, unlock(y),
read-lock(y), r1(y) , unlock(y), write-lock(x), r1(x), w1(x) => x+y 400, unlock(x)
으로 스케줄이 실행이 되고
x = 400 y = 300의 결과가 나오게 됩니다.

NonSerial Schedule로 실행하면

| tx1 | tx2 |
| --- | --- |
| read-lock(y) | |
| r1(y) 200 | |
| unlock(y) | |
| |read-lock(x) |
| write-lock(x) | |
| | r2(x) 100 |
| | unlock(x)
| r1(x) 100  | |
| w1(x) 300 |  |
| unlock(x) | |
| | write-lock(y) |
| |r2(y) |
| |w2(y) 300|
| | unlock(y) |

위 과정으로 스케줄이 진행되기 된다. 위 결과로 보면 x=300, y=300인 이상한 결과를 볼 수 있는데
이 과정에서의 문제점의 이유는 unlock(y) -> read-lock(x) -> write-lock(x) 에서 업데이트 되기 전에 x를 읽었기 때문에 발생되는 문제입니다.
그래서 해당문제를 정상적으로 변경하면

| tx1 | tx2 |
| --- | --- |
| read-lock(y) | |
| r1(y) 200 | |
| write-lock(x) | |
| |read-lock(x) |
| unlock(y) | |
| r1(x) 100  | |
| w1(x) 300 |  |
| unlock(x) | |
| |r2(x) 300 |
| | write-lock(y) |
| | unlock(x) |
| |r2(y) |
| |w2(y) 400|
| | unlock(y) |

해당 방식으로 제대로된 결과를 도출할 수 있습니다.
이렇게 변경된 결과를 [[2PL Protocol]](2 Phase Protocol)이라고 부릅니다.