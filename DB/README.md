# Index

## Index란?

DB에서 select처리를 빠르게 하기 위하여 추가적인 디스크를 사용하여 색인을 하는것.

## Index를 어떤 컬럼에 거는가?

select를 시행할때 사용하는 join, where, orderby 절 구문에 사용되는 컬럼에 주로 index를 사용한다.

이때, Delete, Update, Insert등이 자주 일어나는 구문에 걸게되면, 오히려 index쓰기, 삭제 작업 때문에 시간이 더 걸리게 되니 주의하여야한다.

## Index의 장단점은?

### 장점

select의 속도를 향상시켜서 join, where, orderby등의 작업에서 시간을 단축시킬수있다

### 단점

Index는 디스크에 추가적으로 Index정보를 쓰기 때문에, 디스크 용량을 더 소모한다는 단점이 있고, Update, Delete, Update와 같은 작업을 수행할때, Index역시 바꿔줘야 하기에 자칫 잘못하면 오히려 더 많은 시간을 소모하게 되는 역효과가 날수있다.

## Index의 구조는?

### 해시 테이블

key-value로 이루어져 있는 자료구조다.
내부적으로 배열(버킷)을 사용하여 데이터를 저장하기에 빠른 검색소도가 특징이다.
다만 이때, = 연산이 빠른대신에 >,< 와 같은 부등호 연산은 탐색이 비효율적이다.

### B-tree

자식 노드가 항상 정렬이 되어있는 트리 자료구조이다.
루트에서 리프노드까지 항상 같은 깊이를 가지고 있다.
따라서, 어떤 값도 같은 결과를 얻을수있다.
즉, >,< 와 같은 부등호 연산의 속도가 빠르다.

### B+Tree

- B-Tree와는 다르게 브랜치 노드는 value의 정보가 없고 오로지 key값만 존재한다.
- 그렇기에 B-Tree보다 메모리를 적게 차지하지만, Value를 찾기위해선 리프 노드까지 이동해야 하는 단점이있다.
- 링크는 linked list구조로 되어있어서, 리프 순회가 더 빠르다.
- Full scan시에 B-Tree는 결국 모든 노드를 확인해야하지만, B+tree는 리프노드에
- 모든 value가 있기에, 리프노드만 순회하면된다. 즉, DB에 좀더 적합한 구조이다.

# Connection Pool

## Connection Pool이란?

DB와의 커넥션 객체를 미리 할당을 해서 Pool에 넣어둔다. 이후 DB연결 요청이 들어왔을때 Pool의 있는 Connection을 사용하고, 작업이 끝나면 반납하는방식. Connection을 잇는 작업을 중복으로 처리되지 않도록해서 성능을 올리는 방법.

## Connection Pool을 왜 쓰는가?

Connection Pool을 사용할경우, 연결이 되어있는 Connection이 이미 Pool에 있기 때문에, Connection을 매번 지어줄 필요없다.
따라서, 기존의 작업을 할때마다 conection을 맺고 끊는 과정을 줄여서 성능을 향상시킬수있다

## 실시간 통신과 Pool사용시의 차이가 무엇인가요?

실시간 통신은 DB에 작업을 할떄마다

1. DB connection을 만든다.
2. DB의 작업을 처리한다
3. DB와 Connection을 해제한다.
   의 과정을 거치지만, Pool을 사용하면 1번과 3번의 과정이 생략이되기에, 많은 request처리에서 성능이 더 좋다.

하지만, Connection도 객체기 때문에, 너무 많이 만들어 두게되면 메모리를 더 많이 소모해서 오히려 성능이 떨어지기에 주의해야한다.

# 트랜잭션

## 트랜잭션이란?

여러개의 작업을 하나로 묶은 단위이다.
트랜잭션은 모두 실행되거나, 모두 실행되지 않습니다.
이와 같은 트랜잭션에는 ACID라는 특징이 있습니다.

## ACID원칙이란?

트랙잭션의 특성 4가지의 앞글자를 따서 만든 약어입니다.

- A - Atomic(원자성): 트랜잭션의 작업이 전부실행되거나, 전부 실행이 되지 않거나를 보장하는것을 말합니다.
  (ex: 송금만 되고, 기록이 되지 않는 일이 발생하지 않는다. )
- C - Consistency(일관성): 트랜잭션이 성공적으로 완료되면 일관적인 DB상태를 유지하는것을 말합니다. (ex : int가 string이 되지않는다)
- I - Isolation(고립성):트랜잭션 실행중에는 다른 트랜잭션이 끼어들지 못합니다. (ex: 송금 트랜잭션중 삭제 트랜잭션이 끼어들지 못한다.)
- D - Durability(영구성): 트랜잭션이 실행뒤에 commit이된 사항은 영구적으로 반영되는것을 말합니다.

# 샤딩

## 샤딩이란?

관계형 데이터베이스에서 대량의 데이터를 처리하기 위해서 데이터를 파티셔닝하는 기술이다. (partitioning->분할함)

간단하게 예를 들면, 전 세계의 고객 데이터를 저장하는 대형 데이터베이스를 분산한다고 할때,

