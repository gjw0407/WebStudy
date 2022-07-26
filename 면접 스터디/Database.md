# Database

# 데이터베이스 특징

1. 데이터의 독립성
    1. 물리적 독립성 : 사이즈를 늘리거나 파일을 늘려도 관련된 응용 프로그램을 수정할 필요 없음
    2. 논리적 독립성 : 논리적인 구조로 다양한  응용 프로그램의 논리적 요구 만족
2. 데이터의 무결성
    1. 데이터의 정확성, 일관성, 유효성 유지
    2. 개체 무결성(PK), 참조 무결성(FK), 도메인 무결성(주민번호에 문자 들어가는 경우), Null 무결성, 고유 무결성(Unique), 키 무결성(하나의 키 존재), 관계 무결성
3. 데이터의 보안성
4. 데이터의 일관성
5. 데이터 중복 최소화

<aside>
💡 쿼리 튜닝은 랜덤 I/O 자체를 줄여주는 것이 목적

</aside>

# Index

칼럼의 값과 해당 레코드가 저장된 주소를 키와 값의 쌍으로 인덱스로 생성

추가 자료구조로, 삽입 시 약간의 딜레이를 유발하고 추가 디스크 공간을 요구하지만 검색 속도 면에서 훨씬 우월

SELECT 쿼리 문장의 WHERE 조건절에 사용되는 칼럼이라고 전부 인덱스로 생성하면 데이터 저장 성능이 떨어지고 인덱스의 크기가 비대해져서 오히려 역효과

INSERT, UPDATE, DELETE 가 자주 발생하지 않으며, 규모가 큰 테이블에서 사용하면 유리

## 자료구조

1. B+-Tree
    1. 부등호 사용시 용이
2. Hash 인덱스
    1. 검색 속도 O(1)로 줄일 수 있음
    2. 등호 연산 → 부등호 사용시 문제

### B-Tree

- 외부 기억장치는 블럭단위로 파일 I/O → 입출력의 비용은 파일의 크기와 상관없이 동일
    - 하나의 블럭에 여러 데이터들을 동시에 저장 → 블럭을 효율적으로 사용
- O(logN)의 검색 성능
- leaf node는 같은 레벨
- root node는 자신이 leaf node가 되지 않는 이상 적어도 2개 이상의 자식을 가짐
- root node가 아닌 노드들은 적어도 M/2개의 자식 노드를 가지고 있음
- 탐색을 위해 노드를 찾아서 이동해야 함

### B+Tree

- 같은 레벨의 모든 키값들이 정렬
- 같은 레벨의 Sibling node는 연결리스트 형태 → 순차 검색에 용이
- leaf node가 아닌 자료 → 인덱스 노드 → 포인터 주소 존재
- leaf node 자료 → 데이터 노드 → 데이터 존재
- 키 값은 중복 가능
- 데이터 검색 위해 leaf node까지 내려가야 함

