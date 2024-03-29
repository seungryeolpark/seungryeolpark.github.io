---
title: 트랜잭션
categories: cs-database
---

[1. 트랜잭션 정의](#트랜잭션-정의)  
[2. 트랜잭션 특성](#트랜잭션-특성)  
[3. 트랜잭션 상태](#트랜잭션-상태)
[4. 트랜잭션 격리 수준](#트랜잭션-격리-수준)

## 트랜잭션 정의
+ 데이터베이스의 상태를 변화시키기 위해 수행하는 작업 단위
+ 논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는
원 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주는 기능

### 트랜잭션과 Lock
+ Lock 은 동시성을 제어하기 위한 기능이고 트랜잭션은 데이터의 정합성을 보장하기 위한 기능이다.
+ Lock 은 여러 커넥션에서 동시에 동일한 자원을 요청할 경우 순서대로 한 시점에는 하나의 커넥션만 변경할 수 있게 해주는 역할을 한다.
여기서 자원은 레코드나 테이블을 말한다.
+ 트랜잭션은 논리적인 작업 셋 중 하나의 쿼리가 있든 두 개 이상의 쿼리가 있든 관계없이 논리적인 작업 셋 자체가 100% 적용되거나
아무것도 적용되지 않아야 함을 보장하는 것이다.

## 트랜잭션 특성
+ 원자성(Atomicity)
    + 하나의 트랜잭션은 하나의 원자적 수행단위이다.
    + 트랜잭션은 완전히 수행되거나 전혀 수행되지 않아야 한다.(All or Nothing)
    + [회복 기법](#회복-기법)을 활용하여 보장한다.
+ 일관성(Consistency)
    + 트랜잭션을 완전히 실행하면 데이터베이스를 하나의 일관된 상태에서 또 다른 일관된 상태로 바꿔야 한다.
    + [동시성 제어](#동시성-제어)와 회복 기법을 활용하여 보장한다.
+ 고립성(Isolation)
    + 하나의 트랜잭션은 다른 트랜잭션들과는 독립적으로 실행되는 것처럼 보여야 한다.
    + 하나의 트랜잭션 실행시 동시에 실행중인 다른 트랜잭션의 간섭을 받으면 안된다.
    + 성능 관련 문제로 인해 유연하게 조절할 수 있도록 고립 수준을 단계별로 구분한다.
    + 동시성 제어를 활용하여 보장한다.
+ 지속성(Durability)
    + 성공적으로 완료된 트랜잭션의 결과는 시스템이 고장나더라도 영구적으로 반영되어야 한다.
    + 회복 기법을 활용하여 보장한다.

### 회복 기법
+ 트랜잭션 고장, 시스템 고장, 매체 고장 등으로 문제가 생겼을 때 회복하는 방법
+ 트랜잭션 고장 : Rollback 을 통한 undo
+ 시스템 고장 : [WAL](#WAL) 기법을 통한 [undo](#undo), [redo](#redo)
+ 매체 고장 : 백업

#### undo
+ 무언가를 되돌리는 것으로 사용자가 했던 작업을 반대로 작업합니다. 즉 사용자의 작업을 원상태로 돌립니다.

#### redo
+ 복구를 할때 사용자가 했던 작업을 그대로 다시 시작한다.

#### WAL
+ WAL 은 Write Ahead logging 의 약자이다.
+ 트랜잭션이 일어나기 전에 로그를 미리 기록하여 undo, redo 를 할 수 있도록 한다.

### 동시성 제어
+ 다양한 Lock 기법을 이용해서 동시성 제어
+ Locking 기법(2단계 로킹) : [Shared Lock](#Shared-Lock), [Exclusive Lock](#Exclusive-Lock) 을 활용한 제어
+ [TimeStamp 기법](#TimeStamp-기법) : 트랜잭션의 시작 시간을 이용해 제어
+ 낙관적(검증) 기법 : 각 트랜잭션마다 Copy 해서 작업 후 변동 부분을 검증하며 한꺼번에 반영
+ Locking, TimeStamp 등을 활용한 다중버전(MVCC) 기법(유명한 DBMS 에서 많이 사용) : 한 데이터 항목에 대해
여러개의 버전을 이용하여 이전 버전의 데이터를 활용한 처리방법

#### Shared Lock
+ 읽기 잠금이라고도 불린다.
+ 어떤 트랜잭션에서 데이터를 읽고자 할 때 다른 shared lock 은 허용이 되지만 exclusive lock 은 불가하다.
+ 리소스를 다른 사용자가 동시에 읽을 수 있게 하지만 변경은 불가하게 하는 것이다.
+ 어떤 자원에 shared lock 이 동시에 여러개 적용될 수 있다.
+ 어떤 자원에 shared lock 이 하나라도 걸려 있으면 exclusive lock 을 걸 수 없다.

#### Exclusive Lock
+ 쓰기 잠금이라도 불린다.
+ 어떤 트랜잭션에서 데이터를 변경하고자 할 때 해당 트랜잭션이 완료될 때까지 해당 테이블 혹은 레코드를
다른 트랜잭션에서 읽거나 쓰지 못하게 하기 위해 Exclusive lock 을 걸고 트랜잭션을 진행시키는 것이다.
+ exclusive lock 에 걸리면 shared lock 을 걸 수 없다.
+ exclusive lock 에 걸린 테이블, 레코드 등의 자원에 대해 다른 트랜잭션이 exclusive lock 을 걸 수 없다. 

#### TimeStamp 기법
+ 데이터베이스 병행제어를 위해 데이터 항목에 타임스탬프를 부여하여 직렬가능성을 보장하는 기법
+ 데이터에 접근하는 시간을 미리 정하여서 정해진 시간(Time Stamp)의 순서대로 데이터에 접근 하여 수행
+ [직렬가능성](#직렬가능성)을 보장한다.
+ [교착상태](#교착상태)가 발생하지 않는다.
+ [연쇄복귀](#연쇄복귀)를 초래할 수 있음
+ 동작과정
    + 트랜잭션에서 읽기 또는 쓰기 작업이 정상적으로 완료되면 타임스탬프를 기록한다.
    + 트랜잭션에서 읽기 또는 쓰기 작업을 수행하려고 하는 경우 타임스탬프를 확인한다.
    + 트랜잭션 수행 도중에 타임스탬프가 갱신된 경우 트랜잭션 작업을 중단한다.
    + ex) T1 이 11분 11초에 시작되어 11분 13초에 a 데이터를 업데이트 하려고 하는데
  a 데이터의 타임스탬프가 11분 12초인 경우 a 데이터는 다른 트랜잭션에 의해 읽히거나 쓰인 상태이므로
  현재 트랜잭션을 계속 실행하면 모순이 생기므로 ROLLBACK 한다.

#### 직렬가능성
+ 임계구역 내에서 다수의 트랜잭션들이 동시 활성화 될 경우 각 트랜잭션들은 원자적이기 때문에 여러 트랜잭션들을 병렬로 수행시키면
그 결과는 모든 트랜잭션들을 어떤 임의의 순서에 따라 하나씩 차례로 순차적으로 실행되는 것

#### 교착상태
+ 복수의 트랜잭션을 사용하다보면 교착상태가 일어날 수 있다.
+ 두 개 이상의 트랜잭션이 특정 자원(테이블 또는 행)의 잠금(Lock)을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면
  아무리 기다려도 상황이 바뀌지 않는 상태

#### 연쇄복귀
+ 두 트랜잭션이 동일한 데이터 내용을 접근할 때 발생
+ 한 트랜잭션이 데이터를 갱신한 다음 실패하여 Rollback 연산을 수행하는 과정에서
갱신과 Rollback 연산을 실행하고 있는 사이에 해당 데이터를 읽어서 사용할 때 발생할 수 있는 문제

## 트랜잭션 상태
![](https://user-images.githubusercontent.com/48073115/215404616-b45913ac-8f6a-4b0b-85ac-d7ddb5a610d1.png)
<div align="center">출처 : <a href="https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#index">트랜잭션의 상태</a></div>

+ Active
    + 트랜잭션의 활동 상태
    + 트랜잭션이 실행 중이며 동작 중인 상태를 말한다.
+ Failed
    + 트랜잭션 실패 상태
    + 트랜잭션이 더 이상 정상적으로 진행할 수 없는 상태를 말한다.
+ Partially Committed
    + 트랜잭션의 Commit 명령이 도착한 상태
    + 트랜잭션의 Commit 이전 Sql 문이 수행되고 Commit 만 남은 상태를 말한다.
+ Committed
    + 트랜잭션 완료 상태
    + 트랜잭션이 정상적으로 완료된 상태를 말한다.
+ Aborted
    + 트랜잭션이 취소 상태
    + 트랜잭션이 취소되고 트랜잭션 실행 이전 데이터로 돌아간 상태를 말한다.
+ Partially Committed 와 Committed 의 차이점
    + Commit 요청이 들어오면 상태는 Partial Committed 상태가 된다.
    + 이후 Commit 을 문제 없이 수행할 수 있으면 Committed 상태로 전이된다.
    + 만약 오류가 발생하면 Failed 상태가 된다.
    + Partial Committed 는 Commit 요청이 들어왔을 때를 말하며 Committed 는 Commit 을 정상적으로 완료한 상태를 말한다.

### 트랜잭션을 사용할 때 주의할 점
+ 트랜잭션은 꼭 필요한 최소의 코드에만 적용하는 것이 좋다. 즉 트랜잭션의 범위를 최소화하라는 의미다.
+ 일반적으로 데이터베이스 커넥션은 개수가 제한적이다. 그런데 각 단위 프로그램이 커넥션을 소유하는 시간이 길어진다면
사용가능한 여유 커넥션의 개수는 줄어들게 된다. 그러다 어느 순간에는 각 단위 프로그램에서 커넥션을 가져가기 위해
기다려야 하는 상황이 발생할 수도 있는 것이다.

## 트랜잭션 격리 수준
+ 트랜잭션이 안전하게 수행되기 위해 보장해야하는 ACID 중 고립성(Isolation)에 대하여
성능관련 문제로 인해 유연하게 조절할 수 있도록 격리 수준을 구분해 놓은 것
+ 격리 수준이 낮아지면 성능은 향상되지만 동시성 제어가 되지 않아 문제가 발생할 수 있음
+ 따라서 DBMS 는 다양한 Lock 기법을 활용해 트랜잭션 격리 수준에 맞춰 데이터 접근을 통제

### 격리 수준의 종류와 발생하는 동시성 제어 문제
|         격리수준         |**Lost Update**|**Dirty Read**|**Unrepeatable Read**|**Phantom Data Read|
|:--------------------:|:---:|:---:|:---:|:---:|
| **READ UNCOMMITED**  |X|O|O|O|
|  **READ COMMITED**   |X|X|O|O|
| **REPEATABLE READ**  |X|X|X|O|
|   **SERIALIZABLE**   |X|X|X|X|
+ 격리 수준을 높일 경우 성능이 저하되지만 동시성 제어로 인해 일관성이 보장됨
+ 격리 수준을 낮출 경우 성능은 향상되지만 동시성 제어에 문제가 생겨 일관성이 보장되지 않을 수 있음

#### READ UNCOMMITED
+ Isolation Level 0에 해당하며 기본적인 [Lost Update](#Lost-Update) 만 방지하는 상태
+ UPDATE 와 같이 갱신에 대하여 배타적 락을 설정하여 Lost Update 을 방지
+ 어떤 트랜잭션이 Commit, Rollback 으로 종료되지 않아도 다른 트랜잭션에서 데이터 참조 가능
+ SELECT 시 공유 락을 설정하거나 체크하지 않기 때문에 다른 트랜잭션이 갱신하면서 설정한 배타적 락을 무시하고
변경 중인 데이터를 참조(Dirty Read), 참조중인 데이터를 변경하는 문제(Unrepeatable Read), 
추가 삽입 되는 데이터 문제(Phantom Data Read) 등이 발생

#### READ COMMITED
+ Isolation Level 1에 해당하며 Level 0의 기능에 추가로 [Dirty Read](#Dirty-Read) 까지 방지하는 상태
+ SELECT 와 같이 읽기에 대하여 공유 락을 설정하여 Dirty Read 를 방지
+ 공유 락이 트랜잭션이 종료되는 Commit, Rollback 시점까지 유지되는 것이 아닌 SELECT 문이 처리되는 순간만 설정되므로
반복 읽기시 중간에 데이터의 갱신(Unrepeatable Read), 삽입(Phantom Data Read)에 의해 문제 발생

#### REPEATABLE READ
+ Isolation Level 2에 해당하며 Level 1의 기능에 추가로 [Unrepeatable Read](#Unrepeatable-Read) 까지 방지하는 상태
+ 공유 락, 배타적 락을 트랜잭션이 종료될 때 까지 유지하여 Unrepeatable Read 를 방지
+ 이 단계 부터 Locking 기법을 활용해도 Lock 이 많아지고, MVCC 기법을 활용해도 Undo 영역이 커지면서 성능이 떨어질 수 있음
+ 단 SELECT 나 UPDATE 등을 이용해 현재 존재하는 데이터에 대해서만 락을 설정하므로 SELECT 등을 이용해
범위 데이터를 처리 할 때 범위 내에 있으나 존재 하지 않아 락이 걸리지 않은 영역에 데이터가 추가로 삽입(Phantom Data Read)되면 문제 발생

#### SERIALIZABLE
+ Isolation Level 3에 해당하며 Level 2의 기능에 추가로 [Phantom Data Read](#Phantom-Data-Read) 까지 방지하는 상태
+ SELECT 가 이루어지는 범위(테이블)에 공유락을 설정하여 Phantom Data Read 를 방지
+ 최고 단계 고립 수준으로 일관성이 보장되지만 성능이 안좋음
+ 따라서 동시성이 중요한 DB 에서는 거의 사용하지 않고 낮은 단계의 격리 수준(일반적으로 READ COMMITED)과 함께
어플리케이션 단계에서 [낙관적 락](#낙관적-락), [비관적 락](#비관적-락) 등을 활용해 처리하는 경우가 많음

##### Lost Update
+ 하나의 트랜잭션이 갱신한 내용을 commit/rollback 하기 전에 다른 트랜잭션이 접근해 덮어씀으로써 갱신 했던 내용이 사라지는 것을 의미

##### Dirty Read
+ 하나의 트랜잭션이 갱신하려던 내용을 Rollback 하기 전에 다른 트랜잭션이 접근해 무효가 된 데이터를 읽는 것을 의미

##### Unrepeatable Read
+ 하나의 트랜잭션이 반복적으로 동일한 읽기 연산을 실행할 때 다른 트랜잭션의 갱신완료로 이전과 다른 결과가 나오는 것을 의미

##### Phantom Data Read
+ 하나의 트랜잭션이 범위데이터를 읽을 때 다른 트랜잭션의 데이터 삽입완료로 인해 이전에 없던 유령 데이터가 나타나는 것을 의미

##### 낙관적 락
+ 트랜잭션 대부분은 충돌이 발생하지 않는다고 가정하는 방법
+ 일단 충돌이 나는 것을 막지 않고 충돌이 난 것을 감지하면 그때 처리한다.
+ JPA 가 제공하는 Version 관리 기능을 사용한다.
+ DB 에서 LOCK 을 거는 것이 아닌 Application 단계에서 Lock 을 거는 것이다.
+ 장점
    + 충돌이 안난다는 가정하에 동시 요청에 대해서 처리 성능이 좋다.
+ 단점
    + 잦은 충돌이 일어나는 경우 롤백처리에 대한 비용이 많이 들어 오히려 성능에서 손해를 볼 수 있다.
    + 롤백 처리를 구현하는게 복잡할 수 있다.

##### 비관적 락
+ 트랜잭션 충돌이 발생한다는 것을 가정하고 먼저 Lock 을 거는 방법
+ 하나의 트랜잭션이 자원에 접근시 Lock 을 걸고 다른 트랜잭션이 접근하지 못하게 한다.
+ 데이터베이스에서 Shared Lock 이나 Exclusive Lock 을 겁니다.
+ 장점
    + 충돌이 자주 발생하는 환경에 대해서는 롤백의 횟수를 줄일 수 있으므로 성능에 유리하다.
    + 데이터 무결성을 보장하는 수준이 매우 높다.
+ 단점
    + 데이터 자체에 락을 걸어버리므로 동시성이 떨어져 성능 손해를 많이 보게 된다.
  특히 읽기가 많이 이루어지는 데이터베이스의 경우에는 손해가 더 두드러진다.
    + 서로 자원이 필요한 경우에 락이 걸려있으므로 데드락이 일어날 가능성이 있다.