미국 고객의 경우는 샤드A, 아시아 고객의 경우는 샤드B, 유럽 고객의 경우는 샤드C로 분할해서 저장할수 있다.

## 샤딩하기전에 고려해야하는점은?

1. 좀더 고스펙의 컴퓨터를 사용한다.

2. Read(Select) 명령이 많다면 Cache나 Database Replication을 적용한다.

3. Table의 일부 Column만 사용한다면 수직 분할(Vertically Partitioning)을 사용한다 / Cold & Hot data를 사용한다.

## 샤딩의 단점은?

프로그래밍적, 운영적인 복잡도가 높아진다.
즉, 샤딩을 시작하기 전에 샤딩을 피하거나 지연시킬 수 있는 방법을 찾는게 우선이다.

# 리플리케이션

## 리플리케이션이란?

- 두 개의 이상의 DBMS 시스템을 `Mater / Slave`로 나눠서 동일한 데이터를 저장하는 방식이다.

## **방식**

![https://nesoy.github.io/assets/posts/20180216/2.png](https://nesoy.github.io/assets/posts/20180216/2.png)

`Master DBMS`에는 데이터의 수정사항을 반영만하고 Replication을 하여 `Slave DBMS`에 실제 데이터를 복사한다.

## 리플리케이션하는 방법은?

**로그기반 복제(Binary Log)**

- Statement Based : `SQL문장`을 복사하여 진행
  - issue : SQL에 따라 결과가 달라지는 경우(Timestamp, UUID, …)
- Row Based : SQL에 따라 변경된 `Row 라인`만 기록하는 방식
  - issue : 데이터가 많이 변경된 경우 데이터 커질 수 밖에 없다.
- Mixed : 기본적으로 Statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.

## 리플리케이션의 장점은?

**Replication 장점.**

![https://nesoy.github.io/assets/posts/20180216/3.png](https://nesoy.github.io/assets/posts/20180216/3.png)

언급했던 것처럼 Query의 대부분은 `Select`가 차지하고 있다.

이 부분의 부하를 낮추기 위해 많은 `Slave Database`를 생성하게 된다면 `Read(Select)` 성능 향상 효과를 얻을 수 있다.

`Master Database` 영향없이 로그를 분석할 수 있다.

## hot/cold data란?

- Hot data: 자주 사용되는 데이터

- Cold data: 드물게 사용되거나 아예 사용되지 않는 데이터

# Lock

## Lock이란?

동시성 제어를 위하여 내부적으로 적용되는 로직이다.

- readLock : read lock이 걸린 데이터를 A가 read를 하고있을떄 B가 write할수 없다.
- writeLock : write lock이 걸린 데이터를 A가 write하고 있을떄 B가 read할수없다.

## lock의 단점

동시에 처리가 안되기 때문에 속도가 느리다.
이와같은 단점을 해결하기 위하여 MVCC가 나왔다.

# MVCC

## MVCC란?

다중 버전 동시성 제어라는 뜻이다. 기존 lock의 문제점을 해결하기위해 탄생했다.

트랜잭션이 a라는 값을 30에서 50으로 update하는 중에 있다고 가정해보자.

이떄, 또 다른 트랜잭션이 a라는 값을 select하게 된다면, 50이 아닌 30이라는 값이 보이게된다.

이것이 바로 MVCC에 의한 버전 관리이다.

이것이 가능한 이유는 데이터를 다중으로 유지하고 있어서, 트랜잭션이 commit되기 전까지는 undo영역에 있기 때문이다.

## MVCC의 특징

- lock에 비하여 속도가 빠르다
- 데이터가 지속적으로 쌓이기 때문에 이를 처리해줄 시스템이 추가적으로 필요하다.

## Mysql과 postgresql의 접근 차이

- 첫번째 접근법 **PostgreSQL**, Interbase, SQL Server 의 DB 에 해당하는 방식입니다.

데이터베이스 내에 다중 버전의 데이터를 저장합니다.

더이상 필요하지 않을 때 데이터를 정리합니다.

데이터베이스 내에 다중 버전의 데이터가 저장되기 때문에,

데이터가 많아지는 형식으로 되어 파일 사이즈가 계속 증가하게 됩니다.

그리고 기존의 데이터는 삭제 표시가 생기게 됩니다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpBYdM%2Fbtq1R7pOEDx%2Fuekd8im9RoDzvPQbrIaXu1%2Fimg.png)

- Oracle, **MySQL DB** 에 해당하는 방식입니다.

최신 버전의 데이터만 데이터베이스 내에 저장합니다.

이전의 데이터는 언두를 이용하여 데이터를 저장합니다.

데이터가 갱신되면 Undo 영역에는 이전 데이터블럭들값과 당시의 SCN가(System Commit Number) 저장되어 있습니다.

SELECT 조회 시 새로운 최신 SCN 의 값을 가지며 이 SCN 값 이전의 블록들 값을 읽게 됩니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx95D7%2Fbtq1MpZhdb1%2FgkneYgTOrWbqev4Moegm60%2Fimg.png)