![Untitled](https://user-images.githubusercontent.com/61227459/198881775-7947f2ca-f3a7-48e3-84d7-b998fc804aec.png)

## Cluster Index (Primary Index)

- PK에 대해서만 적용
- PK 값에 의해 레코드의 저장  위치가 결정
    - PK 변경 → 레코드의 물리적인저장 위치 변경
- 테이블 당 하나만 생성 가능
- 물리적으로 행을 재배열하며, 검색 속도가 빠름
- 삽입, 삭제, 갱신이 느림

## Non-cluster Index (Secondary Index)

- 여러 개 생성 가능 (Max 249)
- 인덱스 페이지만 정렬되고 레코드 데이터는 정렬되지 않음
- 리프 페이지는 데이터 포인터를 가르켜 검색 느림
- 삽입, 갱신, 삭제는 빠름

# 정규화

<aside>
💡 함수적 종속성 : 애트리뷰트 데이터들의 의미와 애트리뷰트들 간의 상호 관계로부터 유도되는 제약조건의 일종 → **DB에서 속성들간 종속 관계**

</aside>

## 정규화 조건

- 분해의 대상인 분해 집합은 **무손실 조인**을 보장
- 분해 집합은 **함수적 종속성**을 보존

## 제 1 정규형

1. 각 컬럼이 하나의 속성만을 가져야 한다
2. 하나의 컬럼은 같은 종류나 타입(type)의 값을 가져야 한다
3. 각 컬럼이 유일한(unique) 이름을 가져야 한다
4. 칼럼의 순서가 상관없어야 한다

## 제 2 정규형

1. 제 1 정규형을 만족해야 한다
2. 모든 컬럼이 부분적 종속이 없어야 한다 → 모든 컬럼이 완전 함수 종속을 만족

<aside>
💡 부분적 종속 : 기본키 중에 특정 컬럼에만 종속

</aside>

## 제 3 정규형

1. 제 2 정규형을 만족해야 한다
2. **이행적 함수 종속**을 삭제한다
    1. X→Y이고 Y→Z 이면 X→Z 이다. 이 때 Z는 X에 이행적 함수 종속

## BCNF(Boyce-Codd) 정규형

1. 제 3 정규형을 만족해야 한다
2. 모든 결정자가 후보 키 집합에 속해야 함

## 단점

릴레이션의 분해로 인해 릴레이션 간의 연산(JOIN 연산)이 많아짐

결정자에 의해 동일한 의미의 일반 속성이 하나의 테이블로 집약되므로 한 테이블의 데이터 용량이 최소화되는 효과 있음

정규화된 테이블은 데이터를 처리할 때 속도가 빨라질 수도 있고 느려질 수도 있는 특성을 가짐

### ****어떠한 상황에서 정규화를 진행해야 하는가? 단점에 대한 대응책은?****

SQL 문장에서 조인이 많이 발생하여  성능저하가 나타나는 경우에 반정규화를 적용

**반정규화 대상**

1. 자주 사용되는 테이블에 액세스하는 프로세스의 수가 가장 많고, 항상 일정한 범위만을 조회
2. 테이블에 대량 데이터가 있고 대량의 범위를 자주 처리
3. 테이블에 지나치게 조인을 많이 사용하게 되어 데이터를 조회하는 것이 기술적으로 어려움

# Transaction

> 데이터베이스의 상태를 변환시키는, 하나의 논리적 기능을 수행하기 위한 단위
> 

## 트랜잭션 vs Lock

Lock : 동시성 제어

트랜잭션 : 데이터의 정합성 보장

## ACID

### Atomicity (원자성)

- 트랜잭션의 연산은 모두 반영되거나, 모두 반영되지 않아야 한다. 즉, 트랜잭션 내의 모든 명령은 수행되어야 하며 어느 하나라도 오류가 발생하면 트랜잭션 전부가 취소

### Consistency (일관성)

- 트랜잭션이 성공적으로 완료되면 언제나 일관성 있는 데이터베이스 상태로 변환된다. 시스템의 고정 요소는 트랜잭션 실행 전후로 같아야 한다.

### Isolation (독립성)

- 둘 이상의 트랜잭션이 병행되는 경우 하나의 트랜잭션 수행 도중 다른 트랜잭션이 끼어들거나 참조할 수 없다. 독립성은 총 4개의 단계로 구분

### Durability (영속성)

- 성공된 트랜잭션은 시스템 오류가 발생되어도 영구적으로 반영되어야 한다.

## 트랜잭션 상태

![Untitled 1](https://user-images.githubusercontent.com/61227459/198881777-dfa18dbc-198b-4273-a474-d6205a8f3c36.png)

## 트랜재션 격리 수준 (Isolation Level)

응답성을 높이기 위해 locking 범위를 줄일 수 없어 효율적인 locking을 위해 격리 수준 고려

![Untitled 2](https://user-images.githubusercontent.com/61227459/198881768-26451fbb-5c07-44eb-9760-452717cf2eba.png)

1. Dirty read : 트랜잭션 처리된 작업의 중간 결과를 볼 수 있는 현상
    1. commit 되지 않은 정보를 볼 수 있는 현상
2. Non-repeatable read : 같은 row를 두번 읽는데 각각 다른 데이터를 읽는 경우
3. Phantom read : 한 트랜잭션 안에서 첫 쿼리와 두번째 쿼리 수행 결과가 다른 경우
    1. 외부에 동시에 실행중인 트랜잭션의 insert 작업에 의해 발생하는 read 현상
    2. 결과 범위에 속하지 않은 레코드가 외부 작업에 의해 있을수도 없으수도 있음

### READ UNCOMMITED

- 이전 트랜잭션의 결과가 commit되지 않더라도, update 된 다른 값을 불러온다 (Dirty read) → Rollback 되는 데이터일 수도 있는데 우선 읽어오기 때문에, 보통 권장되지 않는다.

### READ COMMITED

- 보통 일반적으로 사용되는 격리 수준. Undo 영역에 기존 값을 저장해둔 후 저장된 값을 불러온다
- 하지만 하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 실행해도 커밋 전이냐 후냐에 따라서 값이 다르기 때문에, Non-repeatable read 문제를 야기한다.

### REPEATABLE READ

- Undo 영역에 기존 값을 저장해둔 후 저장된 값을 불러오는 것은 동일
- 하지만 해당 트랜잭션이 마무리 되기 전 다른 트랜잭션이 Commit 하더라도 기존의 Undo 영역에 저장된 값을 참조하도록 하여 Non-repeatable read 문제를 해결한다
- 하지만, 다른 트랜잭션이 INSERT나 DELETE 연산을 수행한 경우 새로운 값이 생기거나 기존 값이 사라지는 Phantom read 현상이 발생하기도 한다.

### SERIALIZABLE

- S-lock (Shared lock) - 읽기 잠금
- X-lock (Exclusive lock) - 쓰기 잠금
- 성능이 저하된다는 문제점도 있고, 두 트랜잭션이 각각 S-lock 건 후 상대방에게 update를 위해 X-lock을 시도하는 경우 Dead-lock이 발생하기도 함.

## 교착상태

> 두 개 이상의 트랜잭션이 특정 자원의 잠금을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않은 상태
> 

### 빈도 낮추는 방법

- 트랜잭션 자주 커밋
- 정해진 순서로 테이블 접근
- 읽기 잠금 획득 (SELECT ~ FOR UPDATE) 피함
- 복수 행을 복수의 연결에서 순서 없이 갱신하면 교착상태가 발생하기 쉬움
    - 테이블 단위의 잠금을 획득해 갱신을 직렬화 하면 동시성은 떨어지지만 교착상태를 회피할 수 있음

# ****Statement vs PreparedStatement****

## PreparedStatement

- 쿼리를 수행하기 전에 이미 쿼리가 컴파일 되어 있으며, 반복 수행의 경우 프리 컴파일된 쿼리를 통해 수행이 이루어짐
- 쿼리 자체에 조건이 들어가는 `dynamic sql` 사용 → 퍼포먼스 저하
- 파싱 타임을 줄여줌 (전체의 1/10)
- `SQL Injection` 등의 문제를 보완해주는 `PreparedStatement`를 사용하는 것이 옳다

## Statement

- 변수를 설정하고 바인딩하는 `static sql` 사용

# 클러스터

> 데이터베이스 서버를 여러 개 두는 개념과 같음
>

![Untitled 3](https://user-images.githubusercontent.com/61227459/198881770-33ab514c-6499-484f-9549-823b7c264866.png)

- 하나의 DB 서버로는 고가용성(HA), 병렬처리와 같은 작업 수행이 어려움
- 다중과 구조를 Active-Active 혹은 Active-Standby 형태로 구성해 성능 높임
    - 데이터 동기화 필요

## Active-Active

- 컴포넌트를 동시에 가동
- 성능이 높고 하나가 다운되어도 다른 서버로 동작 가능
- 비용이 높고 가끔 병목 현상 발생

## Active-Standby

- Active 다운되면 Standby 활용
- Standby : Cold-Standby, Warm-Standby Hot-Standby로 나뉨
    - Hot Standby: Standby 장비 가동 후, 즉시 사용 가능
    - Warm Standby: Standby 장비 가동 후, 이용이 가능하게 하기 위해서 준비가 필요
    - Cold Standby: 평소 Standby 장비를 정지시켜두며, 필요시 직접 켜서 구성
    - 일반적인 경우, Hot - Warm - Cold 순으로 Failover 소요시간이 짧다
- 비용은 낮지만 서버 장애 발생으로 교체 시 딜레이 및 DB 중단 시간 발생

## Replication

> 데이터베이스 서버 부하 분산 및  백업을 위해 이용
> 
- Master Server와 Slave Server가 존재
    - Insert, Update, Delete와 같은 작업은 Master Server로 요청
    - Select는 Slave Server로 데이터를 요청
- 비동기 방식으로, Master Server에서 변경 사항을 Slave에게 알림
- 부하 분산으로 성능 개선이 확실하게 가능
    - 읽기의 비중이 크기 때문
- 데이터 동기화가 보장되지 않으므로 일관성 있는 데이터를 얻지 못할 수도 있으며, Master Server가 다운되면 복구가 어려울 수 있음

## **다중화 구조에서 데이터 정합성을 얻는 방법**

### Shared Disk

- 하나의 스토리지를 공유
- 정합성에 대해 특별히 문제될 것은 없음
- Shared Disk 자체를 다중화해야 하기 때문에 비용 측면에서 부담

![Untitled 4](https://user-images.githubusercontent.com/61227459/198881772-33c660fa-6bb5-49a7-9998-a7a3ff1b9996.png)

### Shared Nothing

- 스토리지 간 통신을 하여 데이터 정합성을 확보
- 레플리케이션이라 불림

![Untitled 5](https://user-images.githubusercontent.com/61227459/198881773-24ffeb0c-f4b0-4e5f-829b-5aa8db8dc55c.png)

# RDBMS & NoSQL

## RDBMS

> 2차원의 행과 열로 데이터의 관계를 관리하는 데이터베이스
> 

### 장점

스키마에 맞추어 데이터를 관리하기 때문에 데이터의 정합성을 보장

### 단점

시스템이 커질 수록 쿼리가 복잡해지고 성능이 저하되며, 수평적 확장이 어려움

## NoSQL

> 관계형 데이터 모델을 **지양**하며 대량의 분산된 데이터를 저장하고 조회하는 데 특화되었으며 스키마 없이 사용 가능하거나 느슨한 스키마를 제공하는 저장소
> 
- 대량의 데이터를 빠르게 처리하기 위해 메모리에 임시 저장하고 응답하는 등의 방법을 사용
- 동적인 스케일 아웃을 지원하기도 하며, 가용성을 위하여 데이터 복제 등 가능

### 장점

스키마 없이 Key-Value 형태로 데이터를 관리하여 좀 더 자유롭게 데이터를 관리

### 단점

중복된 데이터가 추가 가능하여, 이에 대한 관리가 필요

### 종류

1. Key-Value Model
    1. 고속 읽기와 쓰기에 최적화
    2. 하나의 서비스 요청에 다수의 데이터 조회 및 수정 연산이 발생하면 트랜잭션 처리가 불가능하여 데이터 정합성을 보장할 수 없음
2. Document Model
    1. 하나의 키에 하나의 구조화된 문서를 저장하고 조회
    2. 키는 문서에 대한 ID 로 표현
    3. 문서를 컬렉션으로 관리하며 문서 저장과 동시에 문서 ID 에 대한 인덱스를 생성
    4. B 트리 인덱스를 사용하여 2 차 인덱스를 생성
    5. 중앙 집중식 로그 저장, 타임라인 저장, 통계 정보 저장 등에 사용
3. Column Model
    1. 하나의 키에 여러 개의 컬럼 이름과 컬럼 값의 쌍으로 이루어진 데이터를 저장하고 조회
    2. 쓰기와 읽기 중에 쓰기에 더 특화
    3. 저장의 기본 단위는 컬럼으로 컬럼은 컬럼 이름과 컬럼 값, 타임스탬프로 구성
    4. 이러한 컬럼들의 집합이 로우(Row)이며, 로우키(Row key)는 각 로우를 유일하게 식별하는 값
    5. 이러한 로우들의 집합은 키 스페이스(Key Space)
    6. 채팅 내용 저장, 실시간 분석을 위한 데이터 저장소 등의 서비스 구현에 적합

## CAP 이론

1. 일관성(Consistency)
    1. 동시성 또는 동일성
    2. 다중 클라이언트에서 같은 시간에 조회하는 데이터는 항상 동일한 데이터임을 보증
    3. 최종적으로 일관성이 유지된다고 하여 최종 일관성 또는 궁극적 일관성을 지원한다 함
    4. 데이터의 저장 결과를 클라이언트로 응답하기 전에 모든 노드에 데이터를 저장하는 동기식 방법이 있음
        1. 느린 응답시간을 보이지만 데이터의 정합성을 보장
    5. 메모리나 임시 파일에 기록하고 클라이언트에 먼저 응답한 다음, 특정 이벤트 또는 프로세스를 사용하여 노드로 데이터를 동기화하는 비동기식 방법
        1. 빠른 응답시간을 보인다는 장점이 있지만, 쓰기 노드에 장애가 발생하였을 경우 데이터가 손실될 있음
2. 가용성(Availability)
    1. 모든 클라이언트의 읽기와 쓰기 요청에 대하여 항상 응답이 가능해야 함
    2. NoSQL은 클러스터 내에서 몇 개의 노드가 망가져도 정상적인 서비스 가능
    3. 몇몇 NoSQL은 가용성 보장을 위해 Replication 사용
3. 네트워크 분할  서용성 (Partition tolerance)
    1. 지역적으로 분할 된 네트워크 환경에서 네트워크가 단절되거나 데이터의 유실이 일어나도 각 지역 내의 시스템은 정상적으로 동작해야 함